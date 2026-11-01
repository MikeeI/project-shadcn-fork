# ISSUE-020 — registry: empty-string file content is silently omitted

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

Review reference: `E16`.
[S] Truthiness checks treat accepted `content: ""` as missing in installation, planning and preview.

## Reach-and-Impact

[S] Valid empty marker or configuration files are neither created nor represented in the result.
No actual file-installation replay was executed.

## Evidence

- [S] `packages/registry/src/registry/schema.ts:119-132` permits an empty content string.
- [S] `packages/registry/src/updaters/update-files.ts:109-113,430-451` skips empty content in execution and planning.
- [S] `packages/shadcn/src/utils/dry-run.ts:161-165` repeats the same exclusion in preview.

## Prior-Art

Custom first-file assignment is a separate concern in `ISSUE-014`.
Upstream searches remain pending.

## Proposed-Change

Distinguish absent content from an empty string consistently across execution and plans.

## Scope-and-Constraints

Keep original file identity consistent through filtering.
Retain Env merge policy; empty input must not erase existing Env assignments.

## API-and-Compatibility

Accepted empty content must create a zero-byte file when the target is new.

## Verification

Planned smoke: install an empty universal marker and check a zero-byte file plus created and preview entries.
Also check ordinary overwrite refusal and Env preservation.

## Publication-Blockers

A versioned public installation replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce empty file omission
Action: Install one universal file with empty content and an absent explicit target.
Done-When: The exact payload, filesystem result and CLI/API file-status output are recorded.
