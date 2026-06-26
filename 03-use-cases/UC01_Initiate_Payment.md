# UC01 — Initiate QR Payment

**Use Case ID:** UC01  
**Use Case Name:** Initiate QR Payment  
**Actor:** Merchant, Customer  
**Priority:** High  
**Status:** Draft

---

## Brief Description
Merchant displays a VietQR code. Customer scans and confirms payment. System validates and processes the transaction.

---

## Preconditions
- Merchant has completed onboarding and KYC verification
- Merchant account is active
- Customer has a VietQR-compatible banking app with sufficient balance

---

## Main Flow (Happy Path)

| Step | Actor | Action |
|------|-------|--------|
| 1 | Merchant | Opens POS app, selects "Receive Payment", enters amount |
| 2 | System | Generates VietQR code (TCCS 04:2024/NHNN standard), displays on screen |
| 3 | Customer | Opens banking app, scans QR code |
| 4 | System | Decodes QR, displays merchant name, amount for customer confirmation |
| 5 | Customer | Reviews details and confirms payment |
| 6 | System | Sends OTP to customer's registered phone number |
| 7 | Customer | Enters 6-digit OTP within 90 seconds |
| 8 | System | Validates OTP, processes payment via Napas 247 |
| 9 | System | Sends confirmation to both merchant and customer |
| 10 | Merchant | Sees "Payment Received" notification with transaction ID |

---

## Alternative Flows

### A1 — Customer cancels at confirmation step
- Step 5a: Customer cancels → System cancels transaction → Merchant sees "Payment Cancelled"

### A2 — OTP expired
- Step 7a: Customer does not enter OTP within 90 seconds → System invalidates OTP → Returns to Step 6 (max 3 retries)

---

## Exception Flows

### E1 — Insufficient balance
- Step 8a: Customer balance insufficient → System returns error → Customer notified "Insufficient funds" → Transaction cancelled

### E2 — Payment session timeout
- After 5 minutes from QR generation → System invalidates QR → Merchant must generate new QR

### E3 — Network error
- Step 8b: Network failure → System retries 3 times → If all fail → Transaction rolled back → Both parties notified

---

## Postconditions
- **Success:** Payment recorded, merchant balance updated, transaction ID generated
- **Failure:** No funds transferred, transaction logged as failed with error code

---

## Business Rules
- QR code must comply with TCCS 04:2024/NHNN (VietQR standard)
- Payment session expires after 5 minutes (FR-07)
- OTP valid for 90 seconds, max 3 retry attempts (FR-09)
- Transactions > 10M VND require biometric authentication per QĐ 2345/QĐ-NHNN
- All transaction logs retained minimum 5 years per AML requirements

---

## Related Requirements
- FR-01: QR code generation
- FR-05: OTP verification
- FR-07: Payment session timeout
- FR-09: Transaction confirmation
- NFR-01: QR generation < 2 seconds
- NFR-03: 99.9% system availability