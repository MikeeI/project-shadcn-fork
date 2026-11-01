# ISSUE-066 — registry: URL normalization disconnects configured request headers

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E65`.
[S] Header registration uses the built URL while lookup uses its subsequently normalized equivalent.

## Reach-and-Impact

[S] A namespace URL containing explicit `:443` and headers without params can lose its configured variant header.
Some earlier-normalized API forms avoid this example; no server request was executed.

## Evidence

- [S] `packages/registry/src/registry/resolver.ts:63-74` registers an exact URL key.
- [S] `packages/registry/src/registry/fetcher.ts:37-38` normalizes before the exact-map lookup.
- [S] `packages/registry/src/registry/context.ts:34-48` stores and retrieves headers by exact string key.

## Prior-Art

Fragment/query assembly is separately tracked as `ISSUE-073`.
Upstream searches remain pending.

## Proposed-Change

Use the identical final fetch key in resolver/API registration and header lookup.

## Scope-and-Constraints

Preserve query parameters and existing v0 path handling.
Cover both active registration sites, not only one caller.

## API-and-Compatibility

Semantically equivalent URL normalization must not change the configured request headers.

## Verification

Planned smoke: request a controlled namespace through explicit standard port and required variant header.
Capture the received header in both affected API paths.

## Publication-Blockers

A versioned request replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce normalized header loss
Action: Fetch a namespaced item whose configured URL includes the explicit HTTPS standard port.
Done-When: Registered key, final fetch key and server-received headers are captured.
