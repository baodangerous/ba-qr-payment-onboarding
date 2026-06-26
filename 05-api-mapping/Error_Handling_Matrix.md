# Error Handling Matrix — QR Payment System

**Project:** QR Payment Merchant Onboarding  
**Author:** Nguyen Le Bao Dang  
**Last updated:** June 2025

---

## HTTP Status Code Reference

| HTTP Code | Category | When Used |
|-----------|----------|-----------|
| 200 | Success | Request successful, data returned |
| 201 | Created | Resource created successfully |
| 400 | Bad Request | Invalid input, business rule violation |
| 401 | Unauthorized | Invalid or expired token |
| 403 | Forbidden | Valid token but insufficient permissions |
| 404 | Not Found | Resource does not exist |
| 409 | Conflict | Duplicate resource or state conflict |
| 422 | Unprocessable | Valid format but fails validation |
| 429 | Too Many Requests | Rate limit or max retry exceeded |
| 500 | Server Error | Unexpected system error |
| 503 | Unavailable | Downstream service unavailable |

---

## Full Error Code Matrix

| Error Code | HTTP | Endpoint | User Message (VI) | Retry? |
|------------|------|----------|-------------------|--------|
| INVALID_TAX_CODE | 400 | /register | "Mã số thuế không hợp lệ" | No |
| DUPLICATE_REGISTRATION | 409 | /register | "Tài khoản đã tồn tại" | No |
| BIOMETRIC_MISMATCH | 400 | /kyc | "Xác thực sinh trắc học thất bại" | Yes (max 3) |
| CHIP_READ_FAILURE | 400 | /kyc | "Không đọc được chip CCCD. Vui lòng thử lại" | Yes (max 3) |
| MAX_ATTEMPTS_EXCEEDED | 429 | /kyc | "Đã vượt quá số lần thử. Vui lòng liên hệ hỗ trợ" | No |
| INVALID_AMOUNT | 400 | /qr/generate | "Số tiền không hợp lệ" | No |
| MERCHANT_NOT_ACTIVE | 403 | /qr/generate | "Tài khoản merchant chưa được kích hoạt" | No |
| QR_SERVICE_UNAVAILABLE | 503 | /qr/generate | "Dịch vụ tạm thời gián đoạn. Vui lòng thử lại" | Yes (auto, max 1) |
| QR_EXPIRED | 400 | /payments/initiate | "Mã QR đã hết hạn. Vui lòng yêu cầu mã mới" | No |
| INSUFFICIENT_BALANCE | 400 | /payments/initiate | "Số dư không đủ để thực hiện giao dịch" | No |
| INVALID_OTP | 400 | /verify-otp | "Mã OTP không đúng. Vui lòng thử lại" | Yes (max 3) |
| OTP_EXPIRED | 400 | /verify-otp | "Mã OTP đã hết hạn. Giao dịch bị huỷ" | No — restart |
| MAX_OTP_ATTEMPTS | 429 | /verify-otp | "Đã nhập sai OTP 3 lần. Giao dịch bị huỷ" | No — restart |
| REFUND_WINDOW_EXPIRED | 400 | /refunds | "Giao dịch đã quá 30 ngày, không thể hoàn tiền" | No |
| DUPLICATE_REFUND | 409 | /refunds | "Yêu cầu hoàn tiền đã tồn tại cho giao dịch này" | No |

---

## Retry Logic

| Scenario | Auto Retry | Max Attempts | Backoff |
|----------|-----------|--------------|---------|
| QR generation service unavailable | Yes | 1 | 2 seconds |
| OTP verification failure | No (user action) | 3 | N/A |
| eKYC chip read failure | No (user action) | 3 | N/A |
| Network timeout | Yes | 3 | Exponential (1s, 2s, 4s) |
| Payment processing via Napas | Yes | 2 | 3 seconds |

---

## Error Response Format (Standard)

All error responses follow this structure:

```json
{
  "error_code": "QR_EXPIRED",
  "http_status": 400,
  "message": "QR session has expired",
  "user_message_vi": "Mã QR đã hết hạn. Vui lòng yêu cầu mã mới",
  "timestamp": "2025-06-01T10:05:01Z",
  "request_id": "REQ_20250601_001"
}
```