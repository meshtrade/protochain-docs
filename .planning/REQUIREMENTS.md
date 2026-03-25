# Requirements: Protochain Documentation Site

**Defined:** 2026-03-25
**Core Value:** External developers can discover, understand, and successfully integrate with every Protochain gRPC service without needing to read the source code or proto files directly.

## v1 Requirements

### Foundation

- [x] **FOUND-01**: docs.json navigation restructured with Guides tab and API Reference tab, service-grouped navigation
- [x] **FOUND-02**: All Mintlify template/placeholder content removed and replaced with Protochain-specific content
- [x] **FOUND-03**: Landing page explains what Protochain is, links to getting started and API reference
- [x] **FOUND-04**: Navbar links updated (remove Mintlify defaults, add relevant Protochain links)
- [x] **FOUND-05**: Shared types reference page documents CommitmentLevel, TokenProgram, KeyPair with behavioral descriptions
- [x] **FOUND-06**: Reusable MDX snippets created for repeated field descriptions (commitment level, token program, connection boilerplate)

### Getting Started

- [x] **START-01**: Getting started page walks through gRPC connection setup in Go, Rust, and TypeScript
- [x] **START-02**: Getting started includes a complete "first API call" example (e.g., GetAccount) in all three languages
- [x] **START-03**: Getting started explains what Protochain is and when to use it (language-agnostic blockchain SDK access)

### Core Concepts

- [x] **CONC-01**: Transaction lifecycle page explains state machine (DRAFT → COMPILED → PARTIALLY_SIGNED → FULLY_SIGNED → SUBMITTED) with visual diagram
- [x] **CONC-02**: Core concepts page explains Solana commitment levels (Processed, Confirmed, Finalized) and when to use each
- [x] **CONC-03**: Core concepts page explains token programs (SPL Token vs Token-2022) and the extension model
- [x] **CONC-04**: Core concepts page explains the instruction builder pattern (System/Token program methods return instructions, not execute on-chain)

### Account Service Reference

- [x] **ACCT-01**: Account Service overview page with service description and method listing
- [x] **ACCT-02**: GetAccount method documented with request/response fields, code examples in Go/Rust/TypeScript
- [x] **ACCT-03**: GenerateNewKeyPair method documented with request/response fields, code examples
- [x] **ACCT-04**: FundNative method documented with request/response fields, devnet-only note, code examples
- [x] **ACCT-05**: GetTokenAccountBalance method documented with request/response fields, code examples
- [x] **ACCT-06**: GetAssociatedTokenAddress method documented with request/response fields, token program distinction, code examples

### Transaction Service Reference

- [x] **TXN-01**: Transaction Service overview page with service description, state machine reference, and method listing
- [x] **TXN-02**: CompileTransaction method documented with state transition (DRAFT → COMPILED), request/response fields, code examples
- [x] **TXN-03**: EstimateTransaction method documented with fee estimation details, request/response fields, code examples
- [x] **TXN-04**: SimulateTransaction method documented with dry-run semantics, request/response fields, code examples
- [x] **TXN-05**: SignTransaction method documented with state transitions, multi-signer support, request/response fields, code examples
- [x] **TXN-06**: CheckIfTransactionIsExpired method documented with blockhash freshness semantics, code examples
- [x] **TXN-07**: SubmitTransaction method documented with async submission semantics, SubmissionResult enum, error handling, code examples
- [x] **TXN-08**: GetTransaction method documented with request/response fields, code examples
- [x] **TXN-09**: MonitorTransaction streaming method documented with stream lifecycle, TransactionStatus enum, reconnection patterns, code examples

### System Program Service Reference

- [x] **SYS-01**: System Program Service overview page with instruction builder pattern explanation and method listing
- [x] **SYS-02**: Create method documented with request/response fields, code examples
- [x] **SYS-03**: Transfer method documented with request/response fields, code examples
- [x] **SYS-04**: Allocate and Assign methods documented with request/response fields, code examples
- [x] **SYS-05**: Seed-based variants (CreateWithSeed, AllocateWithSeed, AssignWithSeed, TransferWithSeed) documented
- [x] **SYS-06**: Nonce operations (InitializeNonceAccount, AuthorizeNonceAccount, WithdrawNonceAccount, AdvanceNonceAccount, UpgradeNonceAccount) documented

### Token Program Service Reference

- [x] **TOK-01**: Token Program Service overview page with SPL Token vs Token-2022 distinction and method listing
- [x] **TOK-02**: CreateToken2022Mint method documented with extension support, request/response fields, code examples
- [x] **TOK-03**: CreateSPLTokenMint method documented with Metaplex metadata, request/response fields, code examples
- [x] **TOK-04**: CreateToken2022HoldingAccount and CreateSPLTokenHoldingAccount methods documented with code examples
- [x] **TOK-05**: Mint method documented with token-program agnostic behavior, decimal amount handling, code examples
- [x] **TOK-06**: ParseMint method documented with extension parsing, MintInfo response, code examples

