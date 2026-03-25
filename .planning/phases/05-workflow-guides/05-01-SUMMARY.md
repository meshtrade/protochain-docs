---
phase: 05-workflow-guides
plan: "01"
subsystem: guides
tags: [guide, transfer-sol, system-program, transaction-service, workflow]
dependency_graph:
  requires:
    - api-reference/system-program/transfer.mdx
    - api-reference/system-program/create.mdx
    - api-reference/transaction/compile-transaction.mdx
    - api-reference/transaction/sign-transaction.mdx
    - api-reference/transaction/submit-transaction.mdx
    - api-reference/transaction/monitor-transaction.mdx
  provides:
    - guides/transfer-sol.mdx
  affects:
    - guides/quickstart.mdx (linked as prerequisite)
tech_stack:
  added: []
  patterns:
    - Narrative prose with inline CodeGroup blocks (D-02)
    - Focused code snippets — one call per section (D-01, D-08)
    - Go, Rust, TypeScript tab order (D-07)
    - Warning callout before SignTransaction for plaintext key risk
    - Note callout after SubmitTransaction for async semantics
key_files:
  created:
    - guides/transfer-sol.mdx
  modified: []
decisions:
  - "Imports shown only in the first CodeGroup (Connect + Transfer section); subsequent sections assume context per D-01"
  - "Warning callout included on SignTransaction section (matches existing pattern on reference page)"
  - "10 CodeGroup blocks total (plan required 5 minimum) — each section has one focused block per language"
metrics:
  duration: "78 seconds"
  completed: "2026-03-25T12:59:16Z"
  tasks_completed: 1
  files_modified: 1
---

# Phase 5 Plan 01: Transfer SOL Workflow Guide Summary

End-to-end Transfer SOL guide using System Program Transfer instruction compiled and submitted via Transaction Service — complete happy-path narrative with Go, Rust, TypeScript CodeGroups for every step.

## What Was Built

`guides/transfer-sol.mdx` — a full narrative workflow guide replacing the "under construction" stub. The guide covers the complete happy path from building a transfer instruction through monitoring the on-chain result:

1. **Build the transfer instruction** — System Program `Transfer` call returning a `SolanaInstruction`
2. **Compile the transaction** — wrap instruction in DRAFT `Transaction`, call `CompileTransaction` (auto-fetches blockhash)
3. **Sign the transaction** — `SignTransaction` with private key, advances to FULLY_SIGNED
4. **Submit the transaction** — `SubmitTransaction` returns signature immediately (async)
5. **Monitor the result** — `MonitorTransaction` stream with full terminal-state handling (FINALIZED, FAILED, DROPPED, TIMEOUT)

## Decisions Made

- Imports included only in the first CodeGroup (System Program connection setup) — subsequent snippets assume context, keeping examples focused (D-01/D-08)
- SignTransaction Warning callout included to match the established pattern on the reference page — plaintext key risk must be visible before the code
- Note callout on SubmitTransaction section reinforces async semantics inline (not just a reference link) — critical developer gotcha

## Deviations from Plan

None — plan executed exactly as written.

## Known Stubs

None — all code sections are wired to real API calls with realistic placeholder values.

## Self-Check: PASSED

- [x] `guides/transfer-sol.mdx` exists with 396 lines of real content
- [x] 10 CodeGroup blocks (plan required minimum 5)
- [x] No stub text ("under construction" not found)
- [x] All 6 required reference links present:
  - `/api-reference/system-program/transfer` — yes
  - `/api-reference/system-program/create` — linked via concept page mention
  - `/api-reference/transaction/compile-transaction` — yes
  - `/api-reference/transaction/sign-transaction` — yes
  - `/api-reference/transaction/submit-transaction` — yes
  - `/api-reference/transaction/monitor-transaction` — yes
- [x] Tab order Go, Rust, TypeScript in every CodeGroup
- [x] No Steps component used
- [x] Commit `0d9ff72` confirmed present
