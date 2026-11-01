# ISSUE-008 — build: an item named registry collides with catalog output

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: Build
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E4`.
[S] Item output `${name}.json` and the catalog both target `registry.json` when an item is named `registry`.

## Reach-and-Impact

[S] A normal build loses that item output to the catalog.
With `--output .`, the item write can first overwrite the input catalog itself; this is the High-impact case.
No build was executed during the review.

## Evidence

- [S] `packages/shadcn/src/commands/build.ts:60-95` writes items before the catalog copy.
- [S] `packages/shadcn/src/preflights/preflight-build.ts:14-26` does not reject the path collision.

## Prior-Art

Local duplicate-name finding `ISSUE-044` has a different cause; one reserved name suffices here.
Upstream thread and release searches remain pending.

## Proposed-Change

Reject collisions between planned item outputs, the reserved catalog path and the input before any write.

## Scope-and-Constraints

Preserve ordinary output names and separate output directories.
Do not globally change dynamic item lookup semantics merely to fix static build paths.

## API-and-Compatibility

A previously accepted colliding item is rejected rather than silently losing source or output.

## Verification

Planned smoke: build an item named `registry` with default output and with `--output .`.
Both must fail before an item write; original source bytes must remain unchanged.

## Publication-Blockers

A versioned build reproduction and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce registry output collision
Action: Build the colliding item in an isolated registry with both output locations.
Done-When: Commands, CLI version and before/after source and output contents are recorded.
