---
phase: 01-foundation
plan: 03
subsystem: api
tags: [mintlify, mdx, grpc, protobuf, solana, snippets]

# Dependency graph
requires:
  - phase: 01-foundation plan 01
    provides: Site structure, api-reference directory, snippets directory
provides:
  - Authoritative shared types reference page (CommitmentLevel, TokenProgram, KeyPair)
  - 6 reusable MDX snippet files for cross-page consistency
  - Cross-link destination for all future method pages that use shared types
affects:
  - Phase 2+ API reference method pages (Account, Transaction, Token Program, System Program, RPC Client)
  - Any page that uses CommitmentLevel, TokenProgram, or KeyPair parameters

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "ResponseField (not ParamField) for documenting proto message fields"
    - "Snippet files as MDX fragments — no frontmatter, imported inline on method pages"
    - "Warning callout for proto enum invalid values (UNSPECIFIED variants)"
    - "Behavioral depth for enum documentation: each value gets when-to-use and trade-offs"

key-files:
  created:
    - api-reference/shared-types.mdx
    - snippets/commitment-level-note.mdx
    - snippets/token-program-field.mdx
    - snippets/base58-note.mdx
    - snippets/grpc-connection-go.mdx
    - snippets/grpc-connection-ts.mdx
    - snippets/grpc-connection-rust.mdx
  modified: []

key-decisions:
  - "D-09 enforced: each CommitmentLevel enum value has behavioral description with when-to-use and trade-offs, not just enum names"
  - "D-10 enforced: no code blocks on shared-types.mdx — code examples belong on method pages"
  - "D-11 enforced: connection snippet tab order is Go, Rust, TypeScript site-wide"
  - "D-13 enforced: connection snippets link to /guides/connecting for production setup, not inline setup"
  - "ResponseField used throughout instead of ParamField to avoid broken API Playground injection"

patterns-established:
  - "Shared types page: link destination pattern — types documented once, method pages cross-link"
  - "Snippet pattern: MDX fragments without frontmatter, imported on method pages to prevent content drift"
  - "Warning pattern: UNSPECIFIED enum values always get a Warning callout explaining they are invalid"
  - "Connection boilerplate pattern: snippet shows Account Service as template; per-service pages adapt the import and client type"

requirements-completed: [FOUND-05, FOUND-06]

# Metrics
duration: 2min
completed: 2026-03-25
---

# Phase 1 Plan 03: Shared Types Reference Page and MDX Snippets Summary

**Authoritative shared types reference (CommitmentLevel, TokenProgram, KeyPair) with behavioral depth and 6 reusable MDX snippet files ready for import by Phase 2+ method pages**

## Performance

- **Duration:** ~2 min
- **Started:** 2026-03-25T10:37:23Z
- **Completed:** 2026-03-25T10:39:13Z
- **Tasks:** 2
- **Files modified:** 7

## Accomplishments

- Wrote `api-reference/shared-types.mdx` with behavioral depth per D-09: each of the 4 CommitmentLevel values documents speed/reliability trade-offs and when to use it; TokenProgram documents SPL Token vs Token-2022 incompatibility; KeyPair documents both encoding formats and security warning
- Created 3 content snippets (commitment-level-note, token-program-field, base58-note) as importable MDX fragments for reuse across ~15+ method pages
- Created 3 connection boilerplate snippets (Go, TypeScript, Rust) with Note callouts linking to /guides/connecting per D-13

## Task Commits

Each task was committed atomically:

1. **Task 1: Write the shared types reference page** - `5c6dae4` (feat)
2. **Task 2: Create reusable MDX snippets** - `ad26a68` (feat)

**Plan metadata:** (docs commit — see final commit)

## Files Created/Modified

- `api-reference/shared-types.mdx` - Cross-link destination for all method pages; documents CommitmentLevel (4 values), TokenProgram (3 values), KeyPair (2 fields) with behavioral guidance and 2 Warning callouts
- `snippets/commitment-level-note.mdx` - Info callout for any method that takes commitment_level; links to shared-types#commitmentlevel
- `snippets/token-program-field.mdx` - ResponseField for token_program with incompatibility warning; links to shared-types#tokenprogram
- `snippets/base58-note.mdx` - Info callout explaining Base58 address encoding format
- `snippets/grpc-connection-go.mdx` - Go gRPC connection setup (Account Service template) with /guides/connecting Note
- `snippets/grpc-connection-ts.mdx` - TypeScript gRPC connection setup (Account Service template) with /guides/connecting Note
- `snippets/grpc-connection-rust.mdx` - Rust gRPC connection setup (Account Service template) with /guides/connecting Note

## Decisions Made

- No new decisions — executed per existing locked decisions D-09 through D-13 from 01-CONTEXT.md
- ResponseField enforced throughout (not ParamField) per STACK.md recommendation to avoid API Playground injection

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- `api-reference/shared-types.mdx` is ready to be cross-linked from any method page using CommitmentLevel, TokenProgram, or KeyPair
- All 6 snippet files are ready for import via Mintlify snippet syntax on Phase 2+ method pages
- Connection snippets show Account Service as template — Phase 2 executors should adapt import paths and client types for their respective services
- No blockers for Phase 2 API reference work

---
*Phase: 01-foundation*
*Completed: 2026-03-25*
