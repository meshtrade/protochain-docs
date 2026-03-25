<!-- GSD:project-start source:PROJECT.md -->
## Project

**Protochain Documentation Site**

A comprehensive documentation site for Protochain — a Protocol Buffer wrapper that provides language-agnostic gRPC APIs over blockchain SDKs. The docs target external developers integrating with the Protochain API, covering everything from getting started to complete API reference for all gRPC services. Built with Mintlify.

**Core Value:** External developers can discover, understand, and successfully integrate with every Protochain gRPC service without needing to read the source code or proto files directly.

### Constraints

- **Platform**: Mintlify — must work within Mintlify's MDX component system and docs.json navigation structure
- **gRPC API**: No OpenAPI spec available — all API docs must be manually authored from proto definitions
- **Proto source**: Proto files in `../protochain/lib/proto/` are the source of truth for all API documentation
- **Versioning**: All services are v1 — docs should reflect this but be structured to accommodate future versions
<!-- GSD:project-end -->

<!-- GSD:stack-start source:research/STACK.md -->
## Technology Stack

## Recommended Stack
### Core Platform
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| Mintlify | Current (docs.json schema) | Documentation hosting, rendering, deployment | Already chosen; Git-native CI/CD, MDX component library, AI-ready with llms.txt + MCP auto-generation, zero-config deploy on push |
| MDX | Mintlify-managed | Page authoring | Superset of Markdown; enables Mintlify components inline; no build config required |
### Navigation Structure (docs.json)
### MDX Components for gRPC API Reference Pages
#### `RequestExample` + `ResponseExample` (pinned sidebar layout)
#### `ResponseField` for request/response message documentation
#### `CodeGroup` for inline tutorial code blocks
#### `Expandable` for nested proto message types
#### Mermaid diagrams for state machines and sequence flows
#### `Steps` for lifecycle walkthrough pages
#### Callouts (`Note`, `Warning`, `Info`, `Tip`) for proto-specific nuances
- `<Warning>` on SubmitTransaction: async submission, not confirmation
- `<Warning>` on SignTransaction: private keys in plaintext — not for production
- `<Note>` on MonitorTransaction: only streaming RPC in the entire API
- `<Info>` on CommitmentLevel: defaults to CONFIRMED if not set
### Reusable Snippets System
| Snippet | Reused By |
|---------|-----------|
| `snippets/commitment-level-note.mdx` | Every method that takes a `CommitmentLevel` parameter |
| `snippets/grpc-connection-go.mdx` | Getting started, all Go code setup |
| `snippets/grpc-connection-ts.mdx` | Getting started, all TS code setup |
| `snippets/grpc-connection-rust.mdx` | Getting started, all Rust code setup |
| `snippets/base58-note.mdx` | All methods that take address strings |
| `snippets/token-program-field.mdx` | Token Program Service methods |
### Code Language Support
| Language | Code Fence Tag | SDK Package Pattern |
|----------|---------------|---------------------|
| Go | `go` | `github.com/meshtrade/protochain/lib/go/protochain/solana/[service]/v1` |
| Rust | `rust` | `protochain` crate (tonic-generated) |
| TypeScript | `typescript` or `ts` | `@protochain/solana-[service]-v1` (assumed) |
| Protocol Buffers | `protobuf` | For showing proto definitions as source of truth |
| JSON | `json` | For showing example request/response payloads |
| Shell | `bash` | For showing grpcurl invocations |
### AI-Readiness Features (Already Configured)
- `/llms.txt` — sitemap for AI indexing
- `/llms-full.txt` — entire docs as single markdown file
### Deployment
| Mechanism | Details |
|-----------|---------|
| GitHub push → auto-deploy | Mintlify GitHub App deploys on every push to the connected branch |
| Preview deployments | Available per PR via Mintlify dashboard |
| Custom domain | Configure via Mintlify dashboard DNS settings |
| Local preview | `mintlify dev` via `npm install -g mintlify` |
## Alternatives Considered
| Category | Recommended | Alternative | Why Not |
|----------|-------------|-------------|---------|
| Platform | Mintlify | Docusaurus | Mintlify is already chosen and configured; Docusaurus would require build tooling, MDX config, custom theme work |
| Platform | Mintlify | Fern | Fern has native gRPC/proto support with auto-generation, but is a different platform — project is already on Mintlify |
| API ref | Manual MDX with ResponseField | ParamField | ParamField adds broken API Playground — no REST endpoint exists |
| Code examples | RequestExample/ResponseExample | Inline CodeGroup | Pinned sidebar keeps examples visible while reading parameters — better for reference pages |
| Diagrams | Mermaid (built-in) | Excalidraw embeds | Mermaid is text-as-code, version-controllable, renders natively in Mintlify |
| Snippets | `/snippets/*.mdx` | Inline repetition | CommitmentLevel appears on ~15 pages; single source prevents drift |
## Installation
## Sources
- Mintlify components overview: https://www.mintlify.com/docs/components (MEDIUM — page 404'd, content verified via llms-full.txt)
- Mintlify Fields (ParamField/ResponseField): https://mintlify.com/docs/components/fields (HIGH)
- Mintlify CodeGroup: https://mintlify.com/docs/content/components/code-groups (HIGH)
- Mintlify RequestExample/ResponseExample: https://mintlify.com/docs/components/examples (HIGH)
- Mintlify reusable snippets: https://www.mintlify.com/docs/create/reusable-snippets (HIGH)
- Mintlify navigation: https://www.mintlify.com/docs/organize/navigation (HIGH)
- Mintlify navigation divisions (tabs/anchors): https://mintlify.com/docs/navigation/divisions (MEDIUM — 404'd, content verified via search)
- Mintlify Mermaid: https://www.mintlify.com/docs/components/mermaid-diagrams (HIGH)
- Mintlify AI contextual menu: https://www.mintlify.com/docs/ai/contextual-menu (HIGH)
- Mintlify AI ingestion: https://mintlify.com/docs/ai-ingestion (HIGH)
- Mintlify docs.json refactor blog: https://www.mintlify.com/blog/refactoring-mint-json-into-docs-json (MEDIUM)
- Protochain proto files: `/Users/kylesmith/Development/protochain/lib/proto/` (HIGH — source of truth)
<!-- GSD:stack-end -->

<!-- GSD:conventions-start source:CONVENTIONS.md -->
## Conventions

Conventions not yet established. Will populate as patterns emerge during development.
<!-- GSD:conventions-end -->

<!-- GSD:architecture-start source:ARCHITECTURE.md -->
## Architecture

Architecture not yet mapped. Follow existing patterns found in the codebase.
<!-- GSD:architecture-end -->

<!-- GSD:workflow-start source:GSD defaults -->
## GSD Workflow Enforcement

Before using Edit, Write, or other file-changing tools, start work through a GSD command so planning artifacts and execution context stay in sync.

Use these entry points:
- `/gsd:quick` for small fixes, doc updates, and ad-hoc tasks
- `/gsd:debug` for investigation and bug fixing
- `/gsd:execute-phase` for planned phase work

Do not make direct repo edits outside a GSD workflow unless the user explicitly asks to bypass it.
<!-- GSD:workflow-end -->



<!-- GSD:profile-start -->
## Developer Profile

> Profile not yet configured. Run `/gsd:profile-user` to generate your developer profile.
> This section is managed by `generate-claude-profile` -- do not edit manually.
<!-- GSD:profile-end -->
