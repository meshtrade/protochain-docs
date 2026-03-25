# Phase 5: Workflow Guides - Context

**Gathered:** 2026-03-25
**Status:** Ready for planning

<domain>
## Phase Boundary

Write three end-to-end workflow guides that composite multiple services: Transfer SOL, Create a Token, and Monitor Transactions. These are the final content pieces — all reference pages they link to already exist.

</domain>

<decisions>
## Implementation Decisions

### Guide completeness
- **D-01:** Code examples use focused snippets — same minimal style as reference pages. Each step shows the key API call, links to reference pages for full details. Not copy-pasteable runnable code.
- **D-02:** Guides use narrative prose with code blocks inline — not Steps component. More natural reading experience for workflow walkthroughs.

### Guide scope
- **D-03:** Transfer SOL guide covers happy path only: create accounts → build transfer instruction → compile → sign → submit → monitor. Straight line, no error recovery detours.
- **D-04:** Create a Token guide covers both SPL Token and Token-2022 paths with explicit token_program selection (from requirements GUIDE-02).
- **D-05:** Monitor Transactions guide is the definitive streaming deep-dive: full reconnection logic, error handling, backoff, and lifecycle management. Production-quality patterns. This is what the MonitorTransaction reference page links to for "full treatment."

### Carried from prior phases
- **D-06 (P1 D-04):** Developer-focused tone — concise, no fluff.
- **D-07 (P1 D-11):** Tab order: Go, Rust, TypeScript.
- **D-08 (P1 D-12):** Minimal focused code examples.

### Claude's Discretion
- Exact narrative structure and flow for each guide
- Whether Create Token guide covers metadata setup in detail or links to Token Program reference
- How to present the SPL vs Token-2022 choice in the Create Token guide (inline or separate sections)
- Monitor Transactions guide: exact reconnection backoff strategy and code patterns

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Reference pages to link to (all exist from Phases 3-4)
- `api-reference/system-program/transfer.mdx` — Transfer/TransferWithSeed (Transfer SOL guide)
- `api-reference/system-program/create.mdx` — Create account (Transfer SOL guide)
- `api-reference/transaction/compile-transaction.mdx` — CompileTransaction
- `api-reference/transaction/sign-transaction.mdx` — SignTransaction
- `api-reference/transaction/submit-transaction.mdx` — SubmitTransaction
- `api-reference/transaction/monitor-transaction.mdx` — MonitorTransaction (streaming reference, links to guide)
- `api-reference/token-program/mint-creation.mdx` — CreateToken2022Mint + CreateSPLTokenMint (Create Token guide)
- `api-reference/token-program/mint.mdx` — Mint tokens (Create Token guide)
- `api-reference/token-program/holding-accounts.mdx` — Holding account creation (Create Token guide)
- `api-reference/errors.mdx` — Error reference (Monitor Transactions guide)
- `api-reference/error-handling-patterns.mdx` — Error handling patterns (Monitor Transactions guide)

### Concept pages to link to
- `concepts/transaction-lifecycle.mdx` — State machine (all guides)
- `concepts/instructions-and-transactions.mdx` — Instruction builder pattern (Transfer SOL, Create Token)
- `concepts/token-programs.mdx` — SPL vs Token-2022 (Create Token guide)

### Proto definitions (for accurate API usage in examples)
- `../protochain/lib/proto/protochain/solana/program/system/v1/service.proto` — Transfer method
- `../protochain/lib/proto/protochain/solana/program/token/v1/service.proto` — Token methods
- `../protochain/lib/proto/protochain/solana/transaction/v1/service.proto` — Transaction lifecycle + MonitorTransaction

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `snippets/commitment-level-note.mdx` — Use where commitment level matters
- `snippets/token-program-field.mdx` — Use in Create Token guide
- Quickstart page (`guides/quickstart.mdx`) — establishes the CodeGroup pattern for guides

### Established Patterns
- CodeGroup with "Go", "Rust", "TypeScript" tab labels
- Narrative prose with inline code blocks (decided in D-02)
- Warning/Note callouts for gotchas

### Integration Points
- Stub pages exist: `guides/transfer-sol.mdx`, `guides/create-token.mdx`, `guides/monitor-transaction.mdx`
- docs.json Guides tab already has a "Guides" group with these pages (Phase 1)

</code_context>

<specifics>
## Specific Ideas

- Transfer SOL guide should feel like "your first real transaction" — the natural next step after the quickstart
- Create Token guide should make the SPL vs Token-2022 choice obvious and early — don't let the developer get halfway through before realizing they picked wrong
- Monitor Transactions guide is the most technical guide — production reconnection patterns are what developers actually need in production, not just "here's how to read from a stream"

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope

</deferred>

---

*Phase: 05-workflow-guides*
*Context gathered: 2026-03-25*
