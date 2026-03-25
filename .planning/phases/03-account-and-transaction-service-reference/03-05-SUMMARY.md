---
phase: 03-account-and-transaction-service-reference
plan: "05"
subsystem: api-reference
tags: [errors, transaction, reference]
dependency_graph:
  requires: []
  provides: [error-reference-page]
  affects: [api-reference/transaction/submit-transaction]
tech_stack:
  added: []
  patterns: [mintlify-callouts, markdown-tables, grouped-reference-tables]
key_files:
  created: []
  modified:
    - api-reference/errors.mdx
decisions:
  - "Error codes grouped into three categories (Permanent/Temporary/Indeterminate) matching proto enum value ranges (0-11, 20-23, 40-47)"
  - "Indeterminate table uses 'Likely Sent?' column instead of 'Retryable' — better captures the nuance for errors where submission status is unknown"
  - "TIMEOUT gets a Warning callout as most dangerous indeterminate error — broadcast may have occurred before timeout"
metrics:
  duration: 68s
  completed: "2026-03-25"
  tasks_completed: 1
  files_modified: 1
---

# Phase 03 Plan 05: Error Reference Summary

**One-liner:** Error reference page with all 24 TransactionErrorCode values in grouped tables plus 5 TransactionSubmissionCertainty levels with handling guidance.

## What Was Built

Replaced the stub `api-reference/errors.mdx` with a complete, scannable error reference page. The page is structured as tables with explanatory prose — no essay walls.

**Structure:**
- Opening paragraph establishing the page's purpose
- `TransactionErrorCode` section with a Note callout explaining the three groups
- **Permanent Failures** table (11 codes, 0–11 range): all rebuild-required errors
- **Temporary Failures** table (4 codes, 20–23 range): retryable without rebuilding
- **Indeterminate** table (8 codes, 40–47 range): unknown submission status; Warning callout on TIMEOUT
- `TransactionSubmissionCertainty` section with handling table for all 5 values
- Note callout explaining `blockhash_expiry_slot` and how to use it for safe resubmission

**Links wired:**
- `/api-reference/transaction/compile-transaction` — in BLOCKHASH_NOT_FOUND and INVALID_BLOCKHASH_FORMAT remediation
- `/api-reference/transaction/monitor-transaction` — in TRANSACTION_SUBMISSION_CERTAINTY_SUBMITTED handling

## Tasks Completed

| Task | Name | Commit | Files |
|------|------|--------|-------|
| 1 | Error reference page | 6984732 | api-reference/errors.mdx |

## Deviations from Plan

**1. [Rule 1 - Deviation] Indeterminate table uses "Likely Sent?" column instead of "Retryable"**
- **Found during:** Task 1
- **Issue:** The plan specifies `| Error Code | Description | Retryable | Remediation |` for all tables, but for indeterminate errors "Retryable" is ambiguous — the question isn't whether to retry but whether the transaction was even sent.
- **Fix:** Used `| Error Code | Description | Likely Sent? | Remediation |` for the Indeterminate table, matching the plan's own prose which says "check `structured_error.certainty` to determine the recovery strategy."
- **Files modified:** api-reference/errors.mdx
- **Commit:** 6984732

## Known Stubs

None — all 24 error codes and all 5 certainty values are fully documented.

## Self-Check: PASSED

- api-reference/errors.mdx: FOUND
- Commit 6984732: verified via git log
- All 24 TRANSACTION_ERROR_CODE_ values: confirmed (grep count = 24)
- All 5 TRANSACTION_SUBMISSION_CERTAINTY_ values: confirmed (grep count = 5)
- No "Coming soon" or stub text: confirmed
