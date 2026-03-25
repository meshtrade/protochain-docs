---
phase: 04-program-services-and-error-reference
plan: 02
subsystem: system-program-service
tags: [api-reference, system-program, instruction-builder, nonce-accounts, mdx]
dependency_graph:
  requires:
    - 04-01 (System Program overview and Create/Transfer pages)
  provides:
    - api-reference/system-program/allocate.mdx
    - api-reference/system-program/assign.mdx
    - api-reference/system-program/nonce-operations.mdx
  affects:
    - System Program Service navigation (all 6 method pages now complete)
tech_stack:
  added: []
  patterns:
    - Grouped method pages (two methods per page with horizontal rule separator)
    - Note callout for instruction builder pattern on every method page
    - ResponseField for request and response documentation
    - CodeGroup with Go/Rust/TypeScript tabs
key_files:
  created:
    - api-reference/system-program/nonce-operations.mdx
  modified:
    - api-reference/system-program/allocate.mdx
    - api-reference/system-program/assign.mdx
decisions:
  - Grouped Allocate + AllocateWithSeed on one page (per D-01)
  - Grouped Assign + AssignWithSeed on one page (per D-01)
  - All 5 nonce operations on single nonce-operations.mdx page (per D-01)
  - Opening prose on nonce-operations.mdx explains why nonce accounts exist before listing methods (per CONTEXT.md specifics)
metrics:
  duration: ~5 minutes
  completed_date: "2026-03-25"
  tasks_completed: 3
  files_changed: 3
---

# Phase 4 Plan 2: Allocate, Assign, and Nonce Operations Summary

Grouped Allocate/AllocateWithSeed, Assign/AssignWithSeed, and all 5 nonce operation methods into three complete reference pages, completing the System Program Service documentation.

## Tasks Completed

| Task | Name | Commit | Files |
|------|------|--------|-------|
| 1 | Write Allocate / AllocateWithSeed grouped method page | 302d252 | api-reference/system-program/allocate.mdx |
| 2 | Write Assign / AssignWithSeed grouped method page | 140ccda | api-reference/system-program/assign.mdx |
| 3 | Write Nonce Operations page (all 5 nonce methods) | 56b7d5d | api-reference/system-program/nonce-operations.mdx |

## What Was Built

**allocate.mdx** — Covers `Allocate` (allocate space to an existing account) and `AllocateWithSeed` (allocate space for a seed-derived account). Both methods documented with typed request fields, SolanaInstruction response, and Go/Rust/TypeScript code examples. Includes Note callout linking to Instructions & Transactions.

**assign.mdx** — Covers `Assign` (change account owner program) and `AssignWithSeed` (reassign owner of seed-derived account). Both methods documented with typed request fields including `owner_program`, SolanaInstruction response, and Go/Rust/TypeScript code examples. Includes Note callout.

**nonce-operations.mdx** — New file covering all 5 nonce account methods: `InitializeNonceAccount`, `AuthorizeNonceAccount`, `WithdrawNonceAccount`, `AdvanceNonceAccount`, `UpgradeNonceAccount`. Opens with 3-sentence explanation of why nonce accounts exist (blockhash expiration problem, offline signing use case). Each method has typed request fields, SolanaInstruction response, and Go/Rust/TypeScript code examples.

## Deviations from Plan

None - plan executed exactly as written.

Note: docs.json was already updated by parallel Plan 01 execution to include `nonce-operations` in the System Program Service navigation, so no docs.json changes were needed.

## Known Stubs

None. All three pages are fully implemented with complete field documentation and code examples.

## Self-Check: PASSED

- `api-reference/system-program/allocate.mdx` — FOUND (129 lines, full content)
- `api-reference/system-program/assign.mdx` — FOUND (129 lines, full content)
- `api-reference/system-program/nonce-operations.mdx` — FOUND (301 lines, full content)
- Commit 302d252 — FOUND (allocate.mdx)
- Commit 140ccda — FOUND (assign.mdx)
- Commit 56b7d5d — FOUND (nonce-operations.mdx)
- grep "Coming soon\|under construction" — 0 matches (CLEAN)
- grep -c "instructions-and-transactions" — 1 per file (FOUND)
