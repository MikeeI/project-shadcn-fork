# ISSUE-058 — mcp: project style and Tailwind normalization context are lost

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

Review reference: `E57`, including Tailwind config needed by existing Legacy-style normalization.
[S] MCP loads registries only, so builtin item URLs fall back to a style unrelated to the project.

## Reach-and-Impact

[S] `radix-nova` can produce a new-york-v4 request; legacy `new-york` also needs Tailwind context for normalization.
The exact returned remote variant was not fetched during review.

## Evidence

- [S] `packages/shadcn/src/mcp/utils.ts:20-27` omits style context.
- [S] `packages/registry/src/registry/config.ts:6-17` reads both style and an empty Tailwind-config value.

## Prior-Art

Project-directory selection is a separate cause in `ISSUE-057`.
Upstream prior-art search remains pending.

## Proposed-Change

Permissively load `style` and `tailwind.config` alongside existing merged registries.

## Scope-and-Constraints

Preserve Registry-only, package-only and partial components configurations.
Requiring complete alias validation through full getConfig is not a safe replacement.

## API-and-Compatibility

Existing read-only MCP consumers must not acquire unrelated full-project configuration prerequisites.

## Verification

Planned smoke: compare MCP and CLI URLs for modern styles and legacy new-york with empty/nonempty Tailwind config.
Retain partial and package-only config controls.

## Publication-Blockers

A versioned URL/protocol replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce MCP style fallback
Action: Request the same builtin item from distinct styles through MCP and the CLI.
Done-When: Raw project fields, normalized style and emitted URLs demonstrate their agreement or divergence.
