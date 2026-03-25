# Phase 1: Foundation - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-03-25
**Phase:** 01-foundation
**Areas discussed:** Landing page content, Navigation structure, Shared types depth, Code example style

---

## Landing page content

| Option | Description | Selected |
|--------|-------------|----------|
| Problem-first | Lead with the pain point: 'Best blockchain SDK is in the wrong language?' | ✓ |
| Capability-first | Lead with what it does: 'Language-agnostic gRPC API for Solana' | |
| You decide | Claude picks the best approach | |

**User's choice:** Problem-first
**Notes:** None

| Option | Description | Selected |
|--------|-------------|----------|
| Quickstart + API ref | Two cards: 'Get Started' and 'API Reference' | ✓ |
| Quickstart only | Single prominent CTA | |
| Full overview | Four entry point cards | |

**User's choice:** Quickstart + API ref

| Option | Description | Selected |
|--------|-------------|----------|
| Yes, with descriptions | List all 5 services with one-line descriptions and links | ✓ |
| Brief mention | Mention services without detail | |
| No service list | Keep landing page high-level | |

**User's choice:** Yes, with descriptions

| Option | Description | Selected |
|--------|-------------|----------|
| Developer-focused | Concise, no marketing fluff, code-forward | ✓ |
| Friendly technical | Warm but precise | |
| You decide | Claude picks the tone | |

**User's choice:** Developer-focused

---

## Navigation structure

| Option | Description | Selected |
|--------|-------------|----------|
| Two groups | Getting Started + Core Concepts | |
| Three groups | Getting Started + Core Concepts + Guides | ✓ |
| You decide | Claude structures it | |

**User's choice:** Three groups

| Option | Description | Selected |
|--------|-------------|----------|
| One group per service | 5 groups: Account, Transaction, System, Token, RPC | ✓ |
| Domain grouping | 3 groups: Core, Programs, Utilities | |
| Flat with overview | Single group with all services | |

**User's choice:** One group per service

| Option | Description | Selected |
|--------|-------------|----------|
| Overview first | Overview page per service then methods | ✓ |
| Methods only | Jump straight to method pages | |

**User's choice:** Overview first

| Option | Description | Selected |
|--------|-------------|----------|
| Own group in API tab | Separate 'Reference' group for shared types + errors | ✓ |
| Under guides | Put in Guides tab under Core Concepts | |
| You decide | Claude places them | |

**User's choice:** Own group in API tab

---

## Shared types depth

| Option | Description | Selected |
|--------|-------------|----------|
| Behavioral guidance | Each value gets description + when to use + trade-offs | ✓ |
| Fields + values only | Brief descriptions, link to Core Concepts | |
| You decide | Claude determines depth | |

**User's choice:** Behavioral guidance

| Option | Description | Selected |
|--------|-------------|----------|
| Yes, per type | Usage example in all 3 languages per type | |
| Reference only | Just document types, code in method pages | ✓ |
| Minimal | One brief example per type, single language | |

**User's choice:** Reference only

---

## Code example style

| Option | Description | Selected |
|--------|-------------|----------|
| Go, Rust, TypeScript | Go first — most common in blockchain backend | ✓ |
| TypeScript, Go, Rust | TypeScript first — largest dev audience | |
| Rust, Go, TypeScript | Rust first — matches backend language | |

**User's choice:** Go, Rust, TypeScript

| Option | Description | Selected |
|--------|-------------|----------|
| Minimal focused | Just the API call + response handling | ✓ |
| Copy-pasteable | Full imports + connection + call + error handling | |
| Focused + imports | Imports and call, skip connection | |

**User's choice:** Minimal focused

| Option | Description | Selected |
|--------|-------------|----------|
| Link only | Link to Getting Started for connection setup | ✓ |
| Collapsible snippet | Expandable Prerequisites section | |
| You decide | Claude picks best approach | |

**User's choice:** Link only

---

## Claude's Discretion

- Exact hero copy and page layout
- Card styling and spacing on landing page
- Placeholder page strategy for undocumented services
- Snippet content for reusable field descriptions

## Deferred Ideas

None — discussion stayed within phase scope
