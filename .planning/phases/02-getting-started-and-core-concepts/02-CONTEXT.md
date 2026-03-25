# Phase 2: Getting Started and Core Concepts - Context

**Gathered:** 2026-03-25
**Status:** Ready for planning

<domain>
## Phase Boundary

Write the Getting Started content (connecting page + quickstart) and all Core Concepts pages (transaction lifecycle, commitment levels, token programs, instructions & transactions). Developer should be able to connect, make a first workflow, and understand the load-bearing domain concepts before reading any API method reference.

</domain>

<decisions>
## Implementation Decisions

### Quickstart flow
- **D-01:** Quickstart is a mini workflow: connect to gRPC server → call GetAccount → generate new keypair → fund with devnet SOL. Developer ends up with a working dev environment.
- **D-02:** Split into two pages: "Connecting" (standalone gRPC connection setup per language) and "Quickstart" (the mini workflow that links to Connecting). Connection page gets reused by method docs via D-13 from Phase 1.
- **D-03:** Endpoint/environment setup details deferred — don't prescribe local vs. remote. Focus on the gRPC client setup and API calls.

### Concept page depth
- **D-04:** Transaction lifecycle page is a practical guide — explain the flow with a "typical transaction" walkthrough. States covered in context, not exhaustively.
- **D-05:** Concept pages are purely conceptual — no code examples. Explain ideas, link to relevant method pages for code. Keeps concept pages focused and short.
- **D-06:** Instruction builder pattern gets its own dedicated "Instructions & Transactions" page. Explains what instructions are, how they compose, and how they feed into Transaction Service. Not merged into lifecycle page.

### Diagram style
- **D-07:** Transaction state machine uses ASCII art diagram, not Mermaid. Renders everywhere without tooling dependency.
- **D-08:** Only the transaction lifecycle page gets a diagram. Other concept pages are straightforward enough with text only.

### Carried from Phase 1
- **D-09 (Phase 1 D-04):** Developer-focused tone — concise, no marketing fluff, code-forward.
- **D-10 (Phase 1 D-11):** Tab order: Go, Rust, TypeScript — consistent across all code groups.
- **D-11 (Phase 1 D-12):** Code examples minimal and focused — just the API call + response handling.

### Claude's Discretion
- Exact structure and ordering of content within each concept page
- How to explain commitment level trade-offs (table, prose, or comparison format)
- SPL Token vs Token-2022 explanation approach on the token programs page
- Whether connecting page needs language-specific subsections or tabs

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Proto definitions (source of truth for domain concepts)
- `../protochain/lib/proto/protochain/solana/transaction/v1/transaction.proto` — Transaction message, TransactionState enum (DRAFT, COMPILED, PARTIALLY_SIGNED, FULLY_SIGNED)
- `../protochain/lib/proto/protochain/solana/transaction/v1/service.proto` — Transaction Service methods (lifecycle flow)
- `../protochain/lib/proto/protochain/solana/transaction/v1/instruction.proto` — SolanaInstruction, SolanaAccountMeta
- `../protochain/lib/proto/protochain/solana/type/v1/commitment_level.proto` — CommitmentLevel enum
- `../protochain/lib/proto/protochain/solana/type/v1/token_program.proto` — TokenProgram enum
- `../protochain/lib/proto/protochain/solana/account/v1/service.proto` — GetAccount, GenerateNewKeyPair, FundNative (used in quickstart)

### Existing site content
- `api-reference/shared-types.mdx` — Already-written shared types page with behavioral descriptions (Phase 1). Concept pages should link here, not duplicate.
- `snippets/grpc-connection-go.mdx`, `snippets/grpc-connection-rust.mdx`, `snippets/grpc-connection-ts.mdx` — Connection boilerplate snippets (Phase 1). Connecting page should expand on these.

### Research
- `.planning/research/STACK.md` — Mintlify component recommendations
- `.planning/research/PITFALLS.md` — gRPC connection wall for REST developers, state machine as load-bearing infrastructure

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `snippets/grpc-connection-go.mdx`, `snippets/grpc-connection-rust.mdx`, `snippets/grpc-connection-ts.mdx`: Connection boilerplate already created — connecting page can expand on these
- `snippets/commitment-level-note.mdx`: Reusable callout linking to shared types page
- `api-reference/shared-types.mdx`: Behavioral descriptions for CommitmentLevel, TokenProgram, KeyPair already written

### Established Patterns
- All pages use MDX with frontmatter (title, description)
- CodeGroup tabs with labels "Go", "Rust", "TypeScript" for auto-sync
- ResponseField component for type documentation (not ParamField)

### Integration Points
- Stub pages already exist at: `guides/quickstart.mdx`, `guides/connecting.mdx`, `concepts/transaction-lifecycle.mdx`, `concepts/commitment-levels.mdx`, `concepts/token-programs.mdx`, `concepts/instructions-and-transactions.mdx`
- docs.json navigation already wired for these paths (Phase 1)

</code_context>

<specifics>
## Specific Ideas

- Quickstart should feel like a "5-minute proof of concept" — developer proves the stack works, gets a funded keypair, and is ready to build
- Concept pages explain ideas and link out — they're not reference material or tutorials
- Transaction lifecycle walkthrough should feel like "here's what a typical transaction looks like" not "here's every possible state transition"

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope

</deferred>

---

*Phase: 02-getting-started-and-core-concepts*
*Context gathered: 2026-03-25*
