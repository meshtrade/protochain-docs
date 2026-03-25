# Feature Landscape

**Domain:** gRPC API documentation site for blockchain SDK wrapper (Protochain)
**Researched:** 2026-03-25
**Confidence:** HIGH (cross-verified across Stripe teardowns, Mintlify platform docs, gRPC official docs, Fern best practices guide, and Solana ecosystem documentation examples)

---

## Table Stakes

Features that external developers expect. Missing or poor execution here causes immediate abandonment.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| Getting started / quickstart | Developers need a working integration before committing; first 10 minutes determines adoption | Low | Should produce a real API call, not just hello world. Connection to port 50051 + one RPC call. |
| Authentication and connection setup | gRPC requires TLS, channel credentials, and per-language client initialization — developers cannot self-discover this | Low–Med | Cover Go, Rust, TypeScript clients separately. Channel setup differs per language. |
| Service-organized navigation | Developers think by service (Account, Transaction, Token) not by operation type | Low | Mintlify sidebar grouping. Must be immediately parseable on first visit. |
| Method-level API reference | Every RPC method needs: description, request fields, response fields, at least one code example | High | 5 services, ~20+ methods. Manually authored from proto files. The bulk of the work. |
| Request/response field documentation | Field names, types, required vs optional, valid values, constraints | Med | Mintlify `<Fields>` and `<Responses>` components cover this natively. |
| Multi-language code examples | Go, Rust, and TypeScript SDKs all exist; developers choose their language | Med | Mintlify `<CodeGroup>` with tabs. Must stay in sync when proto changes. |
| Error reference | gRPC status codes + Protochain's `TransactionErrorCode` enum; developers will hit errors and need to self-serve | Med | Document all error codes, what triggers them, and how to recover. |
| Core concepts page | Transaction state machine, commitment levels, token programs — Protochain has domain-specific concepts not obvious from proto alone | Med | DRAFT → COMPILED → SIGNED → SUBMITTED is non-obvious. Diagram strongly recommended. |
| Search | Developers scan by method name or field name; full-text search prevents frustration | Low | Mintlify provides this out of the box. No custom work needed. |
| Copy-to-clipboard on code | Universal expectation; missing it feels broken | Low | Mintlify provides automatically on code blocks. |

---

## Differentiators

Features that set Protochain docs apart from typical proto-dumped API references. Not universally expected, but valued highly by developers.

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| Transaction state machine diagram | The DRAFT → COMPILED → SIGNED → SUBMITTED flow is the core of integrating correctly; a visual diagram dramatically reduces support questions | Med | Use Mintlify Mermaid diagram support. No external tooling needed. |
| Workflow guides (not just reference) | Common tasks like "create a token", "transfer SOL", "monitor a transaction" cross multiple services; task-oriented guides reduce integration time | Med | 3–4 guides covering the most common developer workflows. Requires understanding real use cases. |
| Streaming RPC documentation (MonitorTransaction) | MonitorTransaction is the only server-side streaming RPC; streaming behavior, error propagation, and reconnection patterns are genuinely different from unary and developers often get them wrong | Med | Cover gRPC trailing metadata, error-in-trailer behavior, and reconnect logic. Solana Yellowstone docs pattern is a good reference. |
| Shared types reference | CommitmentLevel, TokenProgram, KeyPair — used across all services; having a single canonical reference page prevents "which values are valid?" questions | Low | Cross-link from every method that uses these types. |
| Callouts for non-obvious behavior | Blockchain-specific gotchas (commitment level semantics, nonce vs blockhash, token extensions) that cannot be inferred from field names | Low | Mintlify `<Warning>`, `<Note>`, `<Tip>` components. High value per unit of effort. |
| Token-2022 vs legacy SPL token clarification | Token Program supports both; the behavioral differences are material for developers building against token accounts | Med | Dedicated section or callout on every token-related method. |
| Service overview pages | Each service gets a brief "what this service does and when you'd use it" page before the method reference | Low | Prevents developers from reading every method before understanding scope. |
| Google AIP design notes | Protochain follows Google AIP resource-oriented patterns; calling this out sets developer expectations and helps experienced gRPC developers orient faster | Low | A single note in Core Concepts or Getting Started is enough. |

---

## Anti-Features

Features to explicitly NOT build or document.

