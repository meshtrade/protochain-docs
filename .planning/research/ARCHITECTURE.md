# Architecture Patterns

**Domain:** gRPC API documentation site (Mintlify, Protochain / Solana)
**Researched:** 2026-03-25
**Confidence:** HIGH — derived directly from proto source files and Mintlify docs.json schema

---

## Recommended Architecture

A three-tab Mintlify site:

```
Tab 1: Guides          — Narrative content. Gets developers to "hello world" fast.
Tab 2: API Reference   — Service-grouped method pages. The daily reference.
Tab 3: (optional later) Concepts — Deep domain explanations extracted from Guides if it grows.
```

Keep Guides and API Reference as separate tabs. Mixing narrative and reference content in the same nav group makes both harder to navigate. Developers in "learning mode" use Guides; developers in "building mode" use API Reference.

---

## Navigation Structure

### Full docs.json Navigation Map

```json
{
  "navigation": {
    "tabs": [
      {
        "tab": "Guides",
        "groups": [
          {
            "group": "Get Started",
            "pages": [
              "index",
              "guides/quickstart",
              "guides/connecting"
            ]
          },
          {
            "group": "Core Concepts",
            "pages": [
              "concepts/overview",
              "concepts/transaction-lifecycle",
              "concepts/commitment-levels",
              "concepts/token-programs",
              "concepts/instructions-and-transactions"
            ]
          },
          {
            "group": "Guides",
            "pages": [
              "guides/transfer-sol",
              "guides/create-token",
              "guides/mint-tokens",
              "guides/monitor-transaction"
            ]
          }
        ]
      },
      {
        "tab": "API Reference",
        "groups": [
          {
            "group": "Overview",
            "pages": [
              "api-reference/index",
              "api-reference/shared-types"
            ]
          },
          {
            "group": "Account Service",
            "pages": [
              "api-reference/account/overview",
              "api-reference/account/get-account",
              "api-reference/account/generate-new-key-pair",
              "api-reference/account/fund-native",
              "api-reference/account/get-token-account-balance",
              "api-reference/account/get-associated-token-address"
            ]
          },
          {
            "group": "Transaction Service",
            "pages": [
              "api-reference/transaction/overview",
              "api-reference/transaction/compile-transaction",
              "api-reference/transaction/estimate-transaction",
              "api-reference/transaction/simulate-transaction",
              "api-reference/transaction/sign-transaction",
              "api-reference/transaction/check-if-transaction-is-expired",
              "api-reference/transaction/submit-transaction",
              "api-reference/transaction/get-transaction",
              "api-reference/transaction/monitor-transaction"
            ]
          },
          {
            "group": "System Program Service",
            "pages": [
              "api-reference/system-program/overview",
              "api-reference/system-program/create",
              "api-reference/system-program/transfer",
              "api-reference/system-program/allocate",
              "api-reference/system-program/assign",
              "api-reference/system-program/create-with-seed",
              "api-reference/system-program/allocate-with-seed",
              "api-reference/system-program/assign-with-seed",
              "api-reference/system-program/transfer-with-seed",
              "api-reference/system-program/initialize-nonce-account",
              "api-reference/system-program/authorize-nonce-account",
              "api-reference/system-program/withdraw-nonce-account",
              "api-reference/system-program/advance-nonce-account",
              "api-reference/system-program/upgrade-nonce-account"
            ]
          },
          {
            "group": "Token Program Service",
            "pages": [
              "api-reference/token-program/overview",
              "api-reference/token-program/create-token2022-mint",
              "api-reference/token-program/create-spl-token-mint",
              "api-reference/token-program/parse-mint",
              "api-reference/token-program/create-token2022-holding-account",
              "api-reference/token-program/create-spl-token-holding-account",
              "api-reference/token-program/mint"
            ]
          },
          {
            "group": "RPC Client Service",
            "pages": [
              "api-reference/rpc-client/overview",
              "api-reference/rpc-client/get-minimum-balance-for-rent-exemption"
            ]
          },
          {
            "group": "Errors",
            "pages": [
              "api-reference/errors"
            ]
          }
        ]
      }
    ]
  }
}
```

---

## Service Organization Strategy

**Organize by service, not by operation type.** This matches how developers integrate: they connect to one service at a time and look up all its methods together. The service grouping also maps 1:1 to the proto package namespaces developers already know.

The five services map cleanly to five nav groups in the API Reference tab:

