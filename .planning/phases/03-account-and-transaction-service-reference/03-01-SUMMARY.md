---
phase: 03-account-and-transaction-service-reference
plan: "01"
subsystem: api
tags: [mintlify, mdx, grpc, solana, account-service]

requires:
  - phase: 02-getting-started-and-core-concepts
    provides: snippets/commitment-level-note.mdx and snippets/token-program-field.mdx; shared-types.mdx KeyPair and CommitmentLevel docs
provides:
  - Account Service overview page with all 5 methods linked
  - GetAccount full reference with expanded Account type, CommitmentLevel snippet, Go/Rust/TypeScript examples
  - GenerateNewKeyPair reference with plaintext private key Warning, seed field docs, KeyPair link
  - FundNative reference with devnet-only Warning, lamport amount string docs
  - GetTokenAccountBalance reference with token holding account Note, raw integer explanation
  - GetAssociatedTokenAddress reference with token-program-field snippet and SPL vs Token-2022 PDA Warning
affects:
  - 03-02-transaction-service-reference
  - 04-system-and-token-program-service-reference

tech-stack:
  added: []
  patterns:
    - "Standard unary method page: opening prose → optional Warning/Note → ## Request (ResponseField per field + CommitmentLevel snippet) → ## Response (ResponseField, Expandable for nested types) → ## Code Examples (CodeGroup Go/Rust/TypeScript)"
    - "Nested proto message types use Expandable component; linked shared types (KeyPair) are not re-expanded — link to shared-types.mdx instead"
    - "Token program field reused via snippets/token-program-field.mdx import rather than inline"

key-files:
  created:
    - api-reference/account/overview.mdx
    - api-reference/account/get-account.mdx
    - api-reference/account/generate-new-key-pair.mdx
    - api-reference/account/fund-native.mdx
    - api-reference/account/get-token-account-balance.mdx
    - api-reference/account/get-associated-token-address.mdx
  modified: []

key-decisions:
  - "Expandable used for Account nested type in GetAccount response; KeyPair in GenerateNewKeyPair links to shared-types rather than expanding (type already documented there)"
  - "FundNative Warning placed before prose — devnet-only restriction is the first thing developers must see"
  - "GetTokenAccountBalance uses both Note (wrong address gotcha) and Info (decimal example) callouts to prevent two common integration mistakes"

patterns-established:
  - "Standard unary method page template: prose → callouts → Request section → Response section → Code Examples"
  - "Security warnings precede method prose (FundNative, GenerateNewKeyPair) — critical context before API details"
  - "Decimal amount fields documented as raw string with explicit divide-by-10^decimals explanation"

requirements-completed: [ACCT-01, ACCT-02, ACCT-03, ACCT-04, ACCT-05, ACCT-06]

duration: 2min
completed: 2026-03-25
---

# Phase 3 Plan 01: Account Service Overview and Method Pages Summary

**Six MDX reference pages for the Account Service: overview with methods table plus full GetAccount, GenerateNewKeyPair, FundNative, GetTokenAccountBalance, and GetAssociatedTokenAddress documentation with ResponseField components, Warning/Note callouts, and Go/Rust/TypeScript CodeGroup examples**

## Performance

- **Duration:** ~2 min
- **Started:** 2026-03-25T12:14:51Z
- **Completed:** 2026-03-25T12:17:00Z
- **Tasks:** 3
- **Files modified:** 6

## Accomplishments

- Replaced all 6 "Coming soon" stubs with complete developer-ready content
- Established reusable page template for unary gRPC method documentation
- All critical callouts placed: devnet-only Warning on FundNative, plaintext private key Warning on GenerateNewKeyPair, token holding account Note on GetTokenAccountBalance, PDA difference Warning on GetAssociatedTokenAddress

## Task Commits

Each task was committed atomically:

1. **Task 1: Account Service overview page** - `6bd2704` (feat)
2. **Task 2: GetAccount and GenerateNewKeyPair method pages** - `561a38a` (feat)
3. **Task 3: FundNative, GetTokenAccountBalance, and GetAssociatedTokenAddress method pages** - `052030d` (feat)

## Files Created/Modified

- `api-reference/account/overview.mdx` - Account Service overview with methods table linking all 5 methods
- `api-reference/account/get-account.mdx` - Full GetAccount reference with Expandable Account type, CommitmentLevel snippet
- `api-reference/account/generate-new-key-pair.mdx` - GenerateNewKeyPair with security Warning and KeyPair link to shared-types
- `api-reference/account/fund-native.mdx` - FundNative with devnet-only Warning, CommitmentLevel snippet
- `api-reference/account/get-token-account-balance.mdx` - GetTokenAccountBalance with token account Note and decimal explanation
- `api-reference/account/get-associated-token-address.mdx` - GetAssociatedTokenAddress with token-program-field snippet and PDA Warning

## Decisions Made

- Expandable used for Account nested type in GetAccount response; KeyPair in GenerateNewKeyPair links to shared-types rather than expanding (type already documented there with full field definitions)
- FundNative Warning placed before prose — devnet-only restriction must be seen before any API details
- GetTokenAccountBalance uses both Note (wrong address gotcha) and Info (decimal calculation example) callouts to prevent the two most common integration mistakes with this method

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- All 6 Account Service pages complete and free of stubs
- Template pattern established for remaining Transaction Service method pages in plans 03-02 through 03-04
- snippets/commitment-level-note.mdx and snippets/token-program-field.mdx confirmed working via import/usage on multiple pages

## Self-Check: PASSED

All 6 files exist on disk. All 3 task commits verified in git log.

---
*Phase: 03-account-and-transaction-service-reference*
*Completed: 2026-03-25*
