---
phase: 05-workflow-guides
plan: 02
subsystem: guides
tags: [token, spl-token, token-2022, workflow-guide, mint]
dependency_graph:
  requires:
    - api-reference/token-program/mint-creation.mdx
    - api-reference/token-program/holding-accounts.mdx
    - api-reference/token-program/mint.mdx
    - concepts/token-programs.mdx
  provides:
    - guides/create-token.mdx
  affects: []
tech_stack:
  added: []
  patterns:
    - Narrative prose with inline CodeGroup blocks (D-02)
    - Token program choice table before any code (D-04)
    - Go, Rust, TypeScript tab order in every CodeGroup (D-07)
key_files:
  created: []
  modified:
    - guides/create-token.mdx
decisions:
  - "Token program choice table placed before any code — developers must make this call before writing any API code"
  - "Token-2022 path shown first throughout (more feature-rich, recommended for new tokens needing extensions)"
  - "Mint tokens section uses minimal CodeGroup showing only the Mint call, then prose referencing the compile/sign/submit pattern above rather than repeating it"
metrics:
  duration: 85s
  completed: "2026-03-25"
  tasks_completed: 1
  files_modified: 1
---

# Phase 5 Plan 02: Create a Token Workflow Guide Summary

**One-liner:** End-to-end Create a Token guide covering SPL Token and Token-2022 mint creation, holding account setup, and token minting with three-language code examples.

## What Was Built

`guides/create-token.mdx` — fully written, replacing the "under construction" stub. The guide walks developers through the complete token creation workflow:

1. **Choose your token program** — comparison table (SPL Token vs Token-2022) before any code, with a clear recommendation rule
2. **Prerequisites** — payer account, mint keypair, Protochain connection
3. **Create the mint** — separate H3 subsections for Token-2022 and SPL Token, each with a 3-language CodeGroup
4. **Create a holding account** — separate H3 subsections for Token-2022 and SPL Token, each with a 3-language CodeGroup; Note callout on `owner_pub_key` vs ATA
5. **Compile and submit the mint transaction** — assembles all instructions, shows CompileTransaction → SignTransaction (both keypairs) → SubmitTransaction; Note callout on mint keypair as signer requirement
6. **Mint tokens** — Mint call CodeGroup; Note callout on `destination_owner_pub_key` vs ATA; prose reference back to compile/sign/submit pattern
7. **Next steps** — links to Transfer SOL guide, Token Program Service overview, Monitor Transactions guide

## Decisions Made

- Token program choice table placed before any code, per D-04: the SPL vs Token-2022 decision is permanent and must be made before any API calls
- Token-2022 path shown first throughout — it is the more feature-rich option and the recommended default for new tokens with any extension requirements
- Mint tokens section uses a minimal CodeGroup (just the Mint call), with prose referencing the compile/sign/submit pattern above rather than duplicating the full pattern — keeps the guide concise per D-08

## Deviations from Plan

None — plan executed exactly as written.

## Known Stubs

None — all code examples use realistic placeholder addresses and demonstrate real API field usage. No hardcoded empty values flow to UI rendering.

## Self-Check: PASSED

- `guides/create-token.mdx` exists with full content: FOUND
- 6 CodeGroup blocks present: FOUND (grep -c `^<CodeGroup>` = 6)
- No stub text ("under construction"): CLEAN
- Both CreateToken2022Mint and CreateSPLTokenMint covered: FOUND
- Token program choice section before any code: FOUND
- Links to /api-reference/token-program/mint-creation, /holding-accounts, /mint, /concepts/token-programs: FOUND
- Go, Rust, TypeScript tab order in every CodeGroup: FOUND
- No Steps component: CLEAN
- Commit 4ccad93 exists: VERIFIED
