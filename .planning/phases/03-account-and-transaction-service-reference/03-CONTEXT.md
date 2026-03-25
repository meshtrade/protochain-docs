# Phase 3: Account and Transaction Service Reference - Context

**Gathered:** 2026-03-25
**Status:** Ready for planning

<domain>
## Phase Boundary

Write complete API reference for Account Service (5 methods) and Transaction Service (8 unary methods + 1 streaming), including service overview pages and individual method pages. Also write the error reference page for TransactionErrorCode. Each method page has field documentation, behavioral notes, and code examples in Go/Rust/TypeScript.

</domain>

<decisions>
## Implementation Decisions

### Method page structure
- **D-01:** Each method page has: request fields (ResponseField components), response fields (ResponseField components), and code examples (CodeGroup with Go/Rust/TypeScript tabs). Clean and scannable.
- **D-02:** Field types use developer-friendly descriptions: "Base58-encoded public key (string)" — describe what the value IS, with proto type in parentheses.
- **D-03:** Response documentation uses ResponseField components only — no example JSON responses in sidebar (no ResponseExample). Developers see fields and types, not raw JSON.

### Streaming RPC treatment
- **D-04:** MonitorTransaction gets a dedicated guide-style page — narrative walkthrough of the streaming lifecycle with code examples showing the full stream consumption loop. Not the same template as unary methods.
- **D-05:** Brief reconnection note on the MonitorTransaction page, linking to the Phase 5 "Monitor Transactions" guide for full production patterns (reconnection, error handling, backoff).

### Error documentation
- **D-06:** Dedicated error reference page at api-reference/errors.mdx with every TransactionErrorCode value. Method pages link to it rather than documenting errors inline.
- **D-07:** Error page uses table format with columns: Error Code | Description | Retryable | Remediation. Scannable and developer-friendly.
- **D-08:** TransactionSubmissionCertainty levels also documented on the error page with handling guidance per scenario.

### Carried from Phase 1 & 2
- **D-09 (P1 D-04):** Developer-focused tone — concise, no fluff.
- **D-10 (P1 D-11):** Tab order: Go, Rust, TypeScript.
- **D-11 (P1 D-12):** Minimal focused code examples — just the API call + response handling. No imports, connection setup, or error handling.
- **D-12 (P1 D-07):** Each service starts with overview page, then individual method pages.
- **D-13 (P1 D-13):** Link to Getting Started for connection setup — no inline boilerplate.
- **D-14 (P1 D-09):** ResponseField (not ParamField) for all parameter/response docs.

### Claude's Discretion
- Exact wording of method descriptions and behavioral notes
- Which methods deserve extra "gotcha" callouts (e.g., FundNative devnet-only, SubmitTransaction async semantics)
- How to present the TransactionStatus enum values on MonitorTransaction page
- Whether service overview pages list methods as cards, table, or simple list

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Account Service proto definitions
- `../protochain/lib/proto/protochain/solana/account/v1/service.proto` — GetAccount, GenerateNewKeyPair, FundNative, GetTokenAccountBalance, GetAssociatedTokenAddress
- `../protochain/lib/proto/protochain/solana/account/v1/account.proto` — Account message, KeyPair message

### Transaction Service proto definitions
- `../protochain/lib/proto/protochain/solana/transaction/v1/service.proto` — CompileTransaction, EstimateTransaction, SimulateTransaction, SignTransaction, CheckIfTransactionIsExpired, SubmitTransaction, GetTransaction, MonitorTransaction
- `../protochain/lib/proto/protochain/solana/transaction/v1/transaction.proto` — Transaction message, TransactionState enum, TransactionConfig
- `../protochain/lib/proto/protochain/solana/transaction/v1/instruction.proto` — SolanaInstruction, SolanaAccountMeta
- `../protochain/lib/proto/protochain/solana/transaction/v1/error.proto` — TransactionError, TransactionErrorCode, TransactionSubmissionCertainty, SubmissionResult

### Shared types (link target, don't duplicate)
- `api-reference/shared-types.mdx` — CommitmentLevel, TokenProgram, KeyPair already documented

### Concept pages (link target for cross-references)
- `concepts/transaction-lifecycle.mdx` — State machine walkthrough
- `concepts/instructions-and-transactions.mdx` — Instruction builder pattern
- `concepts/commitment-levels.mdx` — Commitment level trade-offs
- `concepts/token-programs.mdx` — SPL vs Token-2022

### Research
- `.planning/research/STACK.md` — ResponseField not ParamField, RequestExample/ResponseExample patterns, CodeGroup tab sync

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `snippets/commitment-level-note.mdx`: Info callout linking to shared types — use on methods that accept CommitmentLevel
- `snippets/token-program-field.mdx`: ResponseField with incompatibility warning — use on token-related methods
- `snippets/base58-note.mdx`: Base58 encoding note — use on methods with address fields
- Existing connection snippets: link to guides/connecting.mdx per D-13

### Established Patterns
- ResponseField component for all request/response field docs (Phase 1 research)
- CodeGroup with exactly "Go", "Rust", "TypeScript" labels for tab sync
- Stub pages already exist for all service overview and method pages (Phase 1)

### Integration Points
- docs.json navigation already wired for Account Service and Transaction Service groups (Phase 1)
- All stub .mdx files exist under api-reference/account/ and api-reference/transaction/
- Error reference page stub at api-reference/errors.mdx (if exists) or needs creation

</code_context>

<specifics>
## Specific Ideas

- Method pages should be scannable — developer lands on a method, sees params, sees response, sees code example. No narrative wall of text.
- MonitorTransaction is the exception — it needs the streaming lifecycle explained because developers won't know how to consume a gRPC stream from just params/response docs.
- Error page should be the one place a developer goes to understand "what went wrong" — scannable table, not an explanation essay.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope

</deferred>

---

*Phase: 03-account-and-transaction-service-reference*
*Context gathered: 2026-03-25*
