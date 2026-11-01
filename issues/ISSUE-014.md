# ISSUE-014 — workspace: custom first-file target is reused across groups

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E10`, revised from Medium to High.
[S] Each workspace group restarts the first-file index while receiving the same custom file target.

## Reach-and-Impact

[S] Distinct groups can overwrite the same absolute custom file when overwrite is enabled.
Without overwrite the later group may conflict or skip; relative paths need not collide.
No workspace installation was executed.

## Evidence

- [S] `packages/registry/src/add-components.ts:225-285` invokes separate grouped file updates.
- [S] `packages/registry/src/resolve-file-path.ts:19-43` gives a custom file target to index zero.

## Prior-Art

Preview option loss is distinct and tracked as `ISSUE-049`.
Upstream prior-art search remains pending.

## Proposed-Change

Determine the global first file before grouping and assign a custom file path only to that file.

## Scope-and-Constraints

Retain original file identity through planning and execution.
Custom directory paths must continue applying to every file.

## API-and-Compatibility

One installation-level file option must not silently become one overwrite target per group.

## Verification

Planned smoke: install two groups with distinct contents, an absolute custom file and overwrite enabled.
Both contents must survive; directory-target behavior must remain unchanged.

## Publication-Blockers

A versioned workspace replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce grouped target collision
Action: Install two workspace file groups into one absolute custom file target.
Done-When: Resolved destinations, operation order and both resulting contents are captured.
