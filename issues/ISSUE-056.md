# ISSUE-056 — preview: missing required CSS is incorrectly planned as creation

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

Review reference: `E55`.
[S] Preview substitutes empty text for missing CSS, while the actual updater requires reading the existing file.

## Reach-and-Impact

[S] A promised CSS create can fail at installation with `ENOENT`.
The example excludes items whose earlier file-install step really creates that CSS target.
No installation replay was executed.

## Evidence

- [S] `packages/shadcn/src/utils/dry-run.ts:259-272,297-302` plans missing CSS from empty content.
- [S] `packages/registry/src/updaters/update-css.ts:45-57` has no corresponding create fallback.

## Prior-Art

Wrong workspace CSS selection is distinct in `ISSUE-052`.
Upstream searches remain pending.

## Proposed-Change

Report the same missing required CSS input in preview rather than inventing a create operation.

## Scope-and-Constraints

Preserve installation's current prerequisite contract.
Actually supporting CSS creation would be a separate behavior change.

## API-and-Compatibility

A successful preview must not promise an operation the selected updater cannot execute.

## Verification

Planned smoke: use a configured missing CSS path and an item that cannot create it in an earlier step.
Preview and updater must identify the same missing input.

## Publication-Blockers

A versioned CLI parity replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce missing CSS promise
Action: Preview a CSS-only item against a configured stylesheet path that does not exist.
Done-When: Preview action and actual updater read failure are captured without unrelated setup changes.
