---
phase: 04-program-services-and-error-reference
plan: "01"
subsystem: system-program-service
tags: [api-reference, system-program, instruction-builder, mdx, navigation]
dependency_graph:
  requires: []
  provides: [system-program-overview, system-program-create, system-program-transfer, docs-json-grouped-nav]
  affects: [docs.json, api-reference/system-program/]
tech_stack:
  added: []
  patterns: [grouped-method-pages, instruction-builder-note-callout, codegen-three-language]
key_files:
  created:
    - api-reference/system-program/overview.mdx
    - api-reference/system-program/create.mdx
    - api-reference/system-program/transfer.mdx
  modified:
    - docs.json
decisions:
  - "Grouped Create+CreateWithSeed on one page and Transfer+TransferWithSeed on one page per D-01"
  - "Note callout on each instruction method page links to /concepts/instructions-and-transactions per D-06"
  - "docs.json System Program group collapsed from 14 individual entries to 6 grouped page entries"
  - "error-handling-patterns added to Errors group in docs.json for Plan 04"
metrics:
  duration: "~5 minutes"
  completed_date: "2026-03-25"
  tasks_completed: 3
  files_created: 3
  files_modified: 1
---

# Phase 4 Plan 1: System Program Service Overview and Create/Transfer Pages Summary

System Program Service overview, Create Account, and Transfer SOL pages written as instruction builder reference docs; docs.json navigation updated to grouped structure removing 9 individual stub entries.

## Tasks Completed

| Task | Name | Commit | Files |
|------|------|--------|-------|
| 1 | Write System Program Service overview page | e9b5b9d | api-reference/system-program/overview.mdx |
| 2 | Write Create / CreateWithSeed grouped method page | 8cc54ee | api-reference/system-program/create.mdx |
| 3 | Write Transfer / TransferWithSeed page + update docs.json | 27d68eb | api-reference/system-program/transfer.mdx, docs.json |

## What Was Built

**System Program Service Overview** (`api-reference/system-program/overview.mdx`): Full overview page explaining the instruction builder pattern. Opening paragraph explains that methods return SolanaInstruction objects that must be submitted via Transaction Service. Note callout reinforces this. Methods table with 5 rows covers all 11 System Program methods via grouped page links.

**Create Account Page** (`api-reference/system-program/create.mdx`): Grouped page covering both Create and CreateWithSeed. Each method has request fields (ResponseField), SolanaInstruction response field, and CodeGroup with Go/Rust/TypeScript examples. Note callout links to Instructions & Transactions.

**Transfer SOL Page** (`api-reference/system-program/transfer.mdx`): Grouped page covering both Transfer and TransferWithSeed. TransferWithSeed documents the from_base/from_seed signing model. Same structure as Create page.

**docs.json Navigation Update**: System Program Service group collapsed from 14 individual entries to 6 grouped entries. Removed individual seed-variant stubs (create-with-seed, allocate-with-seed, assign-with-seed, transfer-with-seed) and nonce operation stubs (5 entries). Added nonce-operations for Plan 02. Added error-handling-patterns to Errors group for Plan 04.

## Deviations from Plan

None — plan executed exactly as written.

## Known Stubs

None — all three written pages contain full content. The `nonce-operations` and `error-handling-patterns` entries added to docs.json reference pages not yet created (planned in Plans 02 and 04). Mintlify will show warnings for these but will not break existing pages.

## Self-Check: PASSED

- api-reference/system-program/overview.mdx: FOUND
- api-reference/system-program/create.mdx: FOUND
- api-reference/system-program/transfer.mdx: FOUND
- docs.json nonce-operations entry: FOUND
- docs.json error-handling-patterns entry: FOUND
- No stub content ("Coming soon"/"under construction") in written pages: CONFIRMED
- docs.json valid JSON: CONFIRMED
- SolanaInstruction in create.mdx: 3 occurrences
- SolanaInstruction in transfer.mdx: 3 occurrences
