# API Mapping Table — QR Payment Merchant Onboarding

**Project:** QR Payment Merchant Onboarding System  
**Author:** Nguyen Le Bao Dang  
**Standard:** REST API · JSON · VietQR TCCS 04:2024/NHNN  
**Last updated:** June 2025

---

## Endpoint Overview

| # | Endpoint | Method | Description | Related FR |
|---|----------|--------|-------------|------------|
| 1 | /api/v1/merchants/register | POST | Merchant registration | FR-01 |
| 2 | /api/v1/merchants/kyc | POST | Submit eKYC verification | FR-02 |
| 3 | /api/v1/merchants/{id}/status | GET | Check merchant account status | FR-03 |
| 4 | /api/v1/qr/generate | POST | Generate VietQR code | FR-04 |
| 5 | /api/v1/payments/initiate | POST | Initiate payment session | FR-05 |
| 6 | /api/v1/payments/{id}/verify-otp | POST | Verify OTP for payment | FR-09 |
| 7 | /api/v1/payments/{id}/status | GET | Query payment status | FR-10 |
| 8 | /api/v1/refunds | POST | Initiate refund request | FR-15 |

---

## Endpoint 1 — Merchant Registration

**POST** `/api/v1/merchants/register`

**Request:**
```json
{
  "business_name": "Nguyen Coffee Shop",
  "business_type": "F&B",
  "tax_code": "0123456789",
  "address": "123 Nguyen Hue, Q1, HCMC",
  "phone": "0901234567",
  "email": "owner@nguyencoffee.vn",
  "bank_account": "1234567890",
  "bank_code": "VCB"
}
```

**Response — 201 Created:**
```json
{
  "merchant_id": "MCH_20250601_001",
  "status": "PENDING_KYC",
  "message": "Registration successful. Please complete eKYC verification.",
  "kyc_token": "eyJhbGciOiJIUzI1NiJ9..."
}
```

**Error Responses:**

| Code | Error | Description |
|------|-------|-------------|
| 400 | INVALID_TAX_CODE | Tax code format invalid or not found |
| 409 | DUPLICATE_REGISTRATION | Tax code or phone already registered |
| 422 | MISSING_REQUIRED_FIELD | One or more required fields missing |

---

## Endpoint 2 — eKYC Verification

**POST** `/api/v1/merchants/kyc`

**Request:**
```json
{
  "merchant_id": "MCH_20250601_001",
  "kyc_token": "eyJhbGciOiJIUzI1NiJ9...",
  "cccd_number": "079xxxxxxxxx",
  "cccd_chip_data": "<base64_encoded_chip_data>",
  "biometric_data": "<base64_encoded_facial_scan>",
  "kyc_method": "EKYC_CHIP"
}
```

**Response — 200 OK:**
```json
{
  "kyc_status": "VERIFIED",
  "verification_id": "KYC_20250601_001",
  "merchant_status": "PENDING_APPROVAL",
  "message": "eKYC verified. Account pending Ops approval (within 24 hours)."
}
```

**Error Responses:**

| Code | Error | Description |
|------|-------|-------------|
| 400 | BIOMETRIC_MISMATCH | Face does not match CCCD photo |
| 400 | CHIP_READ_FAILURE | NFC chip data unreadable |
| 401 | INVALID_KYC_TOKEN | Token expired or invalid |
| 429 | MAX_ATTEMPTS_EXCEEDED | Max 3 KYC attempts reached |

---

## Endpoint 4 — Generate VietQR Code

**POST** `/api/v1/qr/generate`

**Request:**
```json
{
  "merchant_id": "MCH_20250601_001",
  "amount": 150000,
  "currency": "VND",
  "description": "Coffee x2",
  "reference_id": "ORD_20250601_001"
}
```

