# ISSUE-049 — preview: custom path is omitted from file and import planning

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

Review reference: `E48`.
[S] Add forwards the custom path to installation but not to preview options or planned import destinations.

## Reach-and-Impact

[S] Preview can show creation at a default location instead of an actual custom-target conflict or update.
The review observed different resolver targets, not a full preview-to-install replay.

## Evidence

- [S] `packages/shadcn/src/commands/add.ts:310-315,328-329` passes different options to the two paths.
- [S] `packages/shadcn/src/utils/dry-run.ts:75-79,156-175` lacks the custom path.

## Prior-Art

Workspace first-file reuse is a separate installer defect in `ISSUE-014`.
Upstream searches remain pending.

## Proposed-Change

Forward `path` through preview options to actual file resolution and planned import resolution.

## Scope-and-Constraints

Preserve first-file semantics for file targets and all-file semantics for directory targets.
Workspace target selection remains independently owned by `ISSUE-052`.

## API-and-Compatibility

Presentation flags must not silently discard an installation-routing option.

## Verification

Planned smoke: compare file and directory custom paths with distinct existing/default target states.
Preview destinations, actions and imports must match installation planning.

## Publication-Blockers

A versioned CLI parity replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce custom path preview
Action: Preview one item with a custom target whose existence differs from the default target.
Done-When: CLI arguments, preview path/action and actual planned destination are captured.
