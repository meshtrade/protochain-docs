# Phase 2: Getting Started and Core Concepts - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-03-25
**Phase:** 02-getting-started-and-core-concepts
**Areas discussed:** Quickstart flow, Concept page depth, Diagram style

---

## Quickstart flow

| Option | Description | Selected |
|--------|-------------|----------|
| First successful call | Connect + GetAccount only | |
| Mini workflow | Connect + GetAccount + keypair + fund devnet | ✓ |
| Full taste | Connect + keypair + fund + transfer SOL | |

**User's choice:** Mini workflow

| Option | Description | Selected |
|--------|-------------|----------|
| Local (docker-compose) | Start with docker-compose up | |
| Remote endpoint | Connect to hosted devnet | |
| Both paths | Show both options | |

**User's choice:** Other — "Ignore for now"
**Notes:** Endpoint setup details deferred

| Option | Description | Selected |
|--------|-------------|----------|
| Single page | One page covers everything | |
| Split pages | Connecting standalone, Quickstart links to it | ✓ |

**User's choice:** Split pages

---

## Concept page depth

| Option | Description | Selected |
|--------|-------------|----------|
| Comprehensive | Full exhaustive state machine explanation | |
| Practical guide | Typical transaction walkthrough | ✓ |
| Quick reference | Diagram + brief descriptions + links | |

**User's choice:** Practical guide

| Option | Description | Selected |
|--------|-------------|----------|
| Conceptual only | No code, link to method pages | ✓ |
| Light code | One brief snippet per concept | |
| You decide | Claude determines per page | |

**User's choice:** Conceptual only

| Option | Description | Selected |
|--------|-------------|----------|
| Own page | Dedicated Instructions & Transactions page | ✓ |
| Section in lifecycle | Add to transaction lifecycle page | |

**User's choice:** Own page

---

## Diagram style

| Option | Description | Selected |
|--------|-------------|----------|
| Mermaid | Renders natively in Mintlify | |
| ASCII art | Renders everywhere, no tooling | ✓ |
| You decide | Claude picks | |

**User's choice:** ASCII art

| Option | Description | Selected |
|--------|-------------|----------|
| Lifecycle only | Only state machine needs a visual | ✓ |
| Where helpful | Claude adds where they clarify | |
| Every concept page | Each page gets at least one visual | |

**User's choice:** Lifecycle only

---

## Claude's Discretion

- Content structure within each concept page
- Commitment level explanation format
- Token program comparison approach
- Connecting page language section format

## Deferred Ideas

None — discussion stayed within phase scope
