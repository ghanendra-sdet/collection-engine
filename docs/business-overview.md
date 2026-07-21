# Collection Engine — Business Overview

> **Start here if you're new to fintech QA, in HR, or from a non-QA technical role.** This
> document explains what a Collection Engine does and why it matters, before you look at any
> test case or code.

## 1. What problem does it solve?

Merchants need a reliable way to accept digital payments from customers and know — with
certainty — how much money they received, what fee was charged, and when the money will land in
their bank account. The Collection Engine is the system that guarantees this.

## 2. Business Objectives

- Accept customer payments (UPI, card, net banking, wallet)
- Maintain the full transaction lifecycle (initiated → success/failed/deemed/expired)
- Manage settlements — moving collected funds to merchants
- Generate merchant-facing reports
- Calculate commercials (platform fees) and GST
- Produce ledger entries for audit and reconciliation
- Provide dashboard analytics for both merchants and internal ops

## 3. Major Functional Areas

### Merchant-facing
- Login
- Dashboard
- Collection (initiating/viewing payment collection)
- Transaction Search
- Transaction Details
- Settlement
- Reports

### Admin-facing
- Merchant onboarding
- Merchant activation
- Collection enablement
- Commercial configuration
- Merchant & settlement monitoring
- Audit logs

## 4. Glossary

| Term | Meaning |
|---|---|
| **Merchant** | The business receiving payments through the platform |
| **Transaction** | A single customer payment event, with its own lifecycle status |
| **Settlement** | The process/record of moving collected funds to the merchant's bank account |
| **Ledger** | An auditable record of every debit/credit — the source of truth for reconciliation |
| **Commercial** | The platform's fee charged to the merchant for processing a transaction |
| **GST** | Tax applied on top of the commercial fee |
| **Regression** | Re-testing existing functionality to confirm a new change hasn't broken it |
| **Smoke Testing** | A quick check that the most critical paths work after a deployment |
| **Sanity Testing** | A focused check on a specific change/fix, narrower than full regression |

## 5. Why This Module Is High-Risk

Unlike a UI cosmetic bug, a defect in the Collection Engine can mean:

- A merchant is paid the wrong amount
- A settlement report doesn't match the actual bank transfer
- GST is calculated incorrectly, creating a compliance issue
- A ledger entry is missing, breaking the audit trail

This is why QA strategy for this module is built around **financial correctness first** —
commercial accuracy, ledger consistency, and settlement reconciliation are prioritized above
purely cosmetic issues.

## 6. Cross-Module Dependencies

The Collection Engine conceptually depends on / interacts with:

- **Connected Banking** — actual money movement between banks
- **Commercial Engine** — fee and GST calculation logic
- **Settlement & Ledger** — reconciliation and audit trail
- **Admin Portal** — configuration and monitoring
- **Reporting** — merchant and internal analytics

See [`architecture-and-flow.md`](./architecture-and-flow.md) for the detailed flow diagrams.
