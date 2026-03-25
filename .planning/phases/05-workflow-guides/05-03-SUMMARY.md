---
phase: 05-workflow-guides
plan: "03"
subsystem: docs
tags: [grpc, streaming, monitoring, solana, mintlify, mdx]

# Dependency graph
requires:
  - phase: 03-account-and-transaction-service-reference
    provides: MonitorTransaction reference page (api-reference/transaction/monitor-transaction.mdx) with basic stream loop and TransactionStatus table
  - phase: 04-program-services-and-error-reference
    provides: errors.mdx and error-handling-patterns.mdx that the guide links to
provides:
  - "guides/monitor-transaction.mdx — definitive MonitorTransaction streaming guide with production reconnection patterns"
affects: [api-reference/transaction/monitor-transaction, future-phases]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "Narrative prose with inline CodeGroup blocks (no Steps component) for workflow guides"
    - "Exponential backoff pattern: base * 2^attempt, capped at 30s"
    - "Sentinel error values in Go for distinguishing terminal states from retriable errors"

key-files:
  created:
    - guides/monitor-transaction.mdx
  modified: []

key-decisions:
  - "Reconnect-with-backoff function is the centrepiece — longer than other sections intentionally; this is the production pattern"
  - "FAILED and DROPPED do not retry in the backoff loop — only TIMEOUT and stream errors get exponential backoff"
  - "Tab order Go, Rust, TypeScript maintained across all 4 CodeGroup blocks"

patterns-established:
  - "Production streaming pattern: retry on network error and TIMEOUT with backoff; surface FAILED/DROPPED immediately to caller"

requirements-completed: [GUIDE-03]

# Metrics
duration: 88s
completed: "2026-03-25"
---

# Phase 5 Plan 03: Monitor Transactions Summary

**Production MonitorTransaction guide with full lifecycle table, basic stream consumption, terminal state handling prose, and reconnect-with-backoff function in Go, Rust, and TypeScript**

## Performance

- **Duration:** 88s (~1.5 min)
- **Started:** 2026-03-25T12:58:04Z
- **Completed:** 2026-03-25T13:00:12Z
- **Tasks:** 1 completed
- **Files modified:** 1

## Accomplishments

- Replaced stub (`guides/monitor-transaction.mdx`) with full narrative guide
- TransactionStatus table with all 7 statuses and terminal/non-terminal designation
- Basic stream consumption CodeGroup in all 3 languages (adapted from reference page)
- Terminal state handling section covering FINALIZED, FAILED, DROPPED, TIMEOUT — explicit action for each
- Production reconnect-with-backoff function in Go, Rust, and TypeScript (~40-50 lines each)
- Short CodeGroups for commitment level selection and include_logs usage
- Next steps linking to MonitorTransaction reference, Error Reference, Error Handling Patterns, Transfer SOL guide

## Task Commits

Each task was committed atomically:

1. **Task 1: Write the Monitor Transactions workflow guide** - `cd02cdc` (feat)

**Plan metadata:** (docs commit follows)

## Files Created/Modified

- `guides/monitor-transaction.mdx` — Full guide replacing "This page is under construction" stub — 466 lines, 8 CodeGroup blocks, 4 complete code sections

## Decisions Made

- FAILED and DROPPED do not apply exponential backoff in the retry loop — they are deterministic/caller-decides outcomes, not retriable conditions
- TIMEOUT and stream errors both use the same backoff formula and continue the retry loop
- Go uses sentinel error values (`errFailed`, `errDropped`, `errTimeout`) to distinguish terminal states cleanly across the `readStream` helper
- Rust uses string matching on error messages for brevity (avoids a custom error enum in a focused snippet)
- TypeScript uses a Promise wrapper around the event-emitter stream to enable `async/await` usage in the retry loop

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- All 3 workflow guides are now complete (Transfer SOL, Create Token, Monitor Transactions)
- Phase 05 is the final phase — no subsequent phases to prepare for
- Monitor Transactions guide is linked from the MonitorTransaction reference page (already established in Phase 3)

---
*Phase: 05-workflow-guides*
*Completed: 2026-03-25*
