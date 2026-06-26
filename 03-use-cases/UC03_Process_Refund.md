# UC03 — Process Refund

**Use Case ID:** UC03  
**Use Case Name:** Process Refund  
**Actor:** Merchant, Ops Team, System  
**Priority:** Medium  
**Status:** Draft

---

## Brief Description
Merchant initiates a refund for a completed transaction. Ops team reviews and approves. System processes reversal and notifies both parties.

---

## Preconditions
- Original transaction exists and status is "Completed"
- Refund request submitted within 30 days of original transaction
- Merchant account has sufficient balance to cover refund

---

## Main Flow

| Step | Actor | Action |
|------|-------|--------|
| 1 | Merchant | Locates original transaction in POS app history |
| 2 | Merchant | Selects "Request Refund", enters reason and refund amount |
| 3 | System | Validates refund eligibility (within 30 days, amount ≤ original) |
| 4 | System | Creates refund request, assigns ticket ID, notifies Ops Team |
| 5 | Ops Team | Reviews refund request in admin dashboard |
| 6 | Ops Team | Approves refund |
| 7 | System | Processes reversal via Napas 247 |
| 8 | System | Credits customer account, debits merchant account |
| 9 | System | Sends confirmation to merchant and customer with refund ID |

---

## Alternative Flows

### A1 — Partial refund
- Step 2a: Merchant enters amount less than original → System processes partial refund → Both parties notified of partial amount

### A2 — Ops team requests more information
- Step 6a: Ops team marks as "Pending — Info Required" → Merchant notified → Merchant submits additional info → Returns to Step 5

---

## Exception Flows

### E1 — Refund rejected
- Step 6b: Ops team rejects refund → Merchant notified with rejection reason → No funds transferred

### E2 — Refund window expired
- Step 3a: Transaction older than 30 days → System rejects request → Merchant sees "Refund window expired"

### E3 — Insufficient merchant balance
- Step 7a: Merchant account balance insufficient → System pauses refund → Ops team notified → Manual resolution required

---

## Postconditions
- **Approved:** Customer credited, merchant debited, refund ID generated
- **Rejected:** No funds transferred, rejection reason logged
- **Pending:** Request held for additional information

---

## Business Rules
- Refund window: 30 days from original transaction date
- Refund amount cannot exceed original transaction amount
- All refunds require Ops Team approval — no auto-approval
- Refund processing time: maximum 3 business days per NHNN guidelines

---

## Related Requirements
- FR-15: Refund initiation
- FR-16: Ops approval workflow
- FR-17: Refund processing and notification
- NFR-04: Refund status update within 24 hours