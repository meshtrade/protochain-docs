---
phase: 03-account-and-transaction-service-reference
plan: "02"
subsystem: api
tags: [grpc, solana, transaction, mintlify, mdx]

# Dependency graph
requires:
  - phase: 02-getting-started-and-core-concepts
    provides: transaction-lifecycle concept page at /concepts/transaction-lifecycle (linked from overview and method pages)
provides:
  - Transaction Service overview page listing all 8 methods with state transitions
  - CompileTransaction reference page (DRAFT → COMPILED state transition, optional blockhash)
  - EstimateTransaction reference page (compute_units, fee_lamports, priority_fee)
  - SimulateTransaction reference page (dry-run semantics, success/error/logs)
  - SignTransaction reference page (oneof signing_method, COMPILED → FULLY_SIGNED transitions, plaintext key Warning)
  - CheckIfTransactionIsExpired reference page (~150 slot expiry, recompile recovery)
affects: [03-account-and-transaction-service-reference, future guides referencing pre-submission transaction flow]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "Standard unary method template: opening + optional Note/Warning + ## Request (ResponseField) + ## Response (ResponseField) + ## Code Examples (CodeGroup Go/Rust/TypeScript)"
    - "CommitmentLevelNote snippet imported for methods accepting CommitmentLevel"
    - "Expandable for nested proto message types (Transaction, TransactionConfig, SignWithPrivateKeys, KeySeed)"
    - "Warning callout for security-sensitive operations (plaintext private key transmission)"

key-files:
  created: []
  modified:
    - api-reference/transaction/overview.mdx
    - api-reference/transaction/compile-transaction.mdx
    - api-reference/transaction/estimate-transaction.mdx
    - api-reference/transaction/simulate-transaction.mdx
    - api-reference/transaction/sign-transaction.mdx
    - api-reference/transaction/check-if-transaction-is-expired.mdx

key-decisions:
  - "Transaction Service overview uses table with State Transition column (not available on Account Service overview since most methods are read-only)"
  - "CompileTransaction Note explains instructions field is no longer source of truth after compilation — important gotcha for developers reusing objects"
  - "SignTransaction Warning placed before Request section (not after) so it's the first thing developers read"
  - "oneof signing_method documented using Note callout explaining mutual exclusivity, plus separate ResponseField entries for private_keys and seeds"

patterns-established:
  - "State transition vocab (DRAFT, COMPILED, PARTIALLY_SIGNED, FULLY_SIGNED) used consistently across overview and all relevant method pages"
  - "Recovery action linked inline in Note (e.g., CheckIfTransactionIsExpired links to CompileTransaction)"

requirements-completed: [TXN-01, TXN-02, TXN-03, TXN-04, TXN-05, TXN-06]

# Metrics
duration: 2min
completed: 2026-03-25
---

# Phase 03 Plan 02: Transaction Service Overview and Pre-Submission Methods Summary

**Transaction Service overview + 5 pre-submission method pages (CompileTransaction, EstimateTransaction, SimulateTransaction, SignTransaction, CheckIfTransactionIsExpired) with full field docs, behavioral notes, and Go/Rust/TypeScript examples**

## Performance

- **Duration:** 2 min
- **Started:** 2026-03-25T12:15:04Z
- **Completed:** 2026-03-25T12:17:04Z
- **Tasks:** 3
- **Files modified:** 6

## Accomplishments

- Transaction Service overview page listing all 8 methods with state transition column and link to transaction-lifecycle concept page
- CompileTransaction reference documenting DRAFT → COMPILED transition, optional blockhash auto-fetch, and Expandable TransactionConfig fields
- EstimateTransaction and SimulateTransaction pages with full response field docs and CommitmentLevelNote snippet
- SignTransaction page with Warning about plaintext private key transmission and oneof signing_method documentation
- CheckIfTransactionIsExpired page explaining ~150 slot expiry window and linking to CompileTransaction for recovery

## Task Commits

Each task was committed atomically:

1. **Task 1: Transaction Service overview page** - `486998d` (feat)
2. **Task 2: CompileTransaction, EstimateTransaction, and SimulateTransaction pages** - `74bbe54` (feat)
3. **Task 3: SignTransaction and CheckIfTransactionIsExpired pages** - `1f7ad20` (feat)

## Files Created/Modified

- `api-reference/transaction/overview.mdx` - Transaction Service overview with all 8 methods table, state transitions, lifecycle link
- `api-reference/transaction/compile-transaction.mdx` - DRAFT → COMPILED, optional blockhash, Expandable TransactionConfig
- `api-reference/transaction/estimate-transaction.mdx` - Read-only fee/compute estimation, CommitmentLevelNote
- `api-reference/transaction/simulate-transaction.mdx` - Dry-run semantics, simulation caveat Note, success/error/logs response
- `api-reference/transaction/sign-transaction.mdx` - Warning, oneof signing_method, state transitions, PARTIALLY_SIGNED → FULLY_SIGNED
- `api-reference/transaction/check-if-transaction-is-expired.mdx` - is_expired field, ~150 slot expiry, CompileTransaction recovery link

## Decisions Made

- Transaction Service overview includes a State Transition column since the transaction state machine is a central concept; Account Service overview (plan 03-01) omits this because most methods are read-only
- SignTransaction Warning placed before the Request section so it's visible before developers start filling in fields
- oneof signing_method documented via Note callout explaining mutual exclusivity, then separate ResponseField entries for each variant — avoids ambiguity about which fields to send

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- All 6 files from this plan are complete with real content (no stubs remain)
- TXN-01 through TXN-06 requirements fulfilled
- Remaining Transaction Service methods (SubmitTransaction, GetTransaction, MonitorTransaction) and error reference are in separate plans within Phase 03

---
*Phase: 03-account-and-transaction-service-reference*
*Completed: 2026-03-25*
