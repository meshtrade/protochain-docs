# Phase 1: Foundation - Context

**Gathered:** 2026-03-25
**Status:** Ready for planning

<domain>
## Phase Boundary

Restructure the Mintlify docs site with correct navigation (Guides + API Reference tabs), replace all template content with Protochain-specific content, create the landing page, shared types reference page, and reusable MDX snippets. No actual service documentation or guides — those are Phase 2+.

</domain>

<decisions>
## Implementation Decisions

### Landing page content
- **D-01:** Hero section leads with the problem: "Best blockchain SDK is in the wrong language? Protochain wraps it with gRPC." Problem-first messaging.
- **D-02:** Two primary CTAs: "Get Started" linking to quickstart, "API Reference" linking to service overview.
- **D-03:** Landing page lists all 5 gRPC services (Account, Transaction, System Program, Token Program, RPC Client) with one-line descriptions and links to their reference pages.
- **D-04:** Tone is developer-focused — concise, no marketing fluff, code-forward (like Stripe or Vercel docs).

### Navigation structure
- **D-05:** Guides tab has three groups: "Getting Started" (intro, quickstart), "Core Concepts" (transaction lifecycle, commitment levels, etc.), "Guides" (transfer SOL, create token, etc.).
- **D-06:** API Reference tab has one group per service — 5 service groups: Account Service, Transaction Service, System Program, Token Program, RPC Client.
- **D-07:** Each service group starts with an overview page (what it does, method listing), then individual method pages underneath.
- **D-08:** Shared Types and Error Reference live in their own "Reference" group within the API Reference tab.

### Shared types depth
- **D-09:** Shared types page provides behavioral guidance — each enum value gets a description, when to use it, and trade-offs (e.g., "Use FINALIZED when funds must be irreversibly settled").
- **D-10:** No code snippets on the shared types page — code examples live in the service method pages that use these types.

### Code example style
- **D-11:** Tab order is Go, Rust, TypeScript — consistent across all code groups site-wide.
- **D-12:** Code examples are minimal and focused — show just the API call + response handling. Omit imports, connection setup, error handling.
- **D-13:** Connection setup is handled via link to Getting Started page — no inline snippets or collapsible sections on method pages.

### Claude's Discretion
- Exact hero copy and page structure layout
- Card component styling and spacing on landing page
- How to handle placeholder pages for services not yet documented (Phase 2+)
- Snippet content for reusable field descriptions (CommitmentLevel, TokenProgram)

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Proto definitions (source of truth for shared types)
- `../protochain/lib/proto/protochain/solana/type/v1/commitment_level.proto` — CommitmentLevel enum values and semantics
- `../protochain/lib/proto/protochain/solana/type/v1/token_program.proto` — TokenProgram enum (SPL Token vs Token-2022)
- `../protochain/lib/proto/protochain/solana/type/v1/keypair.proto` — KeyPair message structure

### Project context
- `.planning/PROJECT.md` — Project vision, core value, constraints
- `.planning/REQUIREMENTS.md` — FOUND-01 through FOUND-06 requirements for this phase
- `.planning/research/STACK.md` — Mintlify component recommendations (ResponseField not ParamField, CodeGroup tab sync, snippets)
- `.planning/research/ARCHITECTURE.md` — Recommended navigation structure and content dependencies

### Existing site
- `docs.json` — Current Mintlify config (needs full restructure)
- `index.mdx` — Current landing page (Mintlify template, needs complete rewrite)

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `snippets/snippet-intro.mdx`: Existing snippet file — can be repurposed or replaced with Protochain snippets
- `docs.json`: Already has Protochain name, colors (primary: #CAFFB9, light: #66A182, dark: #2E4057), and logo paths configured

### Established Patterns
- Mintlify uses MDX with frontmatter (title, description) — all pages follow this pattern
- Navigation defined entirely in docs.json via tabs > groups > pages hierarchy
- CodeGroup component with matching tab labels auto-syncs across page

### Integration Points
- `docs.json` navigation.tabs — full restructure needed for Guides + API Reference
- `docs.json` navbar.links — remove Mintlify defaults, add Protochain links (GitHub repo, support)
- Template files to remove: essentials/*, ai-tools/*, api-reference/endpoint/*, quickstart.mdx, development.mdx

</code_context>

<specifics>
## Specific Ideas

- Landing page should feel like Stripe or Vercel docs — developer-focused, code-forward, no fluff
- Problem-first hero: communicate the language barrier problem Protochain solves
- Service listing on landing page gives developers immediate visibility into the API surface

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope

</deferred>

---

*Phase: 01-foundation*
*Context gathered: 2026-03-25*
