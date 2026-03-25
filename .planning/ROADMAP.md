# Roadmap: Protochain Documentation Site

## Overview

Five phases build the documentation site from structural foundation to complete API reference to workflow guides. Each phase unblocks the next: navigation structure must exist before content can be registered, core concepts must exist before method pages can cross-link them, Account and Transaction Service reference must exist before Program service methods make sense, and all reference pages must exist before workflow guides can link to them without forward-references.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [x] **Phase 1: Foundation** - docs.json navigation, landing page, shared types, and reusable snippets (completed 2026-03-25)
- [x] **Phase 2: Getting Started and Core Concepts** - gRPC quickstart, connection setup, and all concept pages (completed 2026-03-25)
- [x] **Phase 3: Account and Transaction Service Reference** - complete API reference for the two most-used services (completed 2026-03-25)
- [x] **Phase 4: Program Services and Error Reference** - System Program, Token Program, RPC Client, and error docs (completed 2026-03-25)
- [x] **Phase 5: Workflow Guides** - end-to-end task-oriented guides compositing all services (completed 2026-03-25)

## Phase Details

### Phase 1: Foundation
**Goal**: Developers land on a correctly structured site with working navigation, a clear landing page, and shared type definitions that all subsequent pages can cross-link to
**Depends on**: Nothing (first phase)
**Requirements**: FOUND-01, FOUND-02, FOUND-03, FOUND-04, FOUND-05, FOUND-06
**Success Criteria** (what must be TRUE):
  1. A developer visiting the site sees Protochain-specific content on every page — no Mintlify placeholder text remains
  2. The sidebar navigation has a Guides tab and an API Reference tab with service-grouped entries that correctly resolve to pages
  3. The shared types page (CommitmentLevel, TokenProgram, KeyPair) exists and is linkable from any other page
  4. MDX snippets for CommitmentLevel, TokenProgram, and per-language gRPC connection setup exist and render correctly when included
  5. The landing page accurately describes what Protochain is and links to Getting Started and API Reference
**Plans**: 3 plans

Plans:
- [x] 01-01-PLAN.md — Restructure docs.json navigation, remove template files, create all stub MDX pages
- [x] 01-02-PLAN.md — Rewrite landing page (index.mdx) with problem-first hero, CTAs, and service listing
- [x] 01-03-PLAN.md — Write shared types reference page and create 6 reusable snippet files

### Phase 2: Getting Started and Core Concepts
**Goal**: A developer new to Protochain can connect to the gRPC server, make a successful first API call, and understand the two load-bearing domain concepts (transaction lifecycle and commitment levels) before reading any method reference
**Depends on**: Phase 1
**Requirements**: START-01, START-02, START-03, CONC-01, CONC-02, CONC-03, CONC-04
**Success Criteria** (what must be TRUE):
  1. A developer can follow the Getting Started guide to establish a gRPC connection and call GetAccount in Go, Rust, or TypeScript using only the docs (no source code reading required)
  2. The transaction lifecycle page shows the full state machine (DRAFT → COMPILED → PARTIALLY_SIGNED → FULLY_SIGNED → SUBMITTED) with an ASCII diagram
  3. The commitment levels page explains the processed/confirmed/finalized trade-offs and when to use each — not just a list of enum values
  4. The token programs page explains SPL Token vs Token-2022 incompatibility and the extension model clearly enough that a developer can choose the correct program for their use case
  5. The instruction builder pattern page explains that System/Token program methods return instructions (not execute on-chain) and how to use those instructions via Transaction Service
**Plans**: 4 plans

Plans:
- [x] 02-01-PLAN.md — Write the Connecting to Protochain page (gRPC channel setup in Go, Rust, TypeScript)
- [x] 02-02-PLAN.md — Write Transaction Lifecycle and Instructions & Transactions concept pages
- [x] 02-03-PLAN.md — Write Commitment Levels and Token Programs concept pages
- [x] 02-04-PLAN.md — Write the Quickstart mini workflow (connect → GetAccount → GenerateNewKeyPair → FundNative)

### Phase 3: Account and Transaction Service Reference
**Goal**: Developers can look up every Account Service and Transaction Service method — including the MonitorTransaction streaming RPC — and find complete field documentation, behavioral semantics, and working code examples in all three SDK languages
**Depends on**: Phase 2
**Requirements**: ACCT-01, ACCT-02, ACCT-03, ACCT-04, ACCT-05, ACCT-06, TXN-01, TXN-02, TXN-03, TXN-04, TXN-05, TXN-06, TXN-07, TXN-08, TXN-09
**Success Criteria** (what must be TRUE):
  1. Every Account Service method (GetAccount, GenerateNewKeyPair, FundNative, GetTokenAccountBalance, GetAssociatedTokenAddress) has a dedicated page with request/response fields, behavioral notes, and Go/Rust/TypeScript code examples
  2. Every Transaction Service method has a dedicated page documenting the state transition it causes, request/response fields, and code examples — including async submission semantics for SubmitTransaction and blockhash freshness for CheckIfTransactionIsExpired
  3. The MonitorTransaction page documents stream lifecycle, all TransactionStatus enum values, reconnection patterns, and stream consumption code in all three languages
  4. The error reference page documents every TransactionErrorCode with its trigger, cause, retryability classification, and remediation guidance — not just enum names
