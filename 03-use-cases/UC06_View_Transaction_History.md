# UC06 — View Transaction History

**Use Case ID:** UC06  
**Use Case Name:** View Transaction History  
**Actor:** Merchant  
**Priority:** Medium  
**Status:** Draft

---

## Brief Description
Merchant views list of past transactions with filter and search capabilities for reconciliation purposes.

---

## Preconditions
- Merchant is logged into POS app
- At least one transaction exists

---

## Main Flow

| Step | Actor | Action |
|------|-------|--------|
| 1 | Merchant | Navigates to "Transaction History" |
| 2 | System | Displays transactions — default last 30 days, newest first |
| 3 | Merchant | Applies filters (date range, status, amount range) |
| 4 | System | Returns filtered results |
| 5 | Merchant | Selects a transaction to view detail |
| 6 | System | Displays full detail: amount, time, customer ref, status, transaction ID |

---

## Alternative Flows

### A1 — Export transactions
- Step 4a: Merchant selects "Export" → System generates CSV → Merchant downloads file

---

## Exception Flows

### E1 — No transactions found
- Step 2a: No transactions match filter → System displays "No transactions found" message

---

## Postconditions
- Merchant has viewed transaction history
- Export file generated if requested

---

## Business Rules
- Transaction history retained for minimum 5 years per AML requirements
- Sensitive customer data masked in export (last 4 digits only)
- Export limited to 3 months per request

---

## Related Requirements
- FR-18: Transaction history display
- FR-19: Transaction export
- NFR-06: History load within 3 seconds