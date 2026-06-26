# UC08 — View Settlement Summary

**Use Case ID:** UC08  
**Use Case Name:** View Settlement Summary  
**Actor:** Merchant, System  
**Priority:** Medium  
**Status:** Draft

---

## Brief Description
Merchant views daily settlement summary showing total received, fees deducted, and net amount to be credited to bank account.

---

## Preconditions
- Merchant account is active
- At least one completed transaction exists for the selected period

---

## Main Flow

| Step | Actor | Action |
|------|-------|--------|
| 1 | Merchant | Navigates to "Settlement" section |
| 2 | System | Displays current day settlement summary |
| 3 | Merchant | Selects date range to view historical settlements |
| 4 | System | Returns: gross amount, transaction fee, net amount, settlement status |
| 5 | Merchant | Views individual transaction breakdown within settlement |
| 6 | Merchant | Downloads settlement report (PDF or CSV) |

---

## Alternative Flows

### A1 — Settlement pending
- Step 4a: Today's settlement not yet processed (cut-off not reached) → System shows "Pending — Settlement at 23:00"

---

## Exception Flows

### E1 — Settlement discrepancy
- Step 4a: Merchant identifies discrepancy → Merchant raises dispute via UC07

---

## Postconditions
- Merchant has full visibility of settlement amounts and timing
- Report downloaded if requested

---

## Business Rules
- Settlement cut-off: 23:00 daily
- Settlement credited to merchant bank account next business day (T+1)
- Transaction fee displayed transparently before deduction
- Settlement reports retained 5 years per accounting requirements

---

## Related Requirements
- FR-22: Settlement display
- NFR-08: Settlement report generation within 5 seconds