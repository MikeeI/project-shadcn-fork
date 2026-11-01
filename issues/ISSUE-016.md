# ISSUE-016 — registry: extension priority lets basename fallback beat exact module path

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

Review reference: `E12`.
[S] Exact module candidates and unrelated basename fallbacks share an extension-first sort.

## Reach-and-Impact

[S] An extensionless import can bind to a different module with another export set.
The helper smoke selected `components/widget.tsx` ahead of exact `lib/widget.ts`; no consumer build was run.

## Evidence

- [S] `packages/registry/src/updaters/update-files.ts:559-619` mixes and sorts both candidate classes.
- [S] The alias rewrite uses the selected module at `:525-551`.

## Prior-Art

Alias text corruption is separate in `ISSUE-017`.
Upstream thread and release searches remain pending.

## Proposed-Change

Resolve exact base or index candidates first and consult basename fallbacks only if none exist.

## Scope-and-Constraints

Preserve extension priority within the chosen candidate class.
The demonstrated collision requires an extensionless probable path.

## API-and-Compatibility

Existing exact module bindings must not change merely because another planned file has a preferred extension.

## Verification

Planned smoke: provide exact `lib/widget.ts` and unrelated `components/widget.tsx` simultaneously.
The exact module and its rewritten import must win.

## Publication-Blockers

A versioned public installation replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce exact module displacement
Action: Install a file whose extensionless alias has both exact and basename candidates.
Done-When: Candidate set, selected module and final import are captured.