**Plans**: 5 plans

Plans:
- [x] 03-01-PLAN.md — Account Service overview + all 5 Account methods (GetAccount, GenerateNewKeyPair, FundNative, GetTokenAccountBalance, GetAssociatedTokenAddress)
- [x] 03-02-PLAN.md — Transaction Service overview + lifecycle-prep methods (CompileTransaction, EstimateTransaction, SimulateTransaction, SignTransaction, CheckIfTransactionIsExpired)
- [x] 03-03-PLAN.md — Transaction submission methods (SubmitTransaction, GetTransaction)
- [x] 03-04-PLAN.md — MonitorTransaction guide-style page (streaming RPC, stream lifecycle, all 7 TransactionStatus values)
- [x] 03-05-PLAN.md — Error reference page (all 24 TransactionErrorCode values + TransactionSubmissionCertainty)

### Phase 4: Program Services and Error Reference
**Goal**: Developers can look up every System Program, Token Program, and RPC Client method and understand the instruction builder pattern — that these methods return SolanaInstruction objects for use with Transaction Service, not on-chain execution
**Depends on**: Phase 3
**Requirements**: SYS-01, SYS-02, SYS-03, SYS-04, SYS-05, SYS-06, TOK-01, TOK-02, TOK-03, TOK-04, TOK-05, TOK-06, RPC-01, ERR-01, ERR-02, ERR-03
**Success Criteria** (what must be TRUE):
  1. All System Program methods (Create, Transfer, Allocate, Assign, seed-based variants, and all nonce operations) are documented with request/response fields and code examples
  2. All Token Program methods (CreateToken2022Mint, CreateSPLTokenMint, holding account creation, Mint, ParseMint) explicitly show the token_program parameter being set in every code example with an explanatory comment
  3. The Token Program overview page makes the SPL Token vs Token-2022 incompatibility clear before any method page is encountered
  4. The RPC Client Service GetMinimumBalanceForRentExemption method is documented with the rent exemption calculation context and code examples
  5. Error reference covers TransactionSubmissionCertainty levels with a clear explanation of how to handle each certainty scenario
**Plans**: 4 plans

Plans:
- [x] 04-01-PLAN.md — System Program overview + Create/CreateWithSeed + Transfer/TransferWithSeed pages + docs.json grouped navigation
- [x] 04-02-PLAN.md — System Program Allocate/AllocateWithSeed + Assign/AssignWithSeed + Nonce Operations page
- [x] 04-03-PLAN.md — Token Program overview + Mint Creation + Holding Accounts + Mint + ParseMint pages + docs.json Token Program navigation
- [x] 04-04-PLAN.md — RPC Client overview + GetMinimumBalanceForRentExemption + Error Handling Patterns page

### Phase 5: Workflow Guides
**Goal**: Developers can follow task-oriented guides that walk end-to-end through the most common Protochain workflows — each guide composites multiple services and links to the reference pages written in Phases 3 and 4
**Depends on**: Phase 4
**Requirements**: GUIDE-01, GUIDE-02, GUIDE-03
**Success Criteria** (what must be TRUE):
  1. The Transfer SOL guide walks through the complete flow (create accounts → build transfer instruction via System Program → compile → sign → submit → monitor) with working code in at least one language
  2. The Create a Token guide covers both SPL Token and Token-2022 mint creation paths, metadata setup, and minting tokens with explicit token_program selection
  3. The Monitor Transactions guide covers the MonitorTransaction streaming RPC with lifecycle states, reconnection handling, and multi-language stream consumption examples
**Plans**: 3 plans

Plans:
- [x] 05-01-PLAN.md — Transfer SOL end-to-end guide (System Program Transfer → compile → sign → submit → monitor)
- [x] 05-02-PLAN.md — Create a Token guide (SPL Token + Token-2022 mint creation, holding accounts, minting)
- [x] 05-03-PLAN.md — Monitor Transactions guide (stream lifecycle, reconnection with backoff, all terminal states)

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3 → 4 → 5

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Foundation | 3/3 | Complete   | 2026-03-25 |
| 2. Getting Started and Core Concepts | 4/4 | Complete   | 2026-03-25 |
| 3. Account and Transaction Service Reference | 5/5 | Complete   | 2026-03-25 |
| 4. Program Services and Error Reference | 4/4 | Complete   | 2026-03-25 |
| 5. Workflow Guides | 3/3 | Complete   | 2026-03-25 |
