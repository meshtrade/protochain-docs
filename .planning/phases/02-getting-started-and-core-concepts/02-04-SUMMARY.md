---
phase: 02-getting-started-and-core-concepts
plan: 04
subsystem: docs
tags: [mintlify, grpc, solana, account-service, quickstart, go, rust, typescript]

requires:
  - phase: 02-01
    provides: connecting.mdx and grpc-connection-*.mdx snippets that quickstart links to and builds on

provides:
  - guides/quickstart.mdx — complete 4-call workflow: connect, GetAccount, GenerateNewKeyPair, FundNative + final GetAccount balance verification

affects:
  - future phases referencing guides/quickstart as the canonical getting-started entry point

tech-stack:
  added: []
  patterns:
    - "Steps component wraps multi-step workflows end-to-end"
    - "CodeGroup with Go/Rust/TypeScript tabs in that order (D-10)"
    - "Warning callout for any method returning plaintext private key material"
    - "Note callout for devnet-only methods"
    - "System Program address 11111111111111111111111111111111 used as connectivity test address"

key-files:
  created:
    - guides/quickstart.mdx
  modified: []

key-decisions:
  - "TypeScript tab includes comment noting callback vs promise API depends on generated client version (unverified SDK signatures per STATE.md blocker)"
  - "GetAccount called twice — once for connectivity test (Step 2) and once for balance verification after FundNative (Step 4) — creates concrete success moment"
  - "publicKey variable threaded through Steps 3 and 4 via inline comment rather than full program context — keeps examples minimal per D-11"

patterns-established:
  - "Quickstart pattern: connectivity test first (GetAccount on known address), then generated resources, then devnet funding"
  - "Devnet-only methods always gated with <Note> callout before the code"

requirements-completed:
  - START-01
  - START-02
  - START-03

duration: 5min
completed: 2026-03-25
---

# Phase 02 Plan 04: Quickstart Summary

**Quickstart page with 4-call Account Service workflow: GetAccount connectivity test, GenerateNewKeyPair with security Warning, FundNative devnet airdrop, and balance verification — developer ends with a funded keypair**

## Performance

- **Duration:** ~5 min
- **Started:** 2026-03-25T11:10:00Z
- **Completed:** 2026-03-25T11:15:00Z
- **Tasks:** 1
- **Files modified:** 1

## Accomplishments

- Replaced stub guides/quickstart.mdx with a complete 260-line workflow page
- Four API calls demonstrated end-to-end: GetAccount, GenerateNewKeyPair, FundNative, GetAccount (verification)
- All three SDKs covered in CodeGroup tabs (Go, Rust, TypeScript) for every step
- Page links to /guides/connecting rather than duplicating connection setup
- Security Warning on plaintext private key; devnet-only Note on FundNative

## Task Commits

1. **Task 1: Write the Quickstart page** - `9e0f858` (feat)

## Files Created/Modified

- `guides/quickstart.mdx` — Complete 5-minute workflow page: Steps 1-4 with CodeGroup tabs, Warning/Note callouts, next steps section

## Decisions Made

- TypeScript tab includes a comment noting callback vs promise API depends on generated client version (per STATE.md blocker: exact SDK method signatures not verified)
- Used System Program address (`11111111111111111111111111111111`) as GetAccount connectivity test — always present on all Solana networks
- FundNative + GetAccount verification combined in Step 4 to create the concrete "success moment" described in project context D-01

## Deviations from Plan

None — plan executed exactly as written.

## Known Stubs

None — all code examples reference real proto-defined methods (GetAccount, GenerateNewKeyPair, FundNative) with correct request/response field names from the plan's interface definitions. The TypeScript callback-style API follows the grpc-js pattern established in connecting.mdx and the connection snippets.

## Issues Encountered

None.

## User Setup Required

None — no external service configuration required.

## Next Phase Readiness

- Quickstart page complete; developers now have a concrete first-success path into the documentation
- Phase 3 (API reference) can reference guides/quickstart for working code context
- STATE.md blocker still active: exact TypeScript SDK method signatures unverified — Phase 3 API reference pages should verify against generated client before finalizing TypeScript examples

---
*Phase: 02-getting-started-and-core-concepts*
*Completed: 2026-03-25*
