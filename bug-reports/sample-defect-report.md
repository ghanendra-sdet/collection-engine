# Sample Defect Report — Collection Engine

> Template + worked example using dummy data. Reflects the recurring defect themes commonly
> found in Collection Engine regression: commercial/GST mismatches, ledger inconsistencies, and
> settlement discrepancies.

---

## Defect #1

| Field | Value |
|---|---|
| **ID** | BUG-COL-1042 (sample) |
| **Title** | Ledger debit entry missing for commercial fee on successful UPI collection |
| **Severity** | Critical |
| **Module** | Collection → Ledger |
| **Environment** | UAT (dummy data) |

**Steps to Reproduce**
1. Login as dummy merchant `DEMOMERCHANT001`
2. Initiate a collection of ₹1000 with a 2% commercial fee
3. Wait for transaction status to become `SUCCESS`
4. Navigate to Ledger and search for the corresponding entry

**Expected Result**
A debit ledger entry of ₹20 (2% of ₹1000) should appear, matching the commercial fee deducted
from the merchant's settlement amount.

**Actual Result**
No ledger entry is created for the commercial fee — the settlement amount reflects the deduction,
but the ledger shows only the gross transaction credit, with no matching debit line.

**Impact**
Breaks the audit trail: settlement and ledger totals will not reconcile, which could cause
discrepancies during a compliance or financial audit.

**Root Cause (if known)**
`[Add details]` — commonly caused by an async ledger-write step not being triggered on the same
event as the settlement calculation.

**Suggested Fix**
Ensure the ledger debit write and settlement calculation are triggered from the same transaction
event, ideally within the same atomic operation or a reliably retried async job.

---

## Defect #2

| Field | Value |
|---|---|
| **ID** | BUG-COL-1078 (sample) |
| **Title** | GST rounding mismatch between UI display and downloaded report |
| **Severity** | Major |
| **Module** | Collection → Reports |
| **Environment** | UAT (dummy data) |

**Steps to Reproduce**
1. Initiate a collection producing a commercial fee with a fractional GST value (e.g. GST on ₹17
   at 18% = ₹3.06)
2. View the transaction in the dashboard/transaction details (shows ₹3.06)
3. Download the CSV report for the same date range

**Expected Result**
GST value in the downloaded report should exactly match the value shown in the UI (₹3.06).

**Actual Result**
Report shows ₹3.10 — rounding is applied differently between the UI (rounds to nearest paisa)
and the report generation service (rounds up to nearest 10 paise).

**Impact**
Merchant-facing financial reports don't match the platform's own UI — a trust and compliance
issue even though the discrepancy is small per transaction.

**Suggested Fix**
Centralize the rounding rule in a single shared calculation function used by both the UI and the
report generation service.

---

## Defect Reporting Template (blank)

| Field | Value |
|---|---|
| **ID** | |
| **Title** | |
| **Severity** | Minor / Major / Critical / Blocker |
| **Module** | |
| **Environment** | |

**Steps to Reproduce**
1.
2.
3.

**Expected Result**


**Actual Result**


**Impact**


**Suggested Fix**

