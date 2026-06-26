# UC05 — Generate VietQR Code

**Use Case ID:** UC05  
**Use Case Name:** Generate VietQR Code  
**Actor:** Merchant, System  
**Priority:** High  
**Status:** Draft

---

## Brief Description
Merchant generates a VietQR code for a specific payment amount, compliant with TCCS 04:2024/NHNN standard.

---

## Preconditions
- Merchant account is active and verified
- Merchant is logged into POS app

---

## Main Flow

| Step | Actor | Action |
|------|-------|--------|
| 1 | Merchant | Selects "Receive Payment" in POS app |
| 2 | Merchant | Enters payment amount (required) and description (optional) |
| 3 | System | Validates amount (> 0, ≤ daily limit) |
| 4 | System | Generates VietQR payload per TCCS 04:2024/NHNN |
| 5 | System | Encodes payload into QR image, displays on screen |
| 6 | System | Starts 5-minute session timer |
| 7 | Merchant | Shows QR to customer for scanning |

---

## Exception Flows

### E1 — Amount exceeds daily limit
- Step 3a: Amount > merchant daily limit → System rejects → Merchant notified of limit

### E2 — QR generation service unavailable
- Step 4a: QR service timeout → System retries once → If fails, shows error → Merchant advised to retry

---

## Postconditions
- QR code displayed, session timer started
- QR valid for 5 minutes

---

## Business Rules
- QR must comply with TCCS 04:2024/NHNN (VietQR)
- QR generation SLA: < 2 seconds (NFR-01)
- Static QR (without amount) also supported for display at counter

---

## Related Requirements
- FR-04: QR code generation
- FR-06: Session timer initiation
- NFR-01: QR generation < 2 seconds