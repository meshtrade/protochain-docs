---
gsd_state_version: 1.0
milestone: v1.0
milestone_name: milestone
status: unknown
stopped_at: Completed 05-03-PLAN.md
last_updated: "2026-03-25T13:01:02.599Z"
progress:
  total_phases: 5
  completed_phases: 5
  total_plans: 19
  completed_plans: 19
---

# Project State

## Project Reference

See: .planning/PROJECT.md (updated 2026-03-25)

**Core value:** External developers can discover, understand, and successfully integrate with every Protochain gRPC service without needing to read the source code or proto files directly.
**Current focus:** Phase 05 — workflow-guides

## Current Position

Phase: 05
Plan: Not started

## Performance Metrics

**Velocity:**

- Total plans completed: 0
- Average duration: -
- Total execution time: 0 hours

**By Phase:**

| Phase | Plans | Total | Avg/Plan |
|-------|-------|-------|----------|
| - | - | - | - |

**Recent Trend:**

- Last 5 plans: -
- Trend: -

*Updated after each plan completion*
| Phase 01-foundation P02 | 1 | 1 tasks | 1 files |
| Phase 01-foundation P03 | 2 | 2 tasks | 7 files |
| Phase 01-foundation P01 | 15 | 2 tasks | 73 files |
| Phase 02-getting-started-and-core-concepts P01 | 1 | 1 tasks | 1 files |
| Phase 02 P02 | 2 | 2 tasks | 2 files |
| Phase 02-getting-started-and-core-concepts P03 | 2 | 2 tasks | 2 files |
| Phase 02-getting-started-and-core-concepts P04 | 5 | 1 tasks | 1 files |
| Phase 03-account-and-transaction-service-reference P05 | 68s | 1 tasks | 1 files |
| Phase 03-account-and-transaction-service-reference P03 | 3 minutes | 2 tasks | 2 files |
| Phase 03-account-and-transaction-service-reference P04 | 85 | 1 tasks | 1 files |
| Phase 03-account-and-transaction-service-reference P01 | 2 | 3 tasks | 6 files |
| Phase 03-account-and-transaction-service-reference P02 | 2 | 3 tasks | 6 files |
| Phase 04-program-services-and-error-reference P01 | 5 minutes | 3 tasks | 4 files |
| Phase 04-program-services-and-error-reference P04 | 5min | 2 tasks | 3 files |
| Phase 04-program-services-and-error-reference P02 | 5 minutes | 3 tasks | 3 files |
| Phase 04-program-services-and-error-reference P03 | 176 | 3 tasks | 10 files |
| Phase 05-workflow-guides P01 | 78s | 1 tasks | 1 files |
| Phase 05-workflow-guides P02 | 85s | 1 tasks | 1 files |
| Phase 05-workflow-guides P03 | 88 | 1 tasks | 1 files |

## Accumulated Context

### Decisions

Decisions are logged in PROJECT.md Key Decisions table.
Recent decisions affecting current work:

