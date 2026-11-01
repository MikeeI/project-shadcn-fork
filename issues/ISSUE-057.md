# ISSUE-057 — mcp: cwd option is not forwarded to tool instances

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: API
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E56`.
[S] MCP startup uses `--cwd` for Env loading but tool configuration and runner selection use process cwd.

## Reach-and-Impact

[S] Starting in App A for App B can return A's registries and package runner despite B's requested context.
No MCP server was started during review.

## Evidence

- [S] `packages/shadcn/src/commands/mcp.ts:93-105` does not bind cwd to the server factory.
- [S] `packages/shadcn/src/mcp/index.ts:156,202,260,317,350` and `mcp/utils.ts:15-21` use process cwd.

## Prior-Art

Style-field loss is independently tracked as `ISSUE-058`.
Upstream prior-art search remains pending.

## Proposed-Change

Pass absolute cwd per server instance through all Registry and runner operations and initialize public exports correctly.

## Scope-and-Constraints

Do not use global `process.chdir`, which would couple independent server instances.
Do not invent a requirement to add `--cwd` to every suggested consumer shell command.

## API-and-Compatibility

Preserve default public server behavior while making an explicitly selected project authoritative.

## Verification

Planned smoke: start from A with `--cwd B`, using distinct registry and package-manager data.
Check B's tools plus the ordinary default-export path.

## Publication-Blockers

A versioned MCP protocol replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce MCP cwd loss
Action: Start MCP from one project while explicitly selecting another project directory.
Done-When: Invocation, loaded registries and emitted package runner establish the effective project.