| Nav Group | Proto Package | Method Count | Character |
|-----------|--------------|--------------|-----------|
| Account Service | `protochain.solana.account.v1` | 5 | Query + key utilities |
| Transaction Service | `protochain.solana.transaction.v1` | 8 | Stateful lifecycle + streaming |
| System Program Service | `protochain.solana.program.system.v1` | 13 | Instruction builders (returns `SolanaInstruction`) |
| Token Program Service | `protochain.solana.program.token.v1` | 6 | Instruction builders + token-specific queries |
| RPC Client Service | `protochain.solana.rpc_client.v1` | 1 | Utility / rent queries |

Each service group gets an `overview` page (its purpose, when to use it, connection endpoint) before the individual method pages. This prevents the nav from dropping readers directly into `GetAccount` with no framing.

---

## Component Boundaries

| Component | Responsibility | Page Count |
|-----------|---------------|------------|
| `index` (landing) | What Protochain is, why it exists, quick orientation | 1 |
| Get Started group | Connect to server, make first call, dev environment setup | 2-3 |
| Core Concepts group | Transaction state machine, commitment levels, token programs, instructions model | 4-5 |
| Guides group | End-to-end workflows combining multiple services | 3-5 |
| API Reference Overview | gRPC connection details, shared types (CommitmentLevel, TokenProgram, KeyPair, SolanaInstruction) | 2 |
| Per-service overview pages | Service purpose, RPC list summary, common patterns | 5 (one per service) |
| Per-method reference pages | Request/response types, field descriptions, code examples, errors | 33 methods total |
| Errors page | TransactionErrorCode enum, error classification model, retry logic | 1 |

---

## Content Dependencies

These dependencies determine what must exist before other pages can reference it. They dictate build order.

```
Shared Types page
  └── CommitmentLevel enum
  └── TokenProgram enum
  └── KeyPair message
  └── SolanaInstruction message
  └── Transaction message + TransactionState enum
        └── ALL other service method pages reference these types

Transaction Lifecycle concept page
  └── TransactionState (DRAFT → COMPILED → PARTIALLY_SIGNED → FULLY_SIGNED)
        └── Transaction Service method pages (each documents required state)
        └── Guides that involve building transactions

System Program Service overview
  └── "returns SolanaInstruction" explanation
        └── All 13 System Program method pages
        └── Token Program method pages (same pattern)

Token Program overview
  └── Token2022 vs SPL Token distinction
        └── Token2022-specific method pages
        └── SPL-specific method pages
        └── ParseMint page

Error Classification concept (permanent/temporary/indeterminate)
  └── TransactionErrorCode page
        └── SubmitTransaction page (references SubmissionResult + TransactionError)
        └── MonitorTransaction page (references TransactionStatus)
```

**Key dependency**: The `SolanaInstruction` type is the return value of every System Program and Token Program method. Readers landing on any of those 19 method pages without understanding what `SolanaInstruction` is and how to use it in a `Transaction` will be lost. The Shared Types page and Transaction Lifecycle concept page must exist before those method pages are useful.

---

## Suggested Build Order

Build in this sequence to maximize each phase's standalone usefulness and avoid forward-references that lead nowhere.

### Phase 1: Foundation (unblock everything else)

These pages are referenced by nearly all other content. Write them first.

1. `index` — landing page, what Protochain is
2. `api-reference/shared-types` — CommitmentLevel, TokenProgram, KeyPair, SolanaInstruction, Transaction + TransactionState
3. `concepts/transaction-lifecycle` — DRAFT → COMPILED → SIGNED → SUBMITTED state machine
4. `api-reference/index` — connection details (server address, port 50051, TLS, auth)

Rationale: Every service method page will link back to shared types and the lifecycle. Writing these first means method pages can cross-link immediately rather than leaving TODO placeholders.

### Phase 2: Core Service Reference

Write services in dependency order: Account first (pure queries, no lifecycle complexity), then Transaction (the core stateful service), then programs (which produce instructions consumed by Transaction).

5. `api-reference/account/overview` + 5 Account method pages
6. `api-reference/transaction/overview` + 8 Transaction method pages
7. `api-reference/errors` — TransactionErrorCode, SubmissionResult, retry logic
8. `api-reference/system-program/overview` + 13 System Program method pages
9. `api-reference/token-program/overview` + 6 Token Program method pages
10. `api-reference/rpc-client/overview` + 1 RPC Client method page

### Phase 3: Guides and Concepts

Guides synthesize multiple services into workflows. They can only be written after the method pages exist to link to.