| Anti-Feature | Why Avoid | What to Do Instead |
|--------------|-----------|-------------------|
| Interactive API playground | gRPC does not work in browsers natively; attempting this requires grpc-web + proxy setup, adds significant complexity, and creates a maintenance burden | Show copy-pasteable code examples with real gRPC client setup. Link to grpcurl or Kreya for manual testing. |
| Auto-generated proto docs (protoc-gen-doc style) | Proto comments are often terse or absent; auto-generated output produces developer-hostile walls of field names without explanation | Manually author reference pages. The extra effort produces dramatically better docs. |
| OpenAPI/Swagger integration | Protochain is gRPC, not REST; OpenAPI is not applicable and adding it causes confusion about the actual protocol | Make the gRPC-first nature explicit on the landing page. |
| SDK installation and build instructions | This is the SDK repos' responsibility (Rust crate, Go module, npm package); duplicating it in these docs will drift out of sync | Link to each SDK repo. Document only the parts required to connect to the Protochain gRPC server. |
| Non-Solana blockchain coverage | Only Solana is currently supported; documenting future/hypothetical chains creates confusion and maintenance debt | Namespace convention (`protochain.solana.*`) makes scope explicit. |
| Changelog or versioning history | All services are v1; a changelog adds overhead for what is currently a static API | Note the v1 namespace. When v2 exists, add versioned navigation. |
| Contributing / development docs | Internal docs belong in the protochain repo; putting them here conflates external developer audience with internal contributors | Keep these in the main repo README and CONTRIBUTING.md. |

---

## Feature Dependencies

The following ordering constraints exist between features:

```
Connection setup docs → ALL method reference pages (developers cannot use code examples without knowing how to connect)

Core concepts (state machine) → Transaction Service reference (the lifecycle methods only make sense if the state machine is understood first)

Core concepts (commitment levels) → Account Service, Transaction Service, RPC Client Service (CommitmentLevel appears in many methods)

Shared types reference → All service references (cross-links require the destination to exist)

Workflow guides → All service references (guides composite multiple services; references must exist first)

Service overview pages → Method-level reference pages (overviews go above the method list in each service section)
```

---

## MVP Recommendation

For an initial launch that serves external developers immediately:

**Prioritize:**
1. Landing page (what Protochain is and why it exists)
2. Getting started — connection setup + first API call in all 3 languages
3. Core concepts — state machine and commitment levels (these unlock everything else)
4. Complete API reference for all 5 services with all fields documented
5. Error reference for TransactionErrorCode
6. Shared types reference

**Defer until post-launch:**
- Workflow guides (high value but depend on the reference being solid first)
- Transaction state machine diagram (useful but the text explanation can ship first)
- Token-2022 vs SPL extended comparison (can start as a callout, expand later)
- Streaming reconnect patterns (document the basics; full reconnect guide is a second pass)

---

## Mintlify Platform Capabilities (Relevant to Feature Decisions)

Mintlify natively provides the following, requiring no custom development:
- Full-text search (Algolia-powered)
- Code block copy buttons
- Mermaid diagram rendering (flowcharts, sequence diagrams — relevant for state machine)
- `<CodeGroup>` tabs for multi-language examples
- `<Fields>` and `<Responses>` components for API reference
- `<Tabs>` for organizing related content
- Callout components: `<Warning>`, `<Note>`, `<Tip>`, `<Info>`
- `<Steps>` for sequential instructions (useful for getting started)
- `<Accordion>` for FAQ-style content
- `<Cards>` for service landing pages
- AI assistant (Claude-powered) that searches and cites documentation content

**Mintlify does NOT provide:**
- gRPC playground / interactive testing (REST-only via OpenAPI)
- Auto-generation from proto files (OpenAPI-only)

---

## Sources

- [API Documentation Best Practices Guide — Fern/BuildWithFern](https://buildwithfern.com/post/api-documentation-best-practices-guide)
- [Why Stripe's API Docs Are the Benchmark — APIdog](https://apidog.com/blog/stripe-docs/)
- [8 API Documentation Best Practices — DeepDocs](https://deepdocs.dev/api-documentation-best-practices/)
- [Mintlify Components Overview](https://www.mintlify.com/docs/components)
- [Solana Yellowstone gRPC Docs — Shyft](https://docs.shyft.to/solana-yellowstone-grpc/docs)
- [Transaction Lifecycle — Provenance Blockchain](https://developer.provenance.io/docs/pb/blockchain/basics/transaction-lifecycle/)
- [Error Handling — gRPC Official Docs](https://grpc.io/docs/guides/error/)
- [Core Concepts and Lifecycle — gRPC Official Docs](https://grpc.io/docs/what-is-grpc/core-concepts/)
- [gRPC Best Practices — Kreya](https://kreya.app/blog/grpc-best-practices/)
