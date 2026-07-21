# Collection Engine — Automation Framework

Automation for the Collection Engine's primary merchant regression path, built with
**Playwright + TypeScript** using the **Page Object Model (POM)**.

## Why Playwright + TypeScript

- Native auto-waiting reduces flaky tests around async transaction status updates
- Built-in API request context lets UI and API assertions live in the same test where useful
  (e.g. verify a transaction created via API appears correctly in the UI)
- TypeScript gives compile-time safety for page objects and shared fixtures across a large suite
- Built-in HTML reporter with screenshots/video/trace on failure — no extra reporting plugin needed

## Suggested Project Structure

```
automation/
├── README.md
├── playwright.config.ts
├── pages/
│   ├── LoginPage.ts
│   ├── DashboardPage.ts
│   ├── CollectionPage.ts
│   ├── TransactionSearchPage.ts
│   ├── TransactionDetailsPage.ts
│   └── SettlementPage.ts
├── fixtures/
│   └── dummy-merchant.ts
└── tests/
    ├── sample-collection.spec.ts
    └── ...
```

> This repo currently includes one representative sample (`sample-collection.spec.ts`) rather
> than the full framework, to keep the portfolio focused. The structure above is what a full
> implementation would look like.

## Test Data Policy

All automation uses **dummy data only**:
- Dummy merchant IDs (e.g. `DEMOMERCHANT001`)
- Dummy VPAs (e.g. `test@dummybank`)
- Dummy amounts and dates generated at runtime, never fixed production values

## Running Tests (representative example)

```bash
npm install
npx playwright test
npx playwright show-report
```

## Priority Automated Scenarios

Automated in this order because they form the primary end-to-end merchant regression path,
run on every release:

1. Login
2. Dashboard
3. Collection
4. Transaction Search
5. Transaction Details
6. Settlement
