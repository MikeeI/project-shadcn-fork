# ISSUE-044 — registry: direct catalogs bypass duplicate item-name validation

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

Review reference: `E43`.
[S] The loader returns a no-Include catalog before its existing duplicate-name validation.

## Reach-and-Impact

[S] Build can overwrite an earlier item while direct lookup returns the first duplicate and the catalog keeps both.
A separate validate command can catch it, but build does not require that command first.
No build replay was executed.

## Evidence

- [S] `packages/registry/src/registry/loader.ts:65-111,588-607` only validates duplicates after Include processing.
- [S] `packages/shadcn/src/commands/build.ts:60-95` writes duplicate output paths; loader `:48-62` finds first.

## Prior-Art

Reserved catalog-name collision is separate in `ISSUE-008` and needs no duplicate items.
Upstream searches remain pending.

## Proposed-Change

Run the existing duplicate check before the no-Include return, before consumers select or write any item.

## Scope-and-Constraints

Preserve duplicate rejection for resolved Include catalogs.
Do not merge or silently rename colliding item identities.

## API-and-Compatibility

Invalid duplicate identities must fail consistently before lookup and build output diverge.

## Verification

Planned smoke: build two same-name items directly and through an Include catalog.
Both must reject before item output.

## Publication-Blockers

A versioned build replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce direct catalog duplicates
Action: Build a no-Include registry containing two differently populated items with one name.
Done-When: Loader result, selected item and output collision or rejection are captured.
