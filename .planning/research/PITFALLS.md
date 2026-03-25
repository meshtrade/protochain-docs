# Domain Pitfalls

**Domain:** gRPC API documentation site (Protochain / Mintlify / Solana)
**Researched:** 2026-03-25

---

## Critical Pitfalls

Mistakes that cause rewrites, developer abandonment, or significant credibility damage.

---

### Pitfall 1: Treating Proto Files as Sufficient Documentation

**What goes wrong:** The proto files exist and are readable, so the team ships API reference pages that are little more than formatted proto definitions — field names, types, and not much else. Developers get the schema but not the semantics.

**Why it happens:** gRPC has no equivalent of OpenAPI's `description` fields baked into code-gen workflows for Mintlify. The path of least resistance is to transcribe the proto. The assumption is that proto field names are self-explanatory.

**Consequences:** Developers can't answer: "What does `commitment_level` actually mean for this call?" or "When should I use `DRAFT` vs `COMPILED`?" They read the source, open GitHub issues, or give up. The docs exist but don't serve their purpose.

**Prevention:** Every field, every enum value, every message needs a human-written sentence explaining its behavioral meaning — not just its type. For Protochain specifically, fields like `commitment_level`, the `TransactionErrorCode` enum entries, and the `TokenProgram` selector all carry domain semantics that proto names don't convey.

**Detection:** If you can write a page entirely from reading the proto without looking at the server implementation, you're probably not writing enough.

**Phase:** Address in the API reference phase. Add a checklist item: "Every field has a behavioral description, not just a type annotation."

---

### Pitfall 2: Under-Documenting the Transaction State Machine

**What goes wrong:** The DRAFT → COMPILED → SIGNED → SUBMITTED lifecycle is central to using Protochain correctly, but it gets buried in a "Core Concepts" page that developers skip. API reference pages reference states without linking back to the lifecycle explanation.

**Why it happens:** Writers assume developers will read sequentially. They don't. Developers land on a method reference, see a state mentioned, and have no immediate context.

**Consequences:** Developers call `SubmitTransaction` on an unsigned transaction, get an error, and can't diagnose it. They don't understand why `CompileTransaction` is a separate step. Integration failures increase; support burden grows.

**Prevention:** The state machine diagram/explanation must appear in Getting Started (not just Core Concepts), be linked from every Transaction Service method that references a state, and be the first thing explained in the Transaction Service overview. Treat the state machine as load-bearing infrastructure for all Transaction documentation.

**Detection:** Can a developer look at just the `SubmitTransaction` reference page and understand what state the transaction must be in before calling it? If not, the state machine is insufficiently surfaced.

**Phase:** Getting Started and Core Concepts must nail this before API reference pages are written.

---

### Pitfall 3: gRPC Connection Setup is Not Obvious to REST-Experienced Developers

**What goes wrong:** The Getting Started guide assumes developers know how to establish a gRPC channel, handle TLS, and use generated stubs. Most external developers come from REST backgrounds where "connect" means constructing a URL.

**Why it happens:** gRPC documentation often skips the channel setup because it's "just the SDK." But for a developer who has never used gRPC, the TLS credential decision, the channel reuse requirement, and the stub instantiation pattern are all unfamiliar.

**Consequences:** Developers hit a wall before making a single API call. "It doesn't work" issues filed before any Protochain logic is exercised. Drop-off in the first 15 minutes of integration.

