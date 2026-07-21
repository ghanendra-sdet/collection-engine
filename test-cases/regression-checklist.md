# Collection Engine — Regression Checklist & Test Cases

> Sample regression suite structure with dummy data. Format: ID | Scenario | Steps | Expected Result

## 1. Core Regression Flow

| ID | Scenario | Steps | Expected Result |
|---|---|---|---|
| TC-001 | Merchant login — valid credentials | 1. Navigate to login page 2. Enter valid dummy merchant ID + password 3. Submit | Merchant is redirected to Dashboard |
| TC-002 | Merchant login — invalid credentials | 1. Enter invalid password 2. Submit | Error message shown, login blocked |
| TC-003 | Dashboard summary accuracy | 1. Login 2. Compare dashboard totals to transaction list totals | Dashboard totals match underlying transaction data |
| TC-004 | Initiate a new collection | 1. Go to Collection 2. Enter dummy amount ₹500 3. Select dummy VPA `test@dummybank` 4. Submit | Transaction created with status INITIATED |
| TC-005 | Transaction search — by date range | 1. Go to Transaction Search 2. Filter last 7 days | Only transactions within range are shown |
| TC-006 | Transaction search — by status | 1. Filter by status = SUCCESS | Only SUCCESS transactions shown |
| TC-007 | Transaction details consistency | 1. Open a transaction from search results 2. Compare all fields to search row | All fields match exactly, no drift |
| TC-008 | Settlement reconciliation | 1. Go to Settlement 2. Compare settled amount to sum of SUCCESS transactions minus commercial | Settlement amount is correct to the paisa |
| TC-009 | Report download — CSV | 1. Go to Reports 2. Download CSV for date range | Downloaded totals match on-screen dashboard totals |
| TC-010 | Report download — PDF | 1. Download PDF report | PDF renders correctly, totals match CSV export |

## 2. Commercial & GST Validation

| ID | Scenario | Steps | Expected Result |
|---|---|---|---|
| TC-011 | Commercial fee calculation | 1. Initiate transaction of ₹1000 with 2% commercial | Fee = ₹20, net to merchant = ₹980 |
| TC-012 | GST on commercial | 1. Commercial fee ₹20, GST 18% | GST = ₹3.60, total deduction = ₹23.60 |
| TC-013 | Commercial rounding — edge case | 1. Transaction amount that produces a fractional paisa fee | Rounding follows defined rule (e.g. round half up), consistent across UI and report |
| TC-014 | Ledger debit entry created | 1. Successful transaction with commercial | Ledger shows matching debit entry for fee deducted |

## 3. Negative & Edge Cases

| ID | Scenario | Steps | Expected Result |
|---|---|---|---|
| TC-015 | Empty search results | 1. Search with filters matching no transactions | "No results found" message shown, no error |
| TC-016 | Invalid filter combination | 1. Apply an end date earlier than start date | Validation error shown, search blocked |
| TC-017 | Duplicate transaction request | 1. Submit the same collection request twice rapidly | Second request is either blocked or correctly deduplicated |
| TC-018 | Large transaction volume in search | 1. Search a date range with 10,000+ dummy transactions | Results paginate correctly, no timeout |
| TC-019 | Permission check — restricted admin action | 1. Login as a merchant-role user 2. Attempt to access admin-only settlement monitoring | Access denied |
| TC-020 | Settlement mismatch detection | 1. Simulate a scenario where ledger total ≠ settlement total (test data) | Discrepancy is flagged, not silently settled |

## 4. Full Regression Checklist

- [ ] Login
- [ ] Dashboard
- [ ] Merchant Profile
- [ ] Collection
- [ ] Transaction Search
- [ ] Transaction Details
- [ ] Settlement
- [ ] Reports
- [ ] Downloads / Export
- [ ] Commercial Calculation
- [ ] GST Calculation
- [ ] Ledger Entries
- [ ] Permissions / Role-Based Access

## 5. Priority Automation Candidates

1. Login
2. Dashboard
3. Collection
4. Transaction Search
5. Transaction Details
6. Settlement

These six are automated first because they form the primary merchant regression path and are
run on every release — see [`automation/`](../automation) for the Playwright implementation.
