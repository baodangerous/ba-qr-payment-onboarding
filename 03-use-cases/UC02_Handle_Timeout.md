# UC02 — Handle Payment Session Timeout

**Use Case ID:** UC02  
**Use Case Name:** Handle Payment Session Timeout  
**Actor:** System, Merchant  
**Priority:** High  
**Status:** Draft

---

## Brief Description
Payment session expires after 5 minutes of inactivity. System invalidates QR code and notifies merchant to generate a new one.

---

## Preconditions
- UC01 has been initiated (QR code generated)
- No payment confirmation received within 5 minutes

---

## Main Flow

| Step | Actor | Action |
|------|-------|--------|
| 1 | System | Monitors payment session timer from QR generation |
| 2 | System | At T+5:00, marks session as expired |
| 3 | System | Invalidates QR code — any scan after this returns error |
| 4 | System | Sends timeout notification to merchant POS app |
| 5 | Merchant | Sees "Session Expired" message with option to retry |
| 6 | Merchant | Selects "Generate New QR" |
| 7 | System | Creates new QR code, resets 5-minute timer |

---

## Alternative Flows

### A1 — Customer scans expired QR
- Step 3a: Customer scans expired QR → System returns "QR Expired" error → Customer prompted to ask merchant for new QR

---

## Exception Flows

### E1 — Merchant does not retry
- Merchant dismisses notification → Session closed → No further action required

---

## Postconditions
- **Timeout:** Original QR invalidated, new QR generated if merchant retries
- **No retry:** Session closed, no funds transferred

---

## Business Rules
- Payment session timeout = 5 minutes from QR generation (FR-07)
- Expired QR must return clear error to customer scanner
- Timeout events must be logged for reconciliation

---

## Related Requirements
- FR-07: Payment session timeout
- FR-08: QR code invalidation
- NFR-02: Timeout notification delivered within 1 second