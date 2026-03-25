# Phase 4: Program Services and Error Reference - Context

**Gathered:** 2026-03-25
**Status:** Ready for planning

<domain>
## Phase Boundary

Write complete API reference for System Program Service (11 methods), Token Program Service (6 methods), and RPC Client Service (1 method). Also write the error handling patterns page. All System/Token program methods are instruction builders — they return SolanaInstruction, not execute on-chain.

</domain>

<decisions>
## Implementation Decisions

### Method grouping — System Program
- **D-01:** Group similar methods on shared pages: Create + CreateWithSeed on one page, Allocate + AllocateWithSeed on one page, Assign + AssignWithSeed on one page, Transfer + TransferWithSeed on one page. All 5 nonce operations (Initialize, Authorize, Withdraw, Advance, Upgrade) on one nonce operations page.
- **D-02:** Result: ~6 pages for System Program (overview + Create/CreateWithSeed + Transfer/TransferWithSeed + Allocate/AllocateWithSeed + Assign/AssignWithSeed + Nonce Operations) instead of 12 individual pages.

### Method grouping — Token Program
- **D-03:** Group SPL/Token-2022 pairs on shared pages: CreateToken2022Mint + CreateSPLTokenMint on one page (highlights the choice), CreateToken2022HoldingAccount + CreateSPLTokenHoldingAccount on one page. Mint and ParseMint get individual pages.
- **D-04:** Result: ~5 pages for Token Program (overview + Mint Creation + Holding Account Creation + Mint + ParseMint) instead of 7 individual pages.

### Instruction examples
- **D-05:** Code examples show instruction creation only — the RPC call that creates the SolanaInstruction. End with "Add this instruction to your transaction." Link to transaction lifecycle/Instructions & Transactions concept page for the compile→sign→submit flow.
- **D-06:** Every instruction method page has a brief Note callout: "This method returns a SolanaInstruction. See Instructions & Transactions for how to submit." Links to /concepts/instructions-and-transactions.

### Error handling patterns (ERR-03)
- **D-07:** Error reference page (TransactionErrorCode + TransactionSubmissionCertainty) was already written in Phase 3 at api-reference/errors.mdx. Phase 4 adds an error handling patterns page with code examples showing common error scenarios — this is ERR-03.

### Carried from Phases 1-3
- **D-08 (P3 D-01):** Method pages: request fields (ResponseField) + response fields (ResponseField) + code examples (CodeGroup Go/Rust/TypeScript). No ResponseExample.
- **D-09 (P3 D-02):** Developer-friendly field types: "Base58-encoded public key (string)".
- **D-10 (P1 D-11):** Tab order: Go, Rust, TypeScript.
- **D-11 (P1 D-12):** Minimal focused code examples.
- **D-12 (P3 D-12):** Each service starts with overview page then method pages.
- **D-13 (P1 D-04):** Developer-focused tone — concise, no fluff.

### Claude's Discretion
- Exact page titles for grouped methods (e.g., "Create Account" vs "Create / CreateWithSeed")
- How to present SPL vs Token-2022 on grouped mint/holding pages (tabs, sections, or side-by-side)
- RPC Client page format — only 1 method (GetMinimumBalanceForRentExemption), may combine overview + method
- Error handling patterns page structure and which scenarios to cover

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### System Program proto definitions
- `../protochain/lib/proto/protochain/solana/program/system/v1/service.proto` — All 11 System Program methods

### Token Program proto definitions
- `../protochain/lib/proto/protochain/solana/program/token/v1/service.proto` — All 6 Token Program methods
- `../protochain/lib/proto/protochain/solana/program/token/v1/token2022_extension.proto` — Token-2022 extension types
- `../protochain/lib/proto/protochain/solana/program/token/v1/token2022_extension_metadata.proto` — Extension metadata
- `../protochain/lib/proto/protochain/solana/program/token/v1/token2022_holding_account_extension.proto` — Holding account extensions
- `../protochain/lib/proto/protochain/solana/program/token/v1/spl_token_metadata.proto` — Metaplex metadata
- `../protochain/lib/proto/protochain/solana/program/token/v1/memo_transfer_config.proto` — MemoTransferConfig

### RPC Client proto definitions
- `../protochain/lib/proto/protochain/solana/rpc_client/v1/service.proto` — GetMinimumBalanceForRentExemption

### Error types
- `../protochain/lib/proto/protochain/solana/transaction/v1/error.proto` — TransactionError, TransactionErrorCode (already documented in Phase 3)

### Existing reference pages (link targets, don't duplicate)
- `api-reference/errors.mdx` — Error reference with 24 codes + certainty levels (Phase 3)
- `api-reference/shared-types.mdx` — CommitmentLevel, TokenProgram, KeyPair (Phase 1)
- `concepts/instructions-and-transactions.mdx` — Instruction builder pattern concept (Phase 2)
- `concepts/token-programs.mdx` — SPL vs Token-2022 concept (Phase 2)

### Phase 3 method pages (pattern to follow)
- `api-reference/account/get-account.mdx` — Example of standard method page template
- `api-reference/transaction/overview.mdx` — Example of service overview page

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `snippets/commitment-level-note.mdx` — Use on methods accepting CommitmentLevel
- `snippets/token-program-field.mdx` — Use on token-related methods
- `snippets/base58-note.mdx` — Use on methods with address fields
- Phase 3 method pages — template pattern to replicate

### Established Patterns
- ResponseField for all request/response fields
- CodeGroup with "Go", "Rust", "TypeScript" tab labels
- Service overview pages with methods table
- Warning/Note callouts for gotchas (devnet-only, plaintext keys, etc.)

### Integration Points
- Stub pages exist for all System Program, Token Program, and RPC Client pages under api-reference/
- docs.json navigation already wired (Phase 1)
- Error reference page exists at api-reference/errors.mdx (Phase 3)

</code_context>

<specifics>
## Specific Ideas

- Grouped pages should make the SPL vs Token-2022 choice obvious — developer shouldn't have to read two separate pages to understand which method to call
- Instruction builder Note callout should be brief and consistent — not a mini-explanation of the pattern on every page
- Nonce operations page should explain when you'd use nonces (durable transactions) before listing the methods

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope

</deferred>

---

*Phase: 04-program-services-and-error-reference*
*Context gathered: 2026-03-25*
