# UC04 — Merchant Registration & eKYC

**Use Case ID:** UC04  
**Use Case Name:** Merchant Registration & eKYC Verification  
**Actor:** Merchant, System, KYC System  
**Priority:** High  
**Status:** Draft

---

## Brief Description
New merchant registers on the platform and completes eKYC verification via CCCD chip scan and facial biometric — per Thông tư 17/2024/TT-NHNN.

---

## Preconditions
- Merchant has a valid CCCD (Căn cước công dân) with chip
- Merchant has a smartphone with NFC capability
- Merchant has not previously registered

---

## Main Flow

| Step | Actor | Action |
|------|-------|--------|
| 1 | Merchant | Downloads app, selects "Register as Merchant" |
| 2 | Merchant | Enters business information (name, address, tax code) |
| 3 | System | Validates business info format, checks for duplicates |
| 4 | Merchant | Scans CCCD chip via NFC |
| 5 | KYC System | Reads chip data, verifies authenticity |
| 6 | Merchant | Completes facial biometric scan |
| 7 | KYC System | Matches face with CCCD photo, returns verification result |
| 8 | System | Creates merchant account, sends confirmation email/SMS |
| 9 | Ops Team | Reviews and approves merchant account within 24 hours |
| 10 | System | Activates account, notifies merchant |

---

## Alternative Flows

### A1 — Merchant uses manual KYC
- Step 4a: NFC not available → Merchant uploads CCCD front/back photos → Ops team manually verifies → Longer processing time (up to 3 business days)

---

## Exception Flows

### E1 — CCCD chip read failure
- Step 4a: NFC read fails after 3 attempts → System falls back to manual KYC flow

### E2 — Biometric mismatch
- Step 7a: Face does not match CCCD photo → KYC rejected → Merchant notified → Must restart process

### E3 — Duplicate registration
- Step 3a: Tax code or CCCD already registered → System rejects → Merchant directed to customer support

---

## Postconditions
- **Success:** Merchant account created and activated, ready to generate QR
- **Failure:** Registration rejected with clear reason provided

---

## Business Rules
- eKYC mandatory per Thông tư 17/2024/TT-NHNN
- Biometric data not stored — only verification result retained
- Account activation within 24 hours for eKYC, 3 days for manual KYC
- Tax code must be valid and active per General Department of Taxation database

---

## Related Requirements
- FR-01: Merchant registration
- FR-02: eKYC verification
- FR-03: Account activation
- NFR-05: eKYC response within 10 seconds