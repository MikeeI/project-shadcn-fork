# ISSUE-021 — init: restore removes bak text from a parent directory

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E17`, revised to High for failed configuration recovery.
[S] Removing the first `.bak` occurrence from a full backup path corrupts a parent-directory name.

## Reach-and-Impact

[S] A failed reinitialization under a `.bak`-bearing parent can leave the original config unrestored.
Its backup may remain recoverable manually; no complete-project atomicity is claimed.
The failure path was not executed.

## Evidence

- [S] `packages/shadcn/src/commands/init.ts:171-180` derives the restore target with unrestricted replacement.
- [S] `packages/shadcn/src/utils/file-helper.ts:29-38` appends the backup suffix to that already wrong target.

## Prior-Art

The review rejected the separate claim that any preexisting backup slot must be preserved.
Upstream searches remain pending for this concrete path defect.

## Proposed-Change

Store the original config path and use it directly as the restore target.

## Scope-and-Constraints

Preserve current backup-slot lifecycle and successful initialization.
Do not introduce a command-wide rollback mechanism.

## API-and-Compatibility

Error recovery must return the saved configuration to the exact original path.

## Verification

Planned smoke: fail reinitialization after backup under `/work/app.bak/project`.
Original configuration bytes must return to their original path.

## Publication-Blockers

A versioned recovery replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce bak parent recovery
Action: Trigger the existing restore path from a throwaway project with `.bak` in its parent name.
Done-When: Original path, backup path and postfailure bytes demonstrate the restore destination.
