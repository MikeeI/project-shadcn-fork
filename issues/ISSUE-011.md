# ISSUE-011 — preview: formatting normalization hides semantic literal changes

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

Review reference: `E7`, revised from High to Medium.
[S] Diff normalization strips significant whitespace, quotes and semicolons even inside literals.

## Reach-and-Impact

[S] Focused and overview diffs can omit real text changes without changing the installer result.
The review observed helper equivalence for `"A  B"` versus `"A B"` and `"A;B"` versus `"AB"`, not a full CLI run.

## Evidence

- [S] `packages/shadcn/src/utils/dry-run-formatter.ts:747-779` performs the unsafe normalization.
- [S] The early, grouped and patch-input paths at `:483-515,620-649` consume that equivalence.

## Prior-Art

CSS overview omission is a separate rendering cause in `ISSUE-050`.
Upstream thread and release searches remain pending.

## Proposed-Change

Build diffs from original texts and remove suppression not justified by a syntax-aware equivalence proof.

## Scope-and-Constraints

Cover every normalization consumer, not only the first return.
Do not change actual file installation merely to match an incorrect preview.

## API-and-Compatibility

Meaningful changes become visible; a noisier formatting diff is preferable to hidden code changes.

## Verification

Planned smoke: render whitespace and semicolon changes inside strings plus a mixed hunk.
All meaningful changes must remain visible in focused and grouped output.

## Publication-Blockers

A versioned rendered CLI replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce hidden literal diff
Action: Preview the two literal changes through the actual CLI formatter.
Done-When: Exact inputs and output demonstrate whether meaningful hunks disappear.
