---
phase: 02-getting-started-and-core-concepts
plan: "03"
subsystem: concepts
tags: [commitment-levels, token-programs, solana, core-concepts, documentation]
dependency_graph:
  requires: [api-reference/shared-types.mdx]
  provides: [concepts/commitment-levels.mdx, concepts/token-programs.mdx]
  affects: [docs navigation, developer onboarding flow]
tech_stack:
  added: []
  patterns: [Mintlify MDX, comparison table for trade-off decisions, Warning callout for invalid enum values]
key_files:
  created: []
  modified:
    - concepts/commitment-levels.mdx
    - concepts/token-programs.mdx
decisions:
  - "Comparison table used for CommitmentLevel (PROCESSED/CONFIRMED/FINALIZED/UNSPECIFIED) — aligns with CONTEXT.md D-08 discretion area"
  - "Section-per-program structure used for Token Programs — LEGACY and 2022 each get a dedicated section with program address, characteristics, and use cases"
  - "Added Common Mistakes section to commitment-levels.mdx to address Pitfall 5 root cause (race conditions from wrong level choice)"
metrics:
  duration: "2 minutes"
  completed: "2026-03-25T11:04:11Z"
  tasks_completed: 2
  files_modified: 2
---

# Phase 02 Plan 03: Core Concept Pages — Commitment Levels and Token Programs Summary

Two decision-oriented concept pages replacing stubs: CommitmentLevel trade-off table with common mistakes, and Token Programs incompatibility guide with program addresses and a selection decision tree.

## What Was Built

**concepts/commitment-levels.mdx** — A developer can read this page and choose the correct `CommitmentLevel` enum value for their use case. The page explains the three Solana confirmation stages (processed, confirmed, finalized) in plain terms, presents a comparison table with latency and rollback risk for each level, provides prose decision guidance (default to CONFIRMED, use FINALIZED for fund movements, avoid PROCESSED for anything critical), and includes a Common Mistakes section addressing the two most frequent errors (using PROCESSED for decision-driving reads, using CONFIRMED for deposit verification). Links to `/api-reference/shared-types#commitmentlevel`.

**concepts/token-programs.mdx** — A developer can read this page and understand why they must specify a consistent `token_program` for every operation, and which value to choose. The page leads with the incompatibility premise, includes a Warning callout that `TOKEN_PROGRAM_UNSPECIFIED` causes a service error, gives each program a dedicated section with its on-chain address and characteristics, explains that incompatibility is permanent (baked in at mint creation), and closes with a three-scenario decision guide. Links to `/api-reference/shared-types#tokenprogram` and `/api-reference/token-program/overview`.

## Tasks Completed

| Task | Name | Commit | Files |
|------|------|--------|-------|
| 1 | Write the Commitment Levels concept page | 6aff86a | concepts/commitment-levels.mdx |
| 2 | Write the Token Programs concept page | b16d94e | concepts/token-programs.mdx |

## Deviations from Plan

### Auto-fixed Issues

**1. [Rule 2 - Missing content] Added Common Mistakes section to commitment-levels.mdx**
- **Found during:** Task 1
- **Issue:** The plan's required page structure did not include a Common Mistakes section, but the PITFALLS.md research (Pitfall 5) identified that developers frequently make the wrong CommitmentLevel choice in predictable ways (PROCESSED for decision-driving reads, CONFIRMED for deposit verification). Adding this section directly addresses the pitfall the page was designed to solve.
- **Fix:** Added a "Common mistakes" section after the decision guidance prose.
- **Files modified:** concepts/commitment-levels.mdx
- **Commit:** 6aff86a

None other — plan executed closely as written.

## Known Stubs

None — both pages are complete concept pages with no placeholder content, no hardcoded empty values, and no "coming soon" or "under construction" text.

## Self-Check: PASSED

- [x] concepts/commitment-levels.mdx exists and contains PROCESSED, CONFIRMED, FINALIZED, UNSPECIFIED — verified
- [x] concepts/commitment-levels.mdx links to /api-reference/shared-types — verified
- [x] concepts/commitment-levels.mdx is 50 lines — verified (wc -l = 50)
- [x] concepts/token-programs.mdx exists and contains TOKEN_PROGRAM_LEGACY, TOKEN_PROGRAM_2022 — verified
- [x] concepts/token-programs.mdx contains Warning callout for UNSPECIFIED — verified
- [x] concepts/token-programs.mdx links to shared-types and token-program/overview — verified
- [x] concepts/token-programs.mdx contains "incompatible" — verified
- [x] concepts/token-programs.mdx is 55 lines — verified (wc -l = 55)
- [x] No code fences on either page — verified
- [x] No stub text on either page — verified
- [x] Commit 6aff86a exists — verified
- [x] Commit b16d94e exists — verified