### RPC Client Service Reference

- [x] **RPC-01**: RPC Client Service overview and GetMinimumBalanceForRentExemption method documented with code examples

### Error Reference

- [x] **ERR-01**: Error reference page documents TransactionErrorCode enum with all error codes, descriptions, and retryable classification
- [x] **ERR-02**: Error reference explains TransactionSubmissionCertainty levels and how to handle each
- [x] **ERR-03**: Error handling patterns page with code examples for common error scenarios

### Workflow Guides

- [x] **GUIDE-01**: Transfer SOL guide walks through end-to-end: create accounts → build transfer instruction → compile → sign → submit → monitor
- [x] **GUIDE-02**: Create a Token guide walks through mint creation (SPL + Token-2022), metadata setup, and minting tokens
- [x] **GUIDE-03**: Monitor Transactions guide covers streaming MonitorTransaction RPC with lifecycle, reconnection, and multi-language examples

## v2 Requirements

### Advanced Guides

- **ADV-01**: Multi-signer transaction guide covering PARTIALLY_SIGNED flow
- **ADV-02**: Nonce-based durable transactions guide
- **ADV-03**: Token-2022 extensions deep dive (transfer hooks, transfer fees, interest-bearing)

### Developer Experience

- **DX-01**: SDK-specific installation and setup pages per language
- **DX-02**: Changelog/versioning page for API updates
- **DX-03**: Migration guide from direct Solana SDK to Protochain

## Out of Scope

| Feature | Reason |
|---------|--------|
| Interactive API playground | gRPC is not browser-compatible; no clean solution exists |
| Auto-generated API docs from protos | Proto comments are sparse; manual docs provide richer explanations |
| OpenAPI/Swagger specs | Protochain is gRPC-native, not REST |
| Non-Solana blockchain docs | Only Solana is currently supported |
| SDK build/installation docs | Responsibility of the SDK repos, not the API docs |
| Internal dev docs (contributing, code gen) | Covered in the protochain repo itself |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| FOUND-01 | Phase 1 | Complete |
| FOUND-02 | Phase 1 | Complete |
| FOUND-03 | Phase 1 | Complete |
| FOUND-04 | Phase 1 | Complete |
| FOUND-05 | Phase 1 | Complete |
| FOUND-06 | Phase 1 | Complete |
| START-01 | Phase 2 | Complete |
| START-02 | Phase 2 | Complete |
| START-03 | Phase 2 | Complete |
| CONC-01 | Phase 2 | Complete |
| CONC-02 | Phase 2 | Complete |
| CONC-03 | Phase 2 | Complete |
| CONC-04 | Phase 2 | Complete |
| ACCT-01 | Phase 3 | Complete |
| ACCT-02 | Phase 3 | Complete |
| ACCT-03 | Phase 3 | Complete |
| ACCT-04 | Phase 3 | Complete |
| ACCT-05 | Phase 3 | Complete |
| ACCT-06 | Phase 3 | Complete |
| TXN-01 | Phase 3 | Complete |
| TXN-02 | Phase 3 | Complete |
| TXN-03 | Phase 3 | Complete |
| TXN-04 | Phase 3 | Complete |
| TXN-05 | Phase 3 | Complete |
| TXN-06 | Phase 3 | Complete |
| TXN-07 | Phase 3 | Complete |
| TXN-08 | Phase 3 | Complete |
| TXN-09 | Phase 3 | Complete |
| SYS-01 | Phase 4 | Complete |
| SYS-02 | Phase 4 | Complete |
| SYS-03 | Phase 4 | Complete |
| SYS-04 | Phase 4 | Complete |
| SYS-05 | Phase 4 | Complete |
| SYS-06 | Phase 4 | Complete |
| TOK-01 | Phase 4 | Complete |
| TOK-02 | Phase 4 | Complete |
| TOK-03 | Phase 4 | Complete |
| TOK-04 | Phase 4 | Complete |
| TOK-05 | Phase 4 | Complete |
| TOK-06 | Phase 4 | Complete |
| RPC-01 | Phase 4 | Complete |
| ERR-01 | Phase 4 | Complete |
| ERR-02 | Phase 4 | Complete |
| ERR-03 | Phase 4 | Complete |
| GUIDE-01 | Phase 5 | Complete |
| GUIDE-02 | Phase 5 | Complete |
| GUIDE-03 | Phase 5 | Complete |

**Coverage:**
- v1 requirements: 46 total
- Mapped to phases: 46
- Unmapped: 0 ✓

---
*Requirements defined: 2026-03-25*
*Last updated: 2026-03-25 after initial definition*