**Prevention:** The Getting Started guide must include a complete, runnable connection snippet for each SDK language (Go, Rust, TypeScript) that shows: channel creation with the correct address/port, TLS handling or plaintext flag (they're connecting to port 50051), and stub construction. Don't assume the developer has gRPC background.

**Detection:** Show the Getting Started guide to someone who knows REST but has never used gRPC. Did they get to a first successful API call? If not, find the gap.

**Phase:** Getting Started phase. This is the highest-priority content to get right first.

---

### Pitfall 4: Inconsistent Code Examples Across Languages

**What goes wrong:** Go examples are written, then TypeScript examples are added later. By that point, the Go examples have evolved but the TypeScript ones lag behind, or the TypeScript uses a slightly different API shape because the SDK generated different bindings. A developer using Rust finds that the example doesn't compile against the current SDK version.

**Why it happens:** Three SDK languages means three times the maintenance surface. When the proto changes, one language gets updated, others don't. Code examples are written as prose, not tested as code.

**Consequences:** Developers pick a language, find the example broken, assume the whole API is unstable, and lose trust in the documentation entirely. A broken code example is worse than no example.

**Prevention:** Code examples should be stored as runnable files in a `/examples` directory (or alongside the SDK repos) and referenced in the docs rather than authored inline. If that's not feasible, each language example must be reviewed against the actual generated SDK during any API change. Establish a rule: if a proto changes, all three language examples for affected methods must be updated in the same PR.

**Detection:** Try to run every code example verbatim. Does it compile and execute without modification?

**Phase:** Must be addressed as a process decision before writing the first API reference page. Retrofit is expensive.

---

### Pitfall 5: CommitmentLevel and TokenProgram Enums Explained Nowhere

**What goes wrong:** `CommitmentLevel` (processed/confirmed/finalized) and `TokenProgram` (SPL Token vs Token-2022) are shared types used across multiple services. Their meaning is Solana-specific and non-obvious to developers unfamiliar with the Solana ecosystem. They get a shared types reference page that lists the enum values but doesn't explain when to use which.

**Why it happens:** These feel like "reference" content, so they get reference treatment — just list the values. But they're decision points, not just types.

**Consequences:** Developers default to the first/default enum value without understanding the trade-offs. Using `PROCESSED` when `FINALIZED` is needed causes race conditions in production. Mixing `SPL_TOKEN` and `TOKEN_2022` in the same workflow causes failures that are hard to diagnose.

**Prevention:** The shared types page for `CommitmentLevel` must explain the Solana confirmation model (processed = included in a block, confirmed = supermajority confirmed, finalized = irreversible after 31+ blocks). The `TokenProgram` page must explain that SPL Token is the legacy standard and Token-2022 adds extensions, and that the programs are not interchangeable for a given mint.

**Detection:** Can a developer read just the shared types page and make a correct enum selection for a production workflow? Test this with a real developer.

**Phase:** Core Concepts phase. These are foundational to using the API correctly.

---

## Moderate Pitfalls

Mistakes that degrade developer experience and slow integrations, but don't cause total failure.

---

### Pitfall 6: Navigation That Mirrors Internal Service Organization, Not Developer Tasks

**What goes wrong:** The sidebar is organized as: Account Service / Transaction Service / System Program Service / Token Program Service — which matches the proto file organization. Developers don't think in terms of services; they think in terms of tasks ("how do I create a token?" cuts across Token Program and Account Service).

**Why it happens:** Service-per-section feels natural because it matches the code structure. It's also what the team knows.

**Consequences:** A developer following the "create token" workflow has to jump between service sections, losing context and navigation state. Discovery of related methods is poor.

**Prevention:** Add a Guides section that is organized by task/workflow (Create a Token, Transfer SOL, Monitor a Transaction, Handle Errors) that sits alongside the service reference. The reference can still be organized by service. This gives task-oriented developers an entry point without forcing everyone into service-oriented navigation.

**Detection:** Ask a new developer to find "how to mint new tokens to an existing holder account." Count the navigation steps. More than 3 is a problem.

**Phase:** Information architecture must be decided before navigation is built. Retrofit is possible but painful.

---

### Pitfall 7: MonitorTransaction Streaming RPC Is Underdocumented

**What goes wrong:** `MonitorTransaction` is the only streaming RPC in the entire API. Most developers have never used server-side streaming gRPC before. The reference page documents the request/response types but doesn't explain: how to open and read a stream, what events come back and in what order, when the stream closes, and how to handle reconnection.

**Why it happens:** Streaming RPCs look like regular RPCs in the proto. The documentation template gets applied uniformly. The streaming complexity is invisible at the proto level.

**Consequences:** Developers poll `GetTransaction` instead of using the stream, or they open the stream but don't handle stream closure, causing silent failures in long-running monitoring.

**Prevention:** `MonitorTransaction` needs its own dedicated treatment — either a dedicated guide or a significantly expanded reference page. Include a lifecycle diagram (stream opened → events received → stream closed/error → reconnect), concrete code examples for all three SDK languages showing stream consumption, and explicit documentation of the stream termination conditions.

**Detection:** Is there a complete streaming example that a developer can copy-paste and have working monitoring within 10 minutes?

**Phase:** Transaction Service API reference phase. Flag this as requiring extra depth.

---

### Pitfall 8: Error Documentation Treated as an Afterthought

**What goes wrong:** `TransactionErrorCode` enum gets a reference page that lists the values. There's no explanation of what causes each error, what the developer should do when they encounter it, or whether it's retryable.

**Why it happens:** Error codes are boring to document. They're added at the end of a sprint. The list is complete but the operational guidance is missing.

**Consequences:** Developers see an error, look it up in the docs, find only the name, and have to file a support ticket or read the source to understand what it means. Error handling becomes guess-work.

**Prevention:** For each `TransactionErrorCode` entry: document the triggering condition, provide an example scenario, state whether it's retryable, and give a remediation action. This is especially important for state machine violations (trying to compile an already-compiled transaction) and infrastructure errors.

**Detection:** Pick three error codes at random. Can a developer read the docs and know exactly what they did wrong and how to fix it?

**Phase:** Error reference phase (typically late in the API reference work, but the standard should be set early).

---

### Pitfall 9: Mintlify Navigation Drift as Pages Are Added

**What goes wrong:** Pages are created as `.mdx` files but not added to `docs.json`. Or pages are renamed/moved and `docs.json` still references the old path. Both result in broken navigation — 404 pages or sidebar entries that don't work.

**Why it happens:** Mintlify requires every page to be explicitly listed in `docs.json`. Unlike some platforms, it doesn't auto-discover pages. When working quickly, file creation and navigation registration get out of sync.

**Consequences:** Navigation gaps, 404s in the sidebar, pages that exist but are unreachable. This erodes trust in the documentation site's quality.

**Prevention:** Treat `docs.json` as a mandatory co-edit with every page creation. Establish a rule: no `.mdx` file is considered "done" until it appears in `docs.json`. Run `mintlify dev` locally to verify navigation before each commit. The Mintlify CLI will warn on missing/broken page references.

**Detection:** Run a link check after every batch of page additions. Count 404s in the sidebar.

**Phase:** Every phase. This is an ongoing process concern, not a one-time fix.

---

### Pitfall 10: SPL Token vs Token-2022 Conflation

**What goes wrong:** The Token Program Service supports both `SPL_TOKEN` and `TOKEN_2022`. Documentation examples use one without clarifying which, or uses them interchangeably when they're not interchangeable. Developers copy the example, don't notice the `TokenProgram` field, and wonder why their Token-2022 extension calls fail.

**Why it happens:** The programs share most operations at the proto level (same methods, same message structures). The difference is the `token_program` field. It's easy to write examples without specifying it or defaulting to one silently.

**Consequences:** Developers end up with SPL Token mints when they wanted Token-2022 extension features, or vice versa. The error messages don't clearly indicate a program mismatch. Real debugging time lost.

**Prevention:** Every Token Program Service code example must explicitly set the `token_program` field and include a comment explaining why that value is correct for that use case. The Token Program Service overview must lead with a clear explanation of the two programs and when to choose each.

**Phase:** Token Program Service API reference phase.

---

## Minor Pitfalls

Friction points that slow developers down but are recoverable.

---

### Pitfall 11: Image Paths Breaking in Mintlify

**What goes wrong:** Diagrams and screenshots use relative paths (`../images/diagram.png`) that work locally but break in Mintlify's deployed environment.

**Prevention:** All images must use absolute paths from the `/images` (or `/public`) folder root: `/images/diagram.png`. Never use relative paths. Verify in `mintlify dev` before committing.

**Phase:** Any phase involving diagrams. Establish convention on the first diagram added.

---

### Pitfall 12: mint.json vs docs.json Confusion

**What goes wrong:** Tutorials and StackOverflow answers older than early 2025 reference `mint.json`. The project was migrated to `docs.json` in February 2025. Developers applying old tutorials will modify the wrong file or add wrong config keys.

**Prevention:** Know that `docs.json` is the current config file. Ignore all documentation referencing `mint.json`. The structure has changed (tabs, anchors, versions are now a single recursive navigation structure).

**Phase:** Setup/configuration phase.

---

### Pitfall 13: Assuming Developers Know Solana Terminology

**What goes wrong:** Terms like "nonce account," "associated token account (ATA)," "rent exemption," "lamports," "program-derived address (PDA)" appear in the documentation without definition, assuming Solana familiarity.

**Why it happens:** The team knows Solana. External developers may be integrating Protochain as a blockchain-agnostic abstraction and have limited Solana knowledge.

**Consequences:** Developers get confused by RPC Client Service's `GetMinimumBalanceForRentExemption` without understanding what rent is. Account operations make less sense without understanding Solana's account model.

**Prevention:** The Core Concepts page should include a brief Solana primer covering: the account model (accounts own data and hold lamports), rent (accounts must hold SOL to stay alive), and the ATA pattern. Link to the official Solana docs for depth. Don't replicate the full Solana docs — just enough context to make Protochain's API legible.

**Phase:** Core Concepts phase.

---

### Pitfall 14: Getting Started Guide That Doesn't End in a Working Call

**What goes wrong:** The Getting Started guide explains concepts, shows configuration, but the final step is "now call any method" rather than walking through a specific, complete, working API call with expected output.

**Why it happens:** It feels presumptuous to pick one method as canonical. Writers end with a setup section rather than a complete workflow.

**Consequences:** Developers don't have a success moment. They finish Getting Started without having seen the API respond. Motivation to continue drops.

**Prevention:** Getting Started must end with a complete, working example: connect to the server, call `GetAccount` or `GetLatestBlockhash`, and show the exact response. The developer's first session should have a success moment before they explore further.

**Phase:** Getting Started phase.

---

## Phase-Specific Warnings

| Phase Topic | Likely Pitfall | Mitigation |
|-------------|----------------|------------|
| Getting Started | gRPC connection setup assumes REST familiarity | Include complete channel setup code in all 3 languages |
| Core Concepts | State machine and commitment levels explained only once | Embed state machine references in every Transaction Service method |
| Account Service | Fields like `owner`, `executable`, `lamports` need Solana context | Brief Solana account model primer in Core Concepts |
| Transaction Service | `MonitorTransaction` streaming treated like unary RPC | Dedicate a guide section to streaming lifecycle |
| Token Program Service | SPL Token vs Token-2022 conflation in examples | Always explicit `token_program` field with explanatory comment |
| Error Reference | Error codes listed without operational guidance | For each code: trigger, cause, retryability, fix |
| Navigation / Mintlify | `docs.json` drift as pages are added | Co-edit rule: no page is done until registered in `docs.json` |
| All phases | Proto transcription masquerading as documentation | Behavioral description required for every field and enum value |

---

## Sources

- [Most Frequent Mistakes to Avoid in API Documentation — Archbee](https://www.archbee.com/blog/api-documentation-mistakes)
- [Breaking Down Common Documentation Mistakes — Mintlify](https://www.mintlify.com/blog/breaking-down-common-documentation-mistakes)
- [Complete Mintlify Documentation Migration Guide — Hackmamba](https://hackmamba.io/technical-documentation/mintlify-documentation-migration-guide/)
- [Mintlify Navigation Docs](https://www.mintlify.com/docs/organize/navigation)
- [How We Solved Multi-Language SDK Documentation Chaos — Hatchet](https://docs.hatchet.run/blog/automated-documentation)
- [Solana Transaction Commitment Levels — Chainstack](https://chainstack.com/solana-transaction-commitment-levels/)
- [Transaction Confirmation and Expiration — Solana Docs](https://solana.com/developers/guides/advanced/confirmation)
- [SPL Token Standard and Token-2022 Guide — DEXArea](https://dexarea.com/blog/solana/spl-token-standard-solana-token-2022-practical-guide)
- [gRPC Authentication — grpc.io](https://grpc.io/docs/guides/auth/)
- [gRPC Core Concepts — grpc.io](https://grpc.io/docs/what-is-grpc/core-concepts/)
- [10 Common Developer Documentation Mistakes — Document360](https://document360.com/blog/developer-documentation-mistakes/)
- [Building Navigation for Documentation Sites — I'd Rather Be Writing](https://idratherbewriting.com/files/doc-navigation-wtd/design-principles-for-doc-navigation/)
- [API Documentation Best Practices — Fern (Feb 2026)](https://buildwithfern.com/post/api-documentation-best-practices-guide)
