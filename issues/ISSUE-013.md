# ISSUE-013 — registry: declined files enter the final import rewrite

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

Review reference: `E9`.
[S] An explicit overwrite refusal enters `filesSkipped`, then the interactive final import pass may write it again.

## Reach-and-Impact

[S] A declined code file with a resolvable alias can change without consent and appear in conflicting status lists.
Noninteractive skipped files do not enter this second pass; no interactive CLI replay was executed.

## Evidence

- [S] `packages/registry/src/updaters/update-files.ts:225-250,297-302` reconnects skipped interactive files.
- [S] The final writer at `:393-424` does not retain the refusal reason.

## Prior-Art

Universal-file protection is a separate type-based boundary in `ISSUE-018`.
Upstream prior-art search remains pending.

## Proposed-Change

Track explicit refusals separately and exclude them from all later writes and updated-file status.

## Scope-and-Constraints

Do not exclude every skipped file: identical-content and other skip reasons have different policies.
Keep intended alias normalization for eligible files.

## API-and-Compatibility

A negative overwrite answer must preserve original bytes and remain a skip.

## Verification

Planned smoke: refuse an overwrite whose local alias would otherwise normalize.
Verify unchanged bytes, no updated status, and an unaffected eligible-skip control.

## Publication-Blockers

A versioned prompt replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce refused file rewrite
Action: Decline a real component overwrite in an isolated alias-configured consumer.
Done-When: Prompt answer, original bytes, final bytes and status lists are captured.
