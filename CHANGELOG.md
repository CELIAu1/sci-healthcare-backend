# Changelog

## Unreleased

### Added
- `GET /governance` — current and pending admin for each contract, plus the
  full proposal/handover history. Backed by a new `GovernanceEvent` table fed
  by the `admin_proposed` and `admin_changed` events from all three contracts.
- `GET /providers/:address/history` — every status transition for a clinic,
  oldest first, from a new `ProviderStatusChange` table. Suspension history is
  context the current `status` alone cannot show.
- `GET /providers/:address/services/:code/history` — every observed price for a
  service, oldest first, from a new `ServicePriceChange` table. Written only
  when `service_upserted` actually changes the price; a relabel adds nothing.
- The indexer now polls again immediately when a cycle fills its page budget
  (`collectEvents` returns `hasMore`), instead of waiting `INDEXER_POLL_MS` the
  whole way through a backlog. The normal interval resumes once caught up.

## 0.2.0 — 2026-10-09

### Fixed
- **indexer:** follows the RPC cursor across pages. Previously any backlog over 200 events lost everything after the first page, because the cursor jumped to the RPC tip.

### Added
- Route tests for every endpoint against a throwaway SQLite database, through a new `buildApp()` that builds the API without starting the indexer. Tests: 57 (was 19).
- Issue forms (bug, feature) and a pull request template; blank issues off, security reports routed to SECURITY.md.
- `serviceLabel` on vouchers and receipts; `refundableAt` and `disputeReason` on vouchers. `isRefundable` follows the contract's claim-grace rule.
- `Voucher.disputeReason` column (nullable; `prisma db push` adds it in place).
- `npm run lint` (ESLint 9 + typescript-eslint) and a lint job in CI.

### Changed
- Points at the 2026-10-07 testnet deployment; `INDEXER_START_LEDGER` updated.

## 0.1.0 — 2026-09-02

Initial indexer and read API.
