# Project Research Summary

**Project:** Protochain Documentation Site
**Domain:** gRPC API documentation for a multi-service Solana blockchain SDK wrapper
**Researched:** 2026-03-25
**Confidence:** HIGH

## Executive Summary

Protochain is a gRPC-native SDK wrapper around the Solana blockchain, exposing five services (Account, Transaction, System Program, Token Program, RPC Client) across Go, Rust, and TypeScript clients. The project is already deployed on Mintlify with a `docs.json` configuration, which constrains — and significantly simplifies — all tooling decisions. The recommended approach is manually authored MDX pages organized into two top-level tabs (Guides, API Reference), with service-oriented grouping in the reference and task-oriented guides that cross service boundaries. Mintlify's built-in components (RequestExample/ResponseExample, ResponseField with Expandable, CodeGroup, Mermaid, Steps) cover the full documentation surface without custom development.

The core challenge is not technical but authorial. Protochain has two domain-specific concepts that developers cannot self-discover from the proto files: the DRAFT → COMPILED → SIGNED → SUBMITTED transaction state machine, and the Solana commitment level model (processed/confirmed/finalized). Both must be treated as load-bearing infrastructure — documented in Core Concepts, embedded in Getting Started, and cross-linked from every Transaction Service method page. Failure to surface these prominently is the single most common cause of integration failure in gRPC blockchain APIs.

The most significant execution risk is code example consistency across three SDK languages. With ~33 method pages and three language tabs each, maintaining correctness as the proto evolves is a process problem, not a content problem. The decision of whether examples are stored as runnable files (validated against actual SDKs) or authored inline must be made before writing the first API reference page. Inline examples that drift from the real SDK are worse than no examples at all.

---

## Key Findings

### Recommended Stack

The stack is fully determined: Mintlify (already active), MDX for page authoring, and Mintlify's built-in component library. No additional packages are needed. The critical component decisions are: use `ResponseField` (not `ParamField`) for all request and response message fields — `ParamField` generates a broken gRPC playground; use `RequestExample`/`ResponseExample` for API reference pages (pinned sidebar layout keeps examples visible during field reference reading); use `CodeGroup` for inline guide code blocks. The `/snippets/` directory (already present) should hold reusable content for `CommitmentLevel` notes, per-language gRPC connection setup, and `base58` address notes — these recur on roughly 15 pages and must not drift independently.

