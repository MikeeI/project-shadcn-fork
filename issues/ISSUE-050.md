# ISSUE-050 — preview: bare diff and view overviews omit CSS changes

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

Review reference: `E49`.
[S] Unfocused diff/view overviews consider CSS in the empty check but render only ordinary files.

## Reach-and-Impact

[S] A CSS-only preview can show no CSS content, destination or diff in the requested detail overview.
Explicit CSS-path focus already works; no full CLI rendering was executed.

## Evidence

- [S] `packages/shadcn/src/utils/dry-run-formatter.ts:197-275` omits CSS in both overview loops.
- [S] The summary and focused CSS paths at `:66-78,143-190,302-342` already treat it as an inspectable change.

## Prior-Art

Incorrect CSS computation is independently tracked as `ISSUE-051`.
Upstream searches remain pending.

## Proposed-Change

Include CSS in both overviews under the existing display limit and continuation behavior.

## Scope-and-Constraints

Preserve focused CSS output and bounded output size.
Do not claim that every CLI mode currently hides CSS.

## API-and-Compatibility

The advertised pathless detail flags must represent actual CSS changes as well as registry files.

## Verification

Planned smoke: render CSS-only data with bare `--diff` and `--view`, then compare focused CSS output.
The target and requested detail must be visible in both overviews.

## Publication-Blockers

A versioned CLI rendering replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce missing CSS overview
Action: Preview a CSS-only item through both pathless detail flags.
Done-When: Exact CLI output exposes whether CSS is omitted despite a nonempty CSS change.
