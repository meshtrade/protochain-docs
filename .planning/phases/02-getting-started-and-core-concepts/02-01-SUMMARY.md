---
phase: 02-getting-started-and-core-concepts
plan: "01"
subsystem: docs
tags: [grpc, go, rust, typescript, getting-started, mintlify, mdx]

requires:
  - phase: 01-foundation
    provides: MDX component patterns (CodeGroup, Note, Warning), site navigation structure, shared-types page style

provides:
  - Complete gRPC connection setup guide for Go, Rust, and TypeScript (guides/connecting.mdx)
  - CodeGroup tab pattern with Go/Rust/TypeScript ordering established for getting-started content
  - TLS/credentials explanation covering all three SDK languages

affects:
  - 02-02 (quickstart — links to connecting.mdx as prerequisite)
  - Any future guide referencing gRPC connection setup

tech-stack:
  added: []
  patterns:
    - "CodeGroup tab order: Go first, Rust second, TypeScript third (consistent with D-10)"
    - "Placeholder YOUR_ENDPOINT:50051 used in code examples to prevent silent drift on copy-paste"
    - "Note callout for prerequisites at top of getting-started pages"

key-files:
  created: []
  modified:
    - guides/connecting.mdx

key-decisions:
  - "Used YOUR_ENDPOINT:50051 placeholder (not localhost) to make it explicit that developers must substitute their real endpoint"
  - "Go example shows full package main with defer conn.Close() — more instructive than a snippet, avoids pitfall of leaving connections open"
  - "Rust and TypeScript reuse same conn/client approach shown in snippets; Go reuse-conn pattern called out explicitly in Switching Services section"

patterns-established:
  - "Prerequisites in a Note callout at top of guide pages"
  - "Switching services and TLS covered as short follow-on sections after the main CodeGroup"

requirements-completed: [START-01, START-03]

duration: 1min
completed: "2026-03-25"
---

# Phase 2 Plan 01: Connecting to Protochain Summary

**Complete gRPC connection setup guide with CodeGroup tabs for Go, Rust, and TypeScript — explains channels, credentials, and stub instantiation for developers coming from REST**

## Performance

- **Duration:** ~1 min
- **Started:** 2026-03-25T11:02:00Z
- **Completed:** 2026-03-25T11:02:49Z
- **Tasks:** 1 of 1
- **Files modified:** 1

## Accomplishments

- Replaced the "under construction" stub with a complete, navigable gRPC connection guide
- Covers all three SDK languages (Go, Rust, TypeScript) in a CodeGroup with correct tab order
- Explains the gRPC-vs-REST conceptual gap (why channels and stubs are needed at all)
- Documents TLS/credentials switching for local vs production environments
- Links to Quickstart as the next step in the getting-started flow

## Task Commits

Each task was committed atomically:

1. **Task 1: Write the Connecting to Protochain page** - `1c58751` (feat)

**Plan metadata:** _(docs commit hash added after state update)_

## Files Created/Modified

- `guides/connecting.mdx` — Complete gRPC connection setup guide (100 lines)

## Decisions Made

- Used `YOUR_ENDPOINT:50051` as placeholder (not `localhost:50051`) to make it clear developers must substitute their real endpoint — prevents copy-paste failures from silent wrong endpoint
- Go example is a full `package main` with `defer conn.Close()`, not just a snippet — the defer pattern is easy to miss and leaks connections when omitted
- "Switching services" section calls out that Go can reuse a single `conn` across multiple stubs (efficiency concern not obvious from the snippet)

## Deviations from Plan

None — plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None — no external service configuration required.

## Next Phase Readiness

- `guides/connecting.mdx` is complete and ready to be linked from any getting-started content
- The Quickstart page (`guides/quickstart.mdx`) is referenced as next steps — that page must exist for the link to resolve
- Blocker from STATE.md still applies: exact SDK method signatures not verified against generated code — verify before writing Phase 2 code examples in subsequent plans

---
*Phase: 02-getting-started-and-core-concepts*
*Completed: 2026-03-25*
