# ISSUE-073 — registry: appended query parameters land after URL fragments

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E72`.
[S] Query construction concatenates parameters after the full URL rather than before its fragment.

## Reach-and-Impact

[S] A Registry URL ending in `#section` gains fragment text such as `?version=latest`, not a sent query value.
This is a schema-permitted edge case; no server request was executed.

## Evidence

- [S] `packages/registry/src/registry/builder.ts:104-123` uses string concatenation.
- [S] The configured namespace parameter flow reaches it through `:71-81`.

## Prior-Art

Header-key normalization has an independent cause in `ISSUE-066`.
Upstream searches remain pending.

## Proposed-Change

Insert parameters into the Query component before the fragment while preserving existing query data and order.

## Scope-and-Constraints

Use URL-native parsing where compatible with already supported input forms.
Do not discard fragments or existing query values as a workaround.

## API-and-Compatibility

Configured parameters must reach the Registry server as actual request query values.

## Verification

Planned smoke: combine an existing query, a fragment and required appended parameters against a controlled endpoint.
Inspect the server-received query rather than only the displayed URL.

## Publication-Blockers

A versioned HTTP replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce fragment query loss
Action: Fetch a configured fragment-bearing Registry URL with a required added query parameter.
Done-When: Built URL and server-received query establish whether the parameter was transmitted.
