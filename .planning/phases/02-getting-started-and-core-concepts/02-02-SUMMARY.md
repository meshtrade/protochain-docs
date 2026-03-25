---
phase: 02-getting-started-and-core-concepts
plan: "02"
subsystem: docs
tags: [transaction-lifecycle, instructions, solana, grpc, protochain, mintlify, mdx]

# Dependency graph
requires:
  - phase: 01-foundation
    provides: MDX style patterns from shared-types.mdx, docs.json navigation stubs for concept pages
provides:
  - Transaction Lifecycle concept page with ASCII state machine (concepts/transaction-lifecycle.mdx)
  - Instructions and Transactions concept page with instruction builder pattern (concepts/instructions-and-transactions.mdx)
affects:
  - Phase 03 (API reference) — both pages are cross-link targets for Transaction Service, System Program, and Token Program method pages

# Tech tracking
tech-stack:
  added: []
  patterns:
    - Concept pages are purely prose — no code examples, links to method pages for code (D-05)
    - ASCII art state machine in unlabeled code block renders as monospace (D-07)
    - <Steps> component used for numbered walkthrough sequences on concept pages

key-files:
  created:
    - concepts/transaction-lifecycle.mdx
    - concepts/instructions-and-transactions.mdx
  modified: []

key-decisions:
  - "D-05 enforced: concept pages have no code examples — kept prose-only to preserve focus"
  - "D-07 enforced: ASCII art diagram in unlabeled code block, no Mermaid"
  - "Added a 'What can go wrong' section to transaction-lifecycle.mdx — practical debugging aid not in plan spec but clearly within scope of D-04 practical guide tone"

patterns-established:
  - "Concept pages pattern: opening paragraph → diagram/structure → section prose → practical walkthrough → callouts → footer links"
  - "Instruction builder pattern naming: explicitly called out as 'instruction builder pattern' for consistent cross-referencing"

requirements-completed:
  - CONC-01
  - CONC-04

# Metrics
duration: 2min
completed: 2026-03-25
---

# Phase 02 Plan 02: Core Concepts — Transaction Lifecycle & Instructions Summary

**Transaction lifecycle state machine page (DRAFT→COMPILED→PARTIALLY_SIGNED→FULLY_SIGNED) and instruction builder pattern page, both purely conceptual with forward links to Phase 3 API reference.**

## Performance

- **Duration:** ~2 min
- **Started:** 2026-03-25T11:02:09Z
- **Completed:** 2026-03-25T11:03:37Z
- **Tasks:** 2
- **Files modified:** 2

## Accomplishments

- Transaction Lifecycle page: complete state machine explanation with ASCII diagram, six-step typical transaction walkthrough using `<Steps>`, blockhash expiry note, and debugging section for state errors
- Instructions and Transactions page: SolanaInstruction anatomy, instruction builder pattern walkthrough (prose only), multi-instruction atomicity explanation, and design rationale section
- Both pages replace placeholder stubs — no "under construction" or "coming soon" text remains

## Task Commits

Each task was committed atomically:

1. **Task 1: Write the Transaction Lifecycle page with ASCII state machine** - `110df48` (feat)
2. **Task 2: Write the Instructions and Transactions concept page** - `18afdb0` (feat)

**Plan metadata:** (final commit below)

## Files Created/Modified

- `concepts/transaction-lifecycle.mdx` — Complete concept page: state machine ASCII diagram, four state descriptions, six-step walkthrough, blockhash expiry callout, error debugging guidance, footer link to /api-reference/transaction/overview
- `concepts/instructions-and-transactions.mdx` — Complete concept page: SolanaInstruction field breakdown, instruction builder pattern walkthrough, multi-instruction atomicity, design rationale, footer links to System Program and Token Program API references

## Decisions Made

- Added a "What can go wrong" section to transaction-lifecycle.mdx — this wasn't in the plan spec but fits the D-04 practical guide tone and directly helps developers debug state errors. Within scope, no deviation rule needed.
- Both pages follow D-05 strictly: prose descriptions of concepts, zero code blocks with language tags.

## Deviations from Plan

None — plan executed exactly as written, with one additive enhancement (see Decisions Made above) that falls within the plan's stated practical guide intent.

## Issues Encountered

None.

## User Setup Required

None — no external service configuration required.

## Next Phase Readiness

- Both pages are ready as cross-link targets for Phase 3 API reference pages
- Transaction Service method pages can link to /concepts/transaction-lifecycle for state context
- System Program and Token Program method pages can link to /concepts/instructions-and-transactions for instruction builder context
- No blockers for Phase 3

---
*Phase: 02-getting-started-and-core-concepts*
*Completed: 2026-03-25*
