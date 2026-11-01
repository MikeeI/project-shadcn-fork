# ISSUE-007 — mcp: failed config reads can overwrite existing client settings

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E3`.
[S] MCP initialization converts every config read or parse failure into an empty object before writing the same file.

## Reach-and-Impact

[S] JSON-based MCP client initialization can replace unrelated settings when reading fails but writing succeeds.
No filesystem reproduction was executed; an unsuccessful write alone does not establish data loss.

## Evidence

- [S] `packages/shadcn/src/commands/mcp.ts:249-265` catches read and parse errors before merging and writing.
- [S] The Codex-specific branch is separate; the claim concerns the JSON-client path.

## Prior-Art

Local ledger comparison found no matching cause.
Upstream issues, PRs, discussions and releases have not been searched for this finding.

## Proposed-Change

Treat only `ENOENT` as a new configuration and propagate other read and all parse failures before writing.

## Scope-and-Constraints

Preserve valid deep merges and unrelated client settings.
Keep the separate Codex setup unchanged.

## API-and-Compatibility

Unreadable existing input becomes an explicit error instead of a destructive replacement.

## Verification

Planned smoke: exercise invalid JSON, read-denied/write-allowed input, a missing file and a valid unrelated server.
Failures must preserve original bytes; successful cases must retain existing settings.

## Publication-Blockers

A versioned public-boundary replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce MCP config replacement
Action: Replay failed existing-config reads in an isolated MCP client directory.
Done-When: Exact command, client version, original bytes and resulting error or replacement are captured.
