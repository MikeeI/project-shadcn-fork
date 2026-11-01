# ISSUE-060 — mcp: example search hides partial registry failures

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

Review reference: `E59`.
[S] Implicit example searches tolerate failures but discard `results.errors` in both empty and populated answers.

## Reach-and-Impact

[S] Incomplete coverage is presented as a complete example search.
Explicit registry selection already throws, and later item loading may fail separately.
No protocol replay was executed.

## Evidence

- [S] `packages/shadcn/src/mcp/index.ts:350-385` does not use returned example-search errors.
- [S] Neighboring list/search handlers at `:220-233,281-297` already format skipped registries.

## Prior-Art

Complete failure status is independently tracked as `ISSUE-012`.
Upstream searches remain pending.

## Proposed-Change

Append the existing skipped-registry description to both empty and populated partial example responses.

## Scope-and-Constraints

Keep explicit-selection failure behavior and successful partial results.
Handle total failure through the separate existing error-result contract.

## API-and-Compatibility

A partial result must identify its missing coverage rather than imply a reliable negative search.

## Verification

Planned smoke: one implicit registry succeeds and another fails, with and without matching examples.
Both answers must identify the skipped registry and its reason.

## Publication-Blockers

A versioned partial-failure protocol replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce hidden example failure
Action: Query examples with one successful and one failing implicitly selected registry.
Done-When: Target outcomes and the exact answer reveal whether incomplete coverage is disclosed.
