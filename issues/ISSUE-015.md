# ISSUE-015 — registry: workspace comparison conflates different external modules

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

Review reference: `E11`.
[S] Import-insensitive workspace comparison reduces nonrelative module specifiers to their final segment.

## Reach-and-Impact

[S] A required external-module change can be skipped even when overwrite is enabled.
The review observed equality for `alpha/utils` and `beta/utils` in the real helper, not a workspace install.

## Evidence

- [S] `packages/registry/src/compare.ts:21-52` applies this normalization to external imports.
- [S] `packages/registry/src/updaters/update-files.ts:200-221` compares before the overwrite decision.

## Prior-Art

Wrong module selection during rewriting is independently tracked as `ISSUE-016`.
Upstream searches remain pending.

## Proposed-Change

Normalize only aliases proven to resolve to the same workspace target and preserve external module identity.

## Scope-and-Constraints

Keep equivalence for genuinely interchangeable workspace aliases.
Do not broaden the import regex as a substitute for module identity.

## API-and-Compatibility

A real package change must no longer be reported as unchanged.

## Verification

Planned smoke: compare `alpha/utils` with `beta/utils`, then verify equivalent local aliases still compare equal.
Exercise the workspace write path with overwrite enabled.

## Publication-Blockers

A versioned workspace replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce external import skip
Action: Install an otherwise identical file that changes only the external package import.
Done-When: Comparison output and resulting installed import establish the skip or correct update.
