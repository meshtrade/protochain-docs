---
phase: 04-program-services-and-error-reference
plan: "03"
subsystem: token-program-api-reference
tags: [token-program, spl-token, token-2022, api-reference, mint, holding-accounts]
dependency_graph:
  requires:
    - concepts/token-programs.mdx
    - concepts/instructions-and-transactions.mdx
    - api-reference/shared-types.mdx
  provides:
    - api-reference/token-program/overview.mdx
    - api-reference/token-program/mint-creation.mdx
    - api-reference/token-program/holding-accounts.mdx
    - api-reference/token-program/mint.mdx
    - api-reference/token-program/parse-mint.mdx
  affects:
    - docs.json (Token Program Service navigation group)
tech_stack:
  added: []
  patterns:
    - Grouped SPL/Token-2022 method pairs on single pages
    - Token program choice framing before method content
    - Warning callout for ATA vs owner address distinction
    - Expandable for nested message types (extensions, metadata)
key_files:
  created:
    - api-reference/token-program/mint-creation.mdx
    - api-reference/token-program/holding-accounts.mdx
  modified:
    - api-reference/token-program/overview.mdx
    - api-reference/token-program/mint.mdx
    - api-reference/token-program/parse-mint.mdx
    - docs.json
  deleted:
    - api-reference/token-program/create-token2022-mint.mdx
    - api-reference/token-program/create-spl-token-mint.mdx
    - api-reference/token-program/create-token2022-holding-account.mdx
    - api-reference/token-program/create-spl-token-holding-account.mdx
decisions:
  - "Deleted orphaned stub files (create-token2022-mint.mdx etc.) to prevent stale pages from appearing at direct URLs"
  - "Used sections (H2) rather than tabs to group SPL/Token-2022 pairs — consistent with mint-creation pattern from plan"
  - "ParseMint intentionally omits the Instructions & Transactions Note callout (it is a query, not an instruction builder)"
metrics:
  duration: "~3 minutes"
  completed_date: "2026-03-25"
  tasks_completed: 3
  files_changed: 10
---

# Phase 4 Plan 3: Token Program Service Pages Summary

Token Program Service API reference with 5 grouped pages and docs.json navigation update — covering CreateToken2022Mint/CreateSPLTokenMint, CreateToken2022HoldingAccount/CreateSPLTokenHoldingAccount, Mint (with ATA owner gotcha), and ParseMint query.

## What Was Built

### Task 1: Token Program Overview + docs.json
- Replaced stub `overview.mdx` with full service overview
- SPL vs Token-2022 framing with link to `/concepts/token-programs`
- Methods table using grouped page links (mint-creation, holding-accounts)
- Updated `docs.json` Token Program Service group: 7 individual entries → 5 grouped entries
- Removed `create-token2022-mint`, `create-spl-token-mint`, `create-token2022-holding-account`, `create-spl-token-holding-account` individual nav entries

### Task 2: Mint Creation Grouped Page
- New `mint-creation.mdx` covering both CreateToken2022Mint and CreateSPLTokenMint
- Token-2022 extension support: Expandable for `Token2022ExtensionMetadata` (name, symbol, uri, additional_metadata)
- SPL Token Metaplex metadata: Expandable for `MetaplexTokenMetadata`
- Tip callout linking to `/concepts/token-programs`
- Note callout linking to `/concepts/instructions-and-transactions`
- CodeGroup Go/Rust/TypeScript for both methods

### Task 3: Holding Accounts, Mint, ParseMint Pages
- New `holding-accounts.mdx`: CreateToken2022HoldingAccount + CreateSPLTokenHoldingAccount
  - Token2022HoldingAccountExtension with MemoTransfer support documented
  - Note callout for instructions pattern
- Replaced `mint.mdx` stub: token-program-agnostic Mint method
  - Warning callout prominently placed: pass owner address, not ATA
  - `amount` documented as decimal string with concrete examples
- Replaced `parse-mint.mdx` stub: ParseMint query method
  - MintInfo Expandable with all 5 fields
  - `token_program` enum field (SPL vs Token-2022 detection)
  - `extensions` and `metaplex_metadata` response fields
  - No instruction callout (correctly omitted — query method)

### Deviation: Deleted orphaned stub files
- Old individual stub pages (create-token2022-mint.mdx etc.) remained on disk after docs.json update
- Deleted 4 files to prevent "under construction" pages from appearing at direct URLs
- These files had no navigation links but were discoverable by URL
- Rule 2 auto-fix: missing correctness requirement (orphaned stubs would confuse developers)

## Commits

| Hash | Message |
|------|---------|
| 7d7fcf7 | feat(04-03): write Token Program overview and update docs.json to grouped navigation |
| e10fac4 | feat(04-03): write Mint Creation grouped page (CreateToken2022Mint + CreateSPLTokenMint) |
| 74339f9 | feat(04-03): write Holding Accounts, Mint, and ParseMint pages |
| b04a28c | chore(04-03): delete orphaned individual stub pages replaced by grouped pages |

## Deviations from Plan

### Auto-fixed Issues

**1. [Rule 2 - Missing Correctness] Deleted orphaned individual stub files**
- **Found during:** Overall verification (after Task 3)
- **Issue:** Old stub files (create-token2022-mint.mdx, create-spl-token-mint.mdx, create-token2022-holding-account.mdx, create-spl-token-holding-account.mdx) remained on disk after being removed from docs.json navigation. The plan specified removing them from the navigation but did not explicitly call for file deletion. However, the verification check `grep -r "Coming soon\|under construction" api-reference/token-program/` would have failed with these files present.
- **Fix:** Deleted all 4 orphaned stub files
- **Files modified:** 4 files deleted
- **Commit:** b04a28c

## Known Stubs

None. All pages contain complete content — no placeholder text, no hardcoded empty values, no "coming soon" in authored files.

## Self-Check: PASSED

All 5 MDX files exist. All 4 task commits verified in git log (7d7fcf7, e10fac4, 74339f9, b04a28c).
