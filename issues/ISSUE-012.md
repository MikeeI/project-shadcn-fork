# ISSUE-012 — mcp: total registry failure is returned as successful empty search

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: API
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E8`.
[S] Three MCP tools use `textResult` when all implicitly selected registries fail.

## Reach-and-Impact

[S] Clients see successful empty results rather than a machine-readable execution failure.
Explicit registry selection already throws; partial failure and a successful empty page are different cases.
No MCP protocol replay was performed.

## Evidence

- [S] `packages/shadcn/src/mcp/index.ts:222-227,283-290,366-376` uses success results after tolerated failures.
- [S] The existing wrapper at `:87-112` already supports `isError: true`.
- [S] MCP execution-error guidance: https://modelcontextprotocol.io/specification/2025-11-25/server/tools#error-handling

## Prior-Art

Partial example-search transparency is independently tracked as `ISSUE-060`.
Upstream prior-art search remains pending.

## Proposed-Change

Use `errorResult` for complete failure of a nonempty target set across all three tools.

## Scope-and-Constraints

Preserve partial results, successful zero matches and pages beyond the last result.
Do not infer total failure solely from an empty items array.

## API-and-Compatibility

The corrected MCP result uses the existing error field rather than a new response schema.

## Verification

Planned smoke: compare total failure, partial success, successful zero matches and an empty offset page.
Only total failure must receive the execution-error status.

## Publication-Blockers

A versioned protocol replay and upstream thread search remain pending.

## Next-Action

Summary: Reproduce MCP total failure
Action: Invoke each affected tool with every implicit target unavailable.
Done-When: Target counts, per-registry failures and the exact MCP result status are captured.
