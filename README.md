# QR Payment Merchant Onboarding System
## Self-initiated BA Portfolio Project

**Domain:** Fintech · Digital Payments · Merchant Onboarding  
**Reference:** VietQR · TCCS 04:2024/NHNN · Napas 247 · Thông tư 17/2024/TT-NHNN  
**Status:** In Progress

> This is a self-initiated portfolio project simulating a QR payment merchant onboarding system, designed based on VietQR standard, MoMo merchant flow reference, and current NHNN regulatory framework. All stakeholder interviews, requirements, and deliverables are simulated for learning and portfolio purposes.

---

## Problem Statement

A fictional fintech company is launching a QR payment feature for merchants. Currently, merchants onboard manually via paper forms and in-person KYC verification — a process that takes 5–7 business days and has a 23% error rate due to manual data entry.

**Goal:** Design a digital QR payment merchant onboarding flow that:
- Reduces onboarding time from 5–7 days to under 24 hours
- Automates KYC verification via eKYC (CCCD chip + biometric)
- Complies with Thông tư 17/2024/TT-NHNN and TCCS 04:2024/NHNN (VietQR standard)
- Supports QR generation, payment initiation, timeout handling, refund, and settlement display

---

## Scope

| In Scope | Out of Scope |
|----------|-------------|
| Merchant registration & eKYC | Consumer-side payment app |
| QR code generation (VietQR standard) | Backend payment switching engine |
| Payment initiation & confirmation | Loyalty / rewards integration |
| Timeout & error handling | Multi-currency support |
| Refund flow | Recurring / scheduled payments |
| Settlement display (read-only) | Real-time fraud detection engine |

---

## Simulated Stakeholders

| Role | Key Concern |
|------|-------------|
| Merchant (end user) | Fast onboarding, easy QR display |
| Ops / KYC Team | Verification accuracy, compliance |
| Compliance Officer | NHNN regulatory adherence |
| Developer | Clear API contracts, unambiguous specs |
| QA Engineer | Full test case coverage |
| Product Owner | Scope, timeline, business value |

---

## Requirements Summary

- **22 Functional Requirements** — merchant registration, eKYC, QR generation, payment initiation, error handling, refund, settlement display
- **9 Non-Functional Requirements** — performance (QR < 2s), security (OTP 90s, biometric), availability (99.9%), regulatory compliance
- **8 Use Case specifications** — main flow, alternative flows, exception flows
- **40 Test Cases** mapped via Traceability Matrix

---

## Deliverables

| Deliverable | Folder | Description |
|-------------|--------|-------------|
| BRD | 01-requirements/ | Business Requirements Document |
| SRS | 01-requirements/ | Software Requirements Spec + context diagram |
| Traceability Matrix | 01-requirements/ | 22 FRs → 8 Use Cases → 40 Test Cases |
| BPMN Happy Path | 02-diagrams/ | Merchant scan → payment confirm |
| BPMN Timeout | 02-diagrams/ | Session timeout (5 min) → retry / cancel |
| BPMN Refund | 02-diagrams/ | Refund → ops approval → settlement |
| Sequence Diagram | 02-diagrams/ | OTP request → SMS → verify → proceed |
| Use Cases UC01–UC08 | 03-use-cases/ | Full specifications |
| Wireframes | 04-wireframes/ | Figma link + 14 screen exports |
| API Mapping Table | 05-api-mapping/ | 8 REST endpoints — request/response schema |
| Error Handling Matrix | 05-api-mapping/ | 4xx/5xx codes, retry logic, user messages |
| Interview Notes | 06-stakeholder-notes/ | Simulated elicitation — 5 stakeholder personas |

---

## Regulatory & Technical Context

| Item | Detail |
|------|--------|
| QR Standard | VietQR — TCCS 04:2024/NHNN (EMVCo-compatible) |
| eKYC | CCCD chip + facial biometric — Thông tư 17/2024/TT-NHNN |
| Biometric Auth | Required for transactions > 10M VND — Quyết định 2345/QĐ-NHNN |
| Payment Framework | Nghị định 52/2024/NĐ-CP (thanh toán không dùng tiền mặt) |
| API Style | REST — JSON request/response |
| OTP | 6 digits, 90-second expiry, SMS delivery |
| Payment Session | Expires after 5 minutes |
| QR Generation SLA | < 2 seconds |

---

## Repository Structure

```
ba-qr-payment-onboarding/
├── 01-requirements/      # BRD, SRS, Traceability Matrix
├── 02-diagrams/          # BPMN 2.0, Sequence Diagram, Context Diagram
├── 03-use-cases/         # UC01–UC08 full specifications
├── 04-wireframes/        # Figma link + screen exports
├── 05-api-mapping/       # API Mapping Table, Endpoint spec, Error Matrix
└── 06-stakeholder-notes/ # Simulated interview notes, elicitation templates
```

---

## Skills Demonstrated

`Requirements Elicitation` `BRD / SRS Writing` `BPMN 2.0` `Use Case Specification`  
`API Mapping` `Traceability Matrix` `Stakeholder Management` `Fintech Domain`  
`VietQR / Napas 247` `NHNN Regulatory Knowledge` `Agile / Scrum`

---

*Portfolio project by Nguyen Le Bao Dang — BA candidate*  
*[LinkedIn](https://www.linkedin.com/in/nguyenlebaodang/) · [GitHub](https://github.com/baodangerous)*
