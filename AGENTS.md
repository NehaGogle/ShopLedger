# Repository Guidelines

## Stack

Node.js with CommonJS, Express, and `body-parser`. `server.js` serves plain HTML, CSS, and JavaScript from `public/` and mounts the routers under `/api`. The app persists data in `data/db.json`.

## Module map

- `routes/customers.js` — Lists Khata customers and recent settlements, and records repayments.
- `routes/orders.js` — Lists, fetches, places, and cancels orders.
- `routes/products.js` — Lists and filters products, and lists active coupons.
- `routes/reports.js` — Serves dashboard and GST reconciliation reports, pricing previews, and database export.
- `lib/coupons.js` — Normalizes and validates coupon codes, calculates discounts, and lists active coupons.
- `lib/dates.js` — Formats dates, normalizes date strings, and derives fiscal years.
- `lib/inventory.js` — Finds products, checks sellable stock, deducts physical stock, releases holds, and summarizes inventory.
- `lib/invoice.js` — Recalculates invoice figures and renders printable HTML.
- `lib/ledger.js` — Calculates Khata balances, charges and reverses order debits, and records settlements.
- `lib/orders.js` — Adds, fetches, cancels, and filters orders.
- `lib/pricing.js` — Calculates subtotals, coupon discounts, line GST, and order totals.
- `lib/reports.js` — Aggregates dashboard figures and compares saved order totals and tax with invoice figures.
- `lib/store.js` — Reads and rewrites the JSON database and appends audit entries.
- `lib/tax.js` — Resolves GST rates and splits tax into CGST and SGST.

## Commands

- `npm start` — Runs `node server.js` on port 3000 unless `PORT` is set.
- `node --test` — Runs Node's built-in test runner. The repository currently has no test files, and `npm test` is a failing placeholder.

## Rules

1. Write characterization tests before changing behaviour.
2. Change only the module(s) the ticket is about.
3. Tests must use in-memory data and must never read or write `data/db.json`.
4. Add no dependencies.
5. Make the smallest fix that works.

## Current ticket

- Checkout rejects the last sellable unit because `inventory.check()` rejects `qty >= sellable`.
- Cancelling an order never restores physical stock. `orders.cancel()` calls `inventory.release()`, which looks up `productId` and only adjusts `reserved`. Saved order lines use `id`.
- Allowed files are `lib/inventory.js` and `lib/orders.js`. In `lib/orders.js`, edit only `cancel()`.