**Core technologies:**
- Mintlify (current docs.json schema): documentation hosting, navigation, rendering — already active, zero additional configuration required
- MDX (Mintlify-managed): page authoring — enables all Mintlify components without build configuration
- ResponseField component: request/response field documentation — avoids the broken API Playground triggered by ParamField
- RequestExample/ResponseExample: per-method code examples — pinned sidebar keeps examples visible while reading parameters
- Mermaid (Mintlify-native): state machine and sequence diagrams — text-as-code, version-controllable, renders natively
- Snippets (/snippets/*.mdx): reusable content blocks — prevents drift for CommitmentLevel, connection setup, and base58 notes

### Expected Features

**Must have (table stakes):**
- Getting Started / quickstart with a working API call — developers decide adoption in the first 10 minutes
- gRPC connection setup per language (Go, Rust, TypeScript) — not self-discoverable by REST-background developers
- Service-organized navigation — developers think by service, not operation type
- Method-level API reference for all ~33 RPC methods — complete field descriptions, code examples, error notes
- Core Concepts: transaction state machine and commitment levels — without these, Transaction Service methods are opaque
- Shared types reference (CommitmentLevel, TokenProgram, KeyPair, SolanaInstruction, Transaction) — cross-linked from every method that uses them
- Error reference (TransactionErrorCode) with cause, retryability, and remediation — not just a list of enum values
- Full-text search — provided by Mintlify out of the box

**Should have (differentiators):**
- Transaction state machine diagram (Mermaid) — dramatically reduces support questions about lifecycle errors
- Task-oriented workflow guides (Transfer SOL, Create Token, Mint Tokens, Monitor Transaction) — cross-service workflows are what developers actually need to accomplish
- Streaming RPC documentation for MonitorTransaction — the only server-side streaming RPC; requires lifecycle diagram and reconnection guidance
- Service overview pages before method pages — prevents developers landing cold on GetAccount with no service context
- Callouts for non-obvious behavior (SubmitTransaction is async, private keys in SignTransaction, CommitmentLevel defaults)
- Token-2022 vs SPL Token clarification on every Token Program method — the two programs are not interchangeable

**Defer (v2+):**
- Workflow guides (high value but reference must be complete first; post-launch addition)
- Streaming reconnect deep-dive guide (basics ship with MonitorTransaction reference; full guide is a second pass)
- Token-2022 extended feature comparison (start as callouts in method pages; expand later)
- Versioned navigation (deferred until v2 services exist)
- Contributing/development docs (belong in the main protochain repo, not here)
- Interactive API playground (gRPC does not work in browsers without significant proxy infrastructure; not worth building)

### Architecture Approach

Two top-level Mintlify tabs separate narrative from reference: Guides (Get Started, Core Concepts, task-oriented workflow guides) and API Reference (Overview, five service groups, Errors). This split matches two distinct developer modes — learning mode and building mode — and prevents concept explanations from bloating method pages. The API Reference tab uses service-oriented groups that map 1:1 to proto package namespaces. Each service group leads with an overview page before individual method pages. The file system has approximately 50 pages total across the two tabs plus a snippets directory.

**Major components:**
1. Landing page + Get Started group — what Protochain is, gRPC connection setup in all 3 languages, first working API call
2. Core Concepts group — transaction state machine (load-bearing), commitment levels, token programs, instructions model, brief Solana primer
3. API Reference Overview + Shared Types — gRPC connection details, CommitmentLevel, TokenProgram, KeyPair, SolanaInstruction, Transaction/TransactionState
4. Per-service overview + method pages — 5 services, ~33 methods, each with request/response fields, code examples, error callouts
5. Errors page — TransactionErrorCode with trigger, cause, retryability, and remediation per entry
6. Workflow guides — 4 task-oriented guides compositing multiple services (written after all reference pages exist)

### Critical Pitfalls

1. **Proto transcription without behavioral semantics** — every field and enum value needs a human-written explanation of its behavioral meaning, not just its type. If a page can be written purely by reading the proto, it is not documented enough.
2. **Transaction state machine buried or under-surfaced** — the state machine must appear in Getting Started, in the Transaction Service overview, and be cross-linked from every Transaction Service method page. Developers do not read sequentially.
3. **gRPC connection setup assumes REST familiarity** — Getting Started must include complete, runnable channel setup code (with TLS/plaintext decision, channel creation, stub instantiation) for Go, Rust, and TypeScript. Do not assume gRPC background.
4. **Inconsistent or broken code examples across languages** — three SDK languages triple maintenance surface. A broken code example destroys trust faster than a missing one. Decide before writing the first page whether examples are stored as runnable files or authored inline; enforce a rule that proto changes require all three language examples to be updated in the same PR.
5. **CommitmentLevel and TokenProgram treated as reference lists** — these are decision points, not just types. CommitmentLevel must explain processed/confirmed/finalized trade-offs. TokenProgram must explain SPL Token vs Token-2022 incompatibility. Every Token Program method example must explicitly set `token_program` with an explanatory comment.

---

## Implications for Roadmap

Based on research, the content dependency graph dictates a clear build order. Later phases cannot be written (or are meaningfully incomplete) without earlier phases. Suggested phase structure:

### Phase 1: Foundation and Navigation Structure
**Rationale:** The shared types page and transaction lifecycle concept are cross-linked by nearly every other page. The docs.json navigation must be wired correctly from the start — Mintlify does not auto-discover pages, and docs.json drift causes broken sidebar navigation throughout the project. Establish the file layout and docs.json structure before writing content.
**Delivers:** `docs.json` fully configured with all tabs and page registrations; `index.mdx` landing page; `api-reference/shared-types.mdx`; `concepts/transaction-lifecycle.mdx`; `api-reference/index.mdx` (connection details); initial snippets (CommitmentLevel note, per-language connection setup)
**Addresses:** Table stakes navigation, shared types reference, state machine documentation
**Avoids:** Pitfall 9 (docs.json drift), Pitfall 2 (state machine under-surfaced)

### Phase 2: Getting Started and Core Concepts
**Rationale:** Developers cannot evaluate or integrate Protochain without a working first call. Core Concepts (commitment levels, token programs, instructions model, Solana primer) must precede API reference pages because method pages reference these concepts rather than explaining them inline.
**Delivers:** `guides/quickstart.mdx`, `guides/connecting.mdx`, `concepts/commitment-levels.mdx`, `concepts/token-programs.mdx`, `concepts/instructions-and-transactions.mdx`
**Addresses:** Table stakes getting started, connection setup, CommitmentLevel and TokenProgram decision guidance
**Avoids:** Pitfall 3 (gRPC connection opacity for REST developers), Pitfall 5 (CommitmentLevel/TokenProgram unexplained), Pitfall 14 (Getting Started without a working call)

### Phase 3: Account Service and Transaction Service Reference
**Rationale:** Account Service is pure queries with no lifecycle complexity — the right warmup for the API reference authoring pattern. Transaction Service is the most complex (8 methods, stateful lifecycle, one streaming RPC) and the most critical to get right. Complete both before Program services, which return SolanaInstruction objects that require the Transaction Service to be useful.
**Delivers:** All Account Service pages (overview + 5 methods), all Transaction Service pages (overview + 8 methods including MonitorTransaction streaming guide), `api-reference/errors.mdx`
**Addresses:** Full API reference for the two most-used services; error documentation with operational guidance; MonitorTransaction streaming lifecycle
**Avoids:** Pitfall 1 (proto transcription), Pitfall 4 (inconsistent code examples), Pitfall 7 (MonitorTransaction underdocumented), Pitfall 8 (error codes without guidance)
**Note:** MonitorTransaction requires extra depth — streaming lifecycle diagram, all 3 SDK language stream consumption examples, termination and reconnection conditions.

### Phase 4: System Program and Token Program Reference
**Rationale:** Both services return SolanaInstruction objects (not execution). This "instruction factory" framing must be established in each service overview before individual method pages. Token Program has Token-2022 vs SPL Token distinction that must be explicit in every example.
**Delivers:** All System Program pages (overview + 13 methods), all Token Program pages (overview + 6 methods), RPC Client pages (overview + 1 method)
**Addresses:** Instruction builder pattern documentation, Token-2022 vs SPL Token clarity
**Avoids:** Pitfall 10 (Token-2022/SPL conflation), Pitfall 1 (proto transcription)

### Phase 5: Workflow Guides
**Rationale:** Guides composite multiple services and link to reference pages. They can only be written after all reference pages exist; writing guides first produces forward-references that lead nowhere. These are high-value differentiators from a pure-reference site.
**Delivers:** `guides/transfer-sol.mdx`, `guides/create-token.mdx`, `guides/mint-tokens.mdx`, `guides/monitor-transaction.mdx`
**Addresses:** Task-oriented developer entry point, cross-service workflow documentation
**Avoids:** Pitfall 6 (navigation that mirrors service structure, not developer tasks)

### Phase Ordering Rationale

- Shared types and transaction lifecycle are written first because they are referenced (not explained) by all subsequent pages — cross-links must resolve immediately.
- Getting Started and Core Concepts precede API reference because method pages link to concepts rather than explain them inline; concepts must exist before method pages are useful.
- Account Service before Transaction Service: simpler service, establishes the per-method page authoring pattern before the stateful/streaming complexity of Transaction Service.
- Transaction Service before Program services: program methods return SolanaInstruction which must be used via Transaction Service — Transaction Service must be documented first for the workflow to make sense.
- Guides last: they depend on all reference pages being complete and accurate.
- docs.json registration is a co-edit with every page creation throughout all phases.

### Research Flags

Phases with well-documented patterns (research-phase not needed):
- **Phase 1 (Foundation):** Mintlify docs.json schema is well-documented; file layout is clearly derived from proto structure
- **Phase 2 (Getting Started/Concepts):** Standard gRPC getting started patterns are well-established across all three SDK languages
- **Phase 4 (System/Token Program):** Instruction builder pattern is straightforward once established in Phase 3

Phases that may benefit from deeper research during planning:
- **Phase 3 (Transaction Service):** MonitorTransaction is the only streaming RPC and the most complex page in the project; streaming behavior across Go, Rust, and TypeScript clients warrants direct SDK inspection before writing
- **Phase 5 (Guides):** Specific SDK usage patterns for end-to-end workflows (especially Token-2022 workflows) should be verified against the actual SDK generated code before authoring guide examples

---

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | HIGH | Mintlify is already active; all component choices verified against Mintlify docs; docs.json in repo confirms current schema |
| Features | HIGH | Cross-verified against Stripe API teardowns, gRPC best practices, Fern documentation guide, Solana ecosystem examples |
| Architecture | HIGH | Derived directly from proto source files and docs.json schema inspection; page counts and method lists are exact |
| Pitfalls | HIGH | Multiple independent sources converge on same failure patterns; Protochain-specific pitfalls confirmed against proto structure |

**Overall confidence:** HIGH

### Gaps to Address

- **Code example validation process:** The decision between runnable example files vs inline MDX authoring was flagged as critical but is a team/process decision, not a research finding. Must be resolved before Phase 3 begins.
- **Actual SDK generated code shape:** Research derived component and field names from proto files; the generated Go/Rust/TypeScript SDK method signatures have not been directly verified. The Getting Started guide code snippets in STACK.md are illustrative — verify exact API shapes against the generated SDK code before authoring.
- **MonitorTransaction stream behavior details:** The proto defines the streaming RPC; the exact event ordering, stream termination conditions, and SDK-specific stream consumption patterns require inspection of the actual server implementation or SDK integration tests.
- **CommitmentLevel defaults:** STACK.md notes that CommitmentLevel defaults to `COMMITMENT_LEVEL_CONFIRMED` when unspecified; this should be verified against the server implementation before it appears in documentation.

---

## Sources

### Primary (HIGH confidence)
- Proto source files: `/Users/kylesmith/Development/protochain/lib/proto/` — direct inspection, source of truth for all method, field, and type documentation
- Project docs.json: `/Users/kylesmith/Development/docs/docs.json` — confirms active Mintlify schema version and current navigation structure
- Mintlify Fields docs: https://mintlify.com/docs/components/fields — ResponseField vs ParamField behavior verified
- Mintlify RequestExample/ResponseExample: https://mintlify.com/docs/components/examples — pinned sidebar layout confirmed
- Mintlify CodeGroup: https://mintlify.com/docs/content/components/code-groups — tab sync behavior confirmed
- Mintlify Mermaid: https://www.mintlify.com/docs/components/mermaid-diagrams — native rendering confirmed
- Mintlify reusable snippets: https://www.mintlify.com/docs/create/reusable-snippets — snippets system confirmed
- Mintlify AI contextual menu: https://www.mintlify.com/docs/ai/contextual-menu — already configured in docs.json

### Secondary (MEDIUM confidence)
- Mintlify navigation: https://www.mintlify.com/docs/organize/navigation — tabs/anchors verified via search (page 404'd)
- Mintlify docs.json migration blog: https://www.mintlify.com/blog/refactoring-mint-json-into-docs-json — confirms docs.json as current schema
- Fern API documentation best practices guide: https://buildwithfern.com/post/api-documentation-best-practices-guide — feature priorities and anti-features
- Solana Yellowstone gRPC docs (Shyft): https://docs.shyft.to/solana-yellowstone-grpc/docs — pattern reference for streaming gRPC blockchain docs
- gRPC error handling: https://grpc.io/docs/guides/error/ — error documentation standards

### Tertiary (MEDIUM-LOW confidence)
- Solana commitment levels: https://chainstack.com/solana-transaction-commitment-levels/ — processed/confirmed/finalized semantics; verify against official Solana docs
- Token-2022 vs SPL Token: https://dexarea.com/blog/solana/spl-token-standard-solana-token-2022-practical-guide — behavioral differences; verify against Solana program documentation

---
*Research completed: 2026-03-25*
*Ready for roadmap: yes*
