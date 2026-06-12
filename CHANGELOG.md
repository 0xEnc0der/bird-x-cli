# Changelog

## v3.0.0 (2026-06-12)

### Fixed
- **Transaction ID double-generation bug** — Removed broken `prepareTransactionId()` call from `twitter-client-posting.ts`. The function was generating a transaction ID before the URL was known, causing `getBaseHeaders()` to consume it, then `fetchWithTimeout()` to generate a different one for the actual URL. The mismatch caused HTTP 401 errors on all write operations (tweet, reply).
- **`fetchWithTimeout` now always overwrites** `x-client-transaction-id` with a properly generated value bound to the actual request URL path. The random value from `getBaseHeaders()` is irrelevant because it gets replaced.

### Documentation
- Added Python-based env file writing instructions (bash heredoc/cat corrupts tokens with special characters)
- Added env file verification step (auth_token must be exactly 40 chars)
- Added troubleshooting guide for 401 errors
- Documented the root cause: corrupted env file, not IP binding or code bugs

### Files Changed
- `src/lib/twitter-client-base.ts` — `fetchWithTimeout` always overwrites txn ID
- `src/lib/twitter-client-posting.ts` — Removed `prepareTransactionId()` call

## v0.9.0 (zaydiscold/bird — 2026-06)
- Added `x-client-transaction-id` header support
- Added proper GET vs POST header separation
- Updated GraphQL query IDs for June 2026

## v0.1.0 (jawond/bird — 2025-12)
- Initial release