11. `concepts/commitment-levels` — processed/confirmed/finalized tradeoffs
12. `concepts/token-programs` — SPL Token vs Token-2022 distinctions
13. `concepts/instructions-and-transactions` — how instructions compose into transactions
14. `guides/quickstart` — connect and call GetAccount
15. `guides/transfer-sol` — System Program + Transaction Service end-to-end
16. `guides/create-token` — Token Program + Transaction Service end-to-end
17. `guides/mint-tokens` — follows create-token
18. `guides/monitor-transaction` — SubmitTransaction + MonitorTransaction streaming

---

## Transaction Service: Special Considerations

The Transaction Service has two distinct operation groups that should be visually separated within its overview page (not split into separate nav groups — that adds friction):

**Composition workflow** (must run in order):
```
CompileTransaction (DRAFT → COMPILED)
EstimateTransaction (optional, COMPILED state)
SimulateTransaction (optional, COMPILED state)
SignTransaction (COMPILED → FULLY_SIGNED)
CheckIfTransactionIsExpired (FULLY_SIGNED, before submit)
SubmitTransaction (FULLY_SIGNED → on-chain)
```

**Retrieval and monitoring**:
```
GetTransaction (by signature)
MonitorTransaction (streaming — only streaming RPC in the entire API)
```

`MonitorTransaction` is the only server-side streaming RPC. Its method page needs explicit treatment of streaming client behavior across Go, Rust, and TypeScript SDKs. This is the most complex page to write.

---

## System Program and Token Program: Instruction Builder Pattern

All 13 System Program methods and all 6 Token Program methods return `SolanaInstruction` (or `repeated SolanaInstruction` for the compound mint/holding-account creation methods). This is a deliberate API pattern: these services are instruction factories, not transaction executors.

The documentation must make this pattern explicit at the service overview level before readers hit individual method pages. Without this framing, developers ask "why doesn't Transfer actually transfer anything?" The answer — instructions must be composed into a Transaction and submitted via the Transaction Service — must appear in the System Program and Token Program overview pages, not buried in method pages.

The Token Program service has two "flavors" for most operations (Token-2022 vs SPL Token). Organize each method page to compare the two variants rather than treating them as entirely separate methods. Use tabs or side-by-side code blocks.

---

## Shared Types Page: What to Include

The `api-reference/shared-types` page is a reference anchor for the entire API. It must cover:

| Type | Package | What to document |
|------|---------|-----------------|
| `CommitmentLevel` | `protochain.solana.type.v1` | Enum values, speed/reliability tradeoffs, default behavior (UNSPECIFIED) |
| `TokenProgram` | `protochain.solana.type.v1` | LEGACY vs 2022, when to use each, UNSPECIFIED is an error |
| `KeyPair` | `protochain.solana.type.v1` | Fields, encoding format |
| `SolanaInstruction` | `protochain.solana.transaction.v1` | Structure, how it flows into Transaction.instructions |
| `SolanaAccountMeta` | `protochain.solana.transaction.v1` | is_signer, is_writable semantics |
| `Transaction` | `protochain.solana.transaction.v1` | Full message structure, per-field state lifecycle |
| `TransactionState` | `protochain.solana.transaction.v1` | Enum values, valid transitions |
| `TransactionConfig` | `protochain.solana.transaction.v1` | Compute budget fields, validation options |

Do NOT document `TransactionError` and `TransactionErrorCode` here — those belong on the dedicated Errors page where they can be documented in depth with retry logic guidance.

---

## File System Layout