- [Init]: Manual API reference pages over auto-generation (gRPC protos lack standard doc-gen pipeline for Mintlify)
- [Init]: Structure by service, not operation type (matches developer mental model)
- [Init]: Multi-language code examples — Go, Rust, TypeScript (all three SDKs are generated)
- [Phase 01-foundation]: Hero leads with language barrier problem (D-01): problem-first messaging for Protochain landing page
- [Phase 01-foundation]: Landing page uses two primary CTAs: Get Started → /guides/quickstart, API Reference → /api-reference/index (D-02)
- [Phase 01-foundation]: D-09 enforced: CommitmentLevel enum values use behavioral descriptions with when-to-use and trade-offs (not just enum names)
- [Phase 01-foundation]: D-10 enforced: no code blocks on shared-types.mdx; code examples belong on service method pages
- [Phase 01-foundation]: ResponseField used throughout shared-types and token-program-field snippet (not ParamField) to avoid API Playground injection
- [Phase 01-01]: Anchor icon changed from book-open-cover to globe for Mesh.Trade link
- [Phase 01-01]: Preserved api-reference/shared-types.mdx existing real content rather than downgrading to stub
- [Phase 02-getting-started-and-core-concepts]: YOUR_ENDPOINT:50051 placeholder used (not localhost) in connecting.mdx to prevent silent copy-paste failures
- [Phase 02-getting-started-and-core-concepts]: Go connection example is full package main with defer conn.Close() to teach the resource-management pattern explicitly
- [Phase 02]: D-05 enforced: concept pages have no code examples — prose-only keeps concept pages focused
- [Phase 02]: D-07 enforced: ASCII art state machine diagram in unlabeled code block, no Mermaid
- [Phase 02-getting-started-and-core-concepts]: Comparison table used for CommitmentLevel trade-offs; section-per-program structure used for Token Programs concept page
- [Phase 02-getting-started-and-core-concepts]: TypeScript tab in quickstart includes comment noting callback vs promise API depends on generated client version (unverified SDK signatures per existing blocker)
- [Phase 02-getting-started-and-core-concepts]: System Program address used as GetAccount connectivity test in quickstart — always present on all Solana networks
- [Phase 03-account-and-transaction-service-reference]: Indeterminate table uses 'Likely Sent?' column instead of 'Retryable' — better captures nuance for errors where submission status is unknown
- [Phase 03-account-and-transaction-service-reference]: Warning callout placed before Request in SubmitTransaction to ensure developers see async semantics prominently
- [Phase 03-account-and-transaction-service-reference]: GetTransaction Note placed before Request to prime developers on polling vs MonitorTransaction before reading fields
- [Phase 03-account-and-transaction-service-reference]: MonitorTransaction uses guide-style page (D-04): narrative lifecycle walkthrough with Mermaid diagram, not standard unary template
- [Phase 03-account-and-transaction-service-reference]: TIMEOUT/DROPPED recovery actions documented inline (actionable Note callout): TIMEOUT → GetTransaction, DROPPED → resubmit or recompile
- [Phase 03-account-and-transaction-service-reference]: Expandable used for Account nested type in GetAccount; KeyPair links to shared-types rather than expanding
- [Phase 03-account-and-transaction-service-reference]: Security Warning callouts placed before method prose on FundNative and GenerateNewKeyPair — critical restriction must precede API details
- [Phase 03-account-and-transaction-service-reference]: Transaction Service overview includes State Transition column — state machine is central to this service unlike read-only Account Service
- [Phase 03-account-and-transaction-service-reference]: SignTransaction Warning placed before Request section so it's visible before developers fill in fields
- [Phase 04-program-services-and-error-reference]: Grouped Create+CreateWithSeed and Transfer+TransferWithSeed on shared pages (D-01); Note callout on every instruction method page linking to Instructions & Transactions (D-06)
- [Phase 04-program-services-and-error-reference]: RPC Client overview kept intentionally minimal — single navigation anchor for 1-method service; all substance in method page
- [Phase 04-program-services-and-error-reference]: Error handling patterns page (ERR-03) links to errors.mdx rather than duplicating error code classification
- [Phase 04-program-services-and-error-reference]: Grouped Allocate+AllocateWithSeed and Assign+AssignWithSeed on shared pages; all 5 nonce operations on single nonce-operations.mdx page with opening prose explaining why nonces exist
- [Phase 04-program-services-and-error-reference]: Deleted orphaned individual stub files (create-token2022-mint.mdx etc.) to prevent stale pages from appearing at direct URLs after navigation update
- [Phase 04-program-services-and-error-reference]: ParseMint intentionally omits instruction builder Note callout — it is a query method, not an instruction builder
- [Phase 05-workflow-guides]: Imports shown only in first CodeGroup; subsequent sections assume context (D-01/D-08)
- [Phase 05-workflow-guides]: SignTransaction Warning callout included in guide to match reference page pattern — plaintext key risk visible before code
- [Phase 05-workflow-guides]: Token program choice table placed before any code in Create Token guide — SPL vs Token-2022 decision is permanent and must be made before any API calls
- [Phase 05-workflow-guides]: Create Token guide shows Token-2022 path first — recommended default for new tokens needing extensions
- [Phase 05-workflow-guides]: FAILED and DROPPED do not retry in backoff loop — only TIMEOUT and stream errors get exponential backoff; reconnect-with-backoff function is the centrepiece of Monitor Transactions guide

### Pending Todos

None yet.

### Blockers/Concerns

- [Research]: Code example validation process unresolved — decide before Phase 3 whether examples are runnable files or inline MDX. Inline examples that drift are worse than no examples.
- [Research]: Exact SDK method signatures not verified against generated code — verify before writing Phase 2 Getting Started code examples.
- [Research]: MonitorTransaction stream termination conditions and SDK-specific consumption patterns need SDK inspection before Phase 3.
- [Research]: CommitmentLevel default (CONFIRMED when unspecified) should be verified against server implementation before it appears in docs.

## Session Continuity

Last session: 2026-03-25T13:00:24.827Z
Stopped at: Completed 05-03-PLAN.md
Resume file: None
