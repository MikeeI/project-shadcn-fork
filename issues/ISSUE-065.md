# ISSUE-065 — resolver: cache bypass is lost in the Base-Color helper

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

Review reference: `E64`.
[S] The index/theme Base-Color path calls a helper without forwarding the explicit no-cache option.

## Reach-and-Impact

[S] Repeated Resolve calls with `useCache: false` can retain an old, unoverridden Base-Color variable.
This requires the index/theme and configured Base-Color path, not every component fetch.
No live freshness replay was executed.

## Evidence

- [S] `packages/registry/src/registry/resolver.ts:285-298,550-600` separates item options from the helper call.
- [S] `packages/registry/src/registry/api.ts:290-293` uses the default cached fetch for Base Color.

## Prior-Art

Header-key mismatch is a separate request-context defect in `ISSUE-066`.
Upstream searches remain pending.

## Proposed-Change

Forward the cache option through the theme and Base-Color helper to the actual fetch.

## Scope-and-Constraints

Preserve default caching and current variable merge priority.
Do not turn this bounded finding into an unproven claim about every index helper.

## API-and-Compatibility

An explicit freshness option must apply to the Base-Color data contributing to the resolved result.

## Verification

Planned smoke: change one Base-Color variable between two same-process Resolve calls with caching disabled.
Choose a variable not overwritten by the item; the second result must be fresh.

## Publication-Blockers

A versioned public Resolve replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce stale Base Color
Action: Resolve the index/theme path twice with useCache false while changing a contributing Base-Color response.
Done-When: Response versions, actual fetches and final unoverridden variable values are recorded.