```
docs/
├── index.mdx                          # Landing page
├── concepts/
│   ├── overview.mdx
│   ├── transaction-lifecycle.mdx
│   ├── commitment-levels.mdx
│   ├── token-programs.mdx
│   └── instructions-and-transactions.mdx
├── guides/
│   ├── quickstart.mdx
│   ├── connecting.mdx
│   ├── transfer-sol.mdx
│   ├── create-token.mdx
│   ├── mint-tokens.mdx
│   └── monitor-transaction.mdx
└── api-reference/
    ├── index.mdx                      # Connection details
    ├── shared-types.mdx               # All shared types
    ├── errors.mdx                     # TransactionErrorCode + retry logic
    ├── account/
    │   ├── overview.mdx
    │   ├── get-account.mdx
    │   ├── generate-new-key-pair.mdx
    │   ├── fund-native.mdx
    │   ├── get-token-account-balance.mdx
    │   └── get-associated-token-address.mdx
    ├── transaction/
    │   ├── overview.mdx
    │   ├── compile-transaction.mdx
    │   ├── estimate-transaction.mdx
    │   ├── simulate-transaction.mdx
    │   ├── sign-transaction.mdx
    │   ├── check-if-transaction-is-expired.mdx
    │   ├── submit-transaction.mdx
    │   ├── get-transaction.mdx
    │   └── monitor-transaction.mdx
    ├── system-program/
    │   ├── overview.mdx
    │   ├── create.mdx
    │   ├── transfer.mdx
    │   ├── allocate.mdx
    │   ├── assign.mdx
    │   ├── create-with-seed.mdx
    │   ├── allocate-with-seed.mdx
    │   ├── assign-with-seed.mdx
    │   ├── transfer-with-seed.mdx
    │   ├── initialize-nonce-account.mdx
    │   ├── authorize-nonce-account.mdx
    │   ├── withdraw-nonce-account.mdx
    │   ├── advance-nonce-account.mdx
    │   └── upgrade-nonce-account.mdx
    ├── token-program/
    │   ├── overview.mdx
    │   ├── create-token2022-mint.mdx
    │   ├── create-spl-token-mint.mdx
    │   ├── parse-mint.mdx
    │   ├── create-token2022-holding-account.mdx
    │   ├── create-spl-token-holding-account.mdx
    │   └── mint.mdx
    └── rpc-client/
        ├── overview.mdx
        └── get-minimum-balance-for-rent-exemption.mdx
```

Total: ~50 pages.

---

## Anti-Patterns to Avoid

### Anti-Pattern 1: Mixing Concepts into API Reference Method Pages

**What goes wrong:** A method page for `CompileTransaction` starts with three paragraphs explaining what a blockhash is and how the transaction state machine works.

**Why bad:** Method pages become bloated. Developers using them as daily reference have to scroll past explanations they already know. The explanation lives in two places and gets out of sync.

**Instead:** Method pages reference concept pages with a single sentence and a link. Keep method pages focused on: required state, request fields, response fields, errors, code examples.

### Anti-Pattern 2: Separate Nav Groups for Token-2022 vs SPL Token

**What goes wrong:** Creating a "Token-2022 Methods" group and an "SPL Token Methods" group in the nav, doubling the nav entries.

**Why bad:** Developers choosing between the two programs have to jump between nav sections to compare. The methods are conceptually parallel, not independent.

**Instead:** One nav group "Token Program Service," one page per method, each page documents both variants with tabs or a version selector when behavior differs.

### Anti-Pattern 3: RPC Client Service as a Standalone Tab

**What goes wrong:** Giving `GetMinimumBalanceForRentExemption` its own top-level tab because it's a distinct service.

**Why bad:** One method does not justify a full tab. Tabs in Mintlify carry significant visual weight and imply comparable scope to other tabs.

**Instead:** RPC Client is its own group within API Reference. Two pages: overview + the single method.

### Anti-Pattern 4: No Service Overview Pages

**What goes wrong:** Nav jumps directly to method pages: Account Service > GetAccount, GenerateNewKeyPair, etc. with no overview page.

**Why bad:** Developers landing on a service for the first time have no framing for what the service does, when to use it, or how its methods relate to each other.

**Instead:** Each service group leads with an `overview` page that describes the service's purpose, lists all RPCs with one-line summaries, and notes key behavioral patterns (e.g., "all methods return SolanaInstruction — they do not execute on-chain directly").

---

## Scalability Considerations

| Concern | Current (v1) | Future (v2 added) |
|---------|-------------|-----------------|
| Versioning in nav | Single nav, all v1 | Add version selector or separate tabs |
| New services | Add new group to API Reference tab | Low friction — groups are independent |
| New blockchain (e.g., Ethereum) | N/A | New top-level tab per chain, or version selector |
| Method page count growing | 33 methods, manageable | Group by sub-domain within service if > ~15 methods |

The current v1 structure is clean enough that versioning can be deferred. When v2 arrives, Mintlify's versioning support should handle tab-level version switching without restructuring the file layout.

---

## Sources

- Proto source files: `/Users/kylesmith/Development/protochain/lib/proto/protochain/solana/` (direct inspection — HIGH confidence)
- Mintlify docs.json schema: `/Users/kylesmith/Development/docs/docs.json` (direct inspection — HIGH confidence)
- Project requirements: `/Users/kylesmith/Development/docs/.planning/PROJECT.md` (direct inspection — HIGH confidence)
