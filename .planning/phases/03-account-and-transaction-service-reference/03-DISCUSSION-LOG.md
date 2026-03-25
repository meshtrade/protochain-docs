# Phase 3: Account and Transaction Service Reference - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-03-25
**Phase:** 03-account-and-transaction-service-reference
**Areas discussed:** Method page structure, Streaming RPC treatment, Error documentation

---

## Method page structure

| Option | Description | Selected |
|--------|-------------|----------|
| Params + examples | Request fields, response fields, code examples | ✓ |
| Full reference | Description + fields + behavioral notes + related methods + examples | |
| You decide | Claude determines per method | |

**User's choice:** Params + examples

| Option | Description | Selected |
|--------|-------------|----------|
| Developer-friendly | 'Base58-encoded public key (string)' | ✓ |
| Proto-literal | 'string' with explanation in description | |
| You decide | Claude picks | |

**User's choice:** Developer-friendly

| Option | Description | Selected |
|--------|-------------|----------|
| Response fields only | ResponseField components, no JSON | ✓ |
| Fields + example JSON | ResponseField plus ResponseExample sidebar | |

**User's choice:** Response fields only

---

## Streaming RPC treatment

| Option | Description | Selected |
|--------|-------------|----------|
| Extended page | Same as unary but with stream lifecycle sections | |
| Dedicated guide-style | Narrative walkthrough with full stream consumption code | ✓ |
| You decide | Claude picks | |

**User's choice:** Dedicated guide-style

| Option | Description | Selected |
|--------|-------------|----------|
| On the method page | Self-contained reconnection docs | |
| Defer to guide | Method page = API contract, guide = production patterns | |
| Both, brief + deep | Brief note on method page linking to Phase 5 guide | ✓ |

**User's choice:** Both, brief + deep

---

## Error documentation

| Option | Description | Selected |
|--------|-------------|----------|
| Dedicated error page | Standalone page with every error code | ✓ |
| Inline on methods | Each method lists its errors | |
| Both | Full catalog + inline notes | |

**User's choice:** Dedicated error page

| Option | Description | Selected |
|--------|-------------|----------|
| Table with columns | Error code / Description / Retryable / Remediation | ✓ |
| Grouped by type | Permanent / Temporary / Indeterminate sections | |
| You decide | Claude picks | |

**User's choice:** Table with columns

---

## Claude's Discretion

- Method description wording and behavioral gotchas
- TransactionStatus presentation on MonitorTransaction
- Service overview page format (cards vs table vs list)

## Deferred Ideas

None