**Response — 201 Created:**
```json
{
  "qr_id": "QR_20250601_001",
  "qr_image_base64": "<base64_encoded_png>",
  "qr_payload": "00020101021238...",
  "amount": 150000,
  "currency": "VND",
  "expires_at": "2025-06-01T10:05:00Z",
  "session_timeout_seconds": 300
}
```

**Error Responses:**

| Code | Error | Description |
|------|-------|-------------|
| 400 | INVALID_AMOUNT | Amount must be > 0 |
| 403 | MERCHANT_NOT_ACTIVE | Merchant account not activated |
| 503 | QR_SERVICE_UNAVAILABLE | QR generation service timeout |

---

## Endpoint 5 — Initiate Payment

**POST** `/api/v1/payments/initiate`

**Request:**
```json
{
  "qr_id": "QR_20250601_001",
  "customer_bank_code": "VCB",
  "customer_account": "9876543210"
}
```

**Response — 200 OK:**
```json
{
  "payment_id": "PAY_20250601_001",
  "status": "PENDING_OTP",
  "amount": 150000,
  "merchant_name": "Nguyen Coffee Shop",
  "otp_expires_in_seconds": 90,
  "message": "OTP sent to registered phone number"
}
```

**Error Responses:**

| Code | Error | Description |
|------|-------|-------------|
| 400 | QR_EXPIRED | QR session has expired (> 5 min) |
| 400 | INSUFFICIENT_BALANCE | Customer balance insufficient |
| 404 | QR_NOT_FOUND | QR ID does not exist |

---

## Endpoint 6 — Verify OTP

**POST** `/api/v1/payments/{payment_id}/verify-otp`

**Request:**
```json
{
  "otp": "123456"
}
```

**Response — 200 OK:**
```json
{
  "payment_id": "PAY_20250601_001",
  "status": "COMPLETED",
  "transaction_id": "TXN_20250601_001",
  "amount": 150000,
  "completed_at": "2025-06-01T10:02:35Z",
  "message": "Payment successful"
}
```

**Error Responses:**

| Code | Error | Description |
|------|-------|-------------|
| 400 | INVALID_OTP | OTP does not match |
| 400 | OTP_EXPIRED | OTP expired after 90 seconds |
| 429 | MAX_OTP_ATTEMPTS | Max 3 OTP attempts reached — payment cancelled |

---

## Endpoint 7 — Query Payment Status

**GET** `/api/v1/payments/{payment_id}/status`

**Response — 200 OK:**
```json
{
  "payment_id": "PAY_20250601_001",
  "status": "COMPLETED",
  "amount": 150000,
  "merchant_id": "MCH_20250601_001",
  "transaction_id": "TXN_20250601_001",
  "created_at": "2025-06-01T10:00:00Z",
  "completed_at": "2025-06-01T10:02:35Z"
}
```

**Payment Status Values:**

| Status | Description |
|--------|-------------|
| PENDING_OTP | Awaiting OTP verification |
| PROCESSING | Payment being processed via Napas |
| COMPLETED | Payment successful |
| FAILED | Payment failed |
| EXPIRED | Session or OTP expired |
| CANCELLED | Cancelled by customer or system |

---

## Endpoint 8 — Initiate Refund

**POST** `/api/v1/refunds`

**Request:**
```json
{
  "transaction_id": "TXN_20250601_001",
  "refund_amount": 150000,
  "reason": "Customer requested cancellation",
  "dispute_type": "CUSTOMER_REQUEST"
}
```

**Response — 201 Created:**
```json
{
  "refund_id": "REF_20250601_001",
  "status": "PENDING_APPROVAL",
  "refund_amount": 150000,
  "message": "Refund request submitted. Ops team will review within 3 business days."
}
```

**Error Responses:**

| Code | Error | Description |
|------|-------|-------------|
| 400 | REFUND_WINDOW_EXPIRED | Transaction older than 30 days |
| 400 | AMOUNT_EXCEEDS_ORIGINAL | Refund amount > original transaction |
| 409 | DUPLICATE_REFUND | Refund already exists for this transaction |