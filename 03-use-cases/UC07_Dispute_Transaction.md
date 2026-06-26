# UC07 — Dispute a Transaction

**Use Case ID:** UC07  
**Use Case Name:** Dispute a Transaction  
**Actor:** Merchant, Ops Team, System  
**Priority:** Medium  
**Status:** Draft

---

## Brief Description
Merchant raises a dispute for a transaction with an incorrect amount or unauthorized charge. Ops team investigates and resolves.

---

## Preconditions
- Transaction exists and status is "Completed"
- Dispute raised within 60 days of transaction date

---

## Main Flow

| Step | Actor | Action |
|------|-------|--------|
| 1 | Merchant | Locates transaction, selects "Raise Dispute" |
| 2 | Merchant | Selects dispute type (wrong amount / unauthorized / other) |
| 3 | Merchant | Provides description and uploads supporting evidence |
| 4 | System | Creates dispute ticket, assigns ID, notifies Ops Team |
| 5 | Ops Team | Reviews evidence within 3 business days |
| 6 | Ops Team | Resolves dispute (approve refund / reject / escalate) |
| 7 | System | Notifies merchant of resolution outcome |

---

## Exception Flows

### E1 — Dispute window expired
- Step 1a: Transaction older than 60 days → System rejects dispute → Merchant directed to contact support

### E2 — Duplicate dispute
- Step 4a: Dispute already exists for same transaction → System rejects → Merchant notified

---

## Postconditions
- **Resolved:** Dispute closed, outcome applied (refund or rejection)
- **Escalated:** Dispute passed to senior Ops team for manual review

---

## Business Rules
- Dispute window: 60 days from transaction date
- Ops team response SLA: 3 business days
- Escalated disputes resolved within 10 business days

---

## Related Requirements
- FR-20: Dispute initiation
- FR-21: Ops dispute resolution workflow
- NFR-07: Dispute acknowledgement within 1 hour