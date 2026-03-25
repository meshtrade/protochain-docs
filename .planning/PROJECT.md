# Protochain Documentation Site

## What This Is

A comprehensive documentation site for Protochain — a Protocol Buffer wrapper that provides language-agnostic gRPC APIs over blockchain SDKs. The docs target external developers integrating with the Protochain API, covering everything from getting started to complete API reference for all gRPC services. Built with Mintlify.

## Core Value

External developers can discover, understand, and successfully integrate with every Protochain gRPC service without needing to read the source code or proto files directly.

## Requirements

### Validated

- ✓ Landing page explains what Protochain is and why it exists — Phase 1
- ✓ Shared types reference documents CommitmentLevel, TokenProgram, KeyPair enums/messages — Phase 1
- ✓ Site navigation restructured with Guides + API Reference tabs — Phase 1
- ✓ Reusable MDX snippets for repeated content — Phase 1
- ✓ Getting started guide walks through connecting to the gRPC server and making a first API call — Phase 2
- ✓ Core concepts page explains the transaction state machine (DRAFT → COMPILED → SIGNED → SUBMITTED) — Phase 2
- ✓ Core concepts page explains commitment levels, token programs, and key domain types — Phase 2

### Active
- [ ] Account Service API reference documents all 5 methods with request/response types
- [ ] Transaction Service API reference documents all lifecycle, submission, and monitoring methods
- [ ] System Program Service API reference documents all account and nonce operations
- [ ] Token Program Service API reference documents mint creation, holding accounts, minting, and parsing
- [ ] RPC Client Service API reference documents the rent exemption method
- [ ] Code examples use multiple SDK languages (Go, Rust, TypeScript)
- [ ] Guides cover common workflows (create token, transfer SOL, monitor transaction)
- [ ] Error reference documents TransactionErrorCode enum and error handling patterns
- [ ] Navigation is logically structured with clear service groupings
- [ ] Site branding matches Protochain identity (colors, logo already configured)

### Out of Scope

- Auto-generated OpenAPI specs — Protochain uses gRPC, not REST
- Interactive API playground — gRPC doesn't support browser-native testing like REST
- SDK installation/build docs — that's the SDK repos' responsibility
- Internal development docs (contributing, code gen, linting) — covered in protochain repo itself
- Non-Solana blockchain documentation — only Solana is currently supported

## Context

- Protochain lives at `../protochain` relative to this docs project
- The API is defined in 17 proto files under `lib/proto/protochain/solana/`
- 5 gRPC services: Account, Transaction, System Program, Token Program, RPC Client
- SDKs are generated for Rust, Go, and TypeScript
- The project follows Google AIP resource-oriented design patterns
- Namespace convention: `protochain.[blockchain].[domain].v1`
- Transaction lifecycle follows a strict state machine
- Token program supports both legacy SPL Token and Token-2022 with extensions
- Backend runs as a Rust gRPC server on port 50051
- MonitorTransaction is the only streaming RPC (server-side streaming)
- Protochain is built by Mesh.Trade

## Constraints

- **Platform**: Mintlify — must work within Mintlify's MDX component system and docs.json navigation structure
- **gRPC API**: No OpenAPI spec available — all API docs must be manually authored from proto definitions
- **Proto source**: Proto files in `../protochain/lib/proto/` are the source of truth for all API documentation
- **Versioning**: All services are v1 — docs should reflect this but be structured to accommodate future versions

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Manual API reference pages over auto-generation | gRPC protos don't have a standard doc-gen pipeline for Mintlify; manual pages allow richer explanations | — Pending |
| Structure by service, not by operation type | Matches how developers think about the API (Account vs Transaction vs Token) | — Pending |
| Multi-language code examples (Go, Rust, TS) | Protochain generates SDKs for all three; external devs may use any | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd:transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd:complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-03-25 after Phase 2 completion*
