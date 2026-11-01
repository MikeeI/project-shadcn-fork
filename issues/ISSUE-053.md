# ISSUE-053 — preview: Env files ignore alternative targets and append-only merge

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

Review reference: `E52`.
[S] Preview treats a Registry Env file as raw replacement while installation may select another file and append new keys.

## Reach-and-Impact

[S] Incoming `.env.local` can be previewed as creation while installation changes existing `.env` and retains old values.
This concerns files, not the separate `envVars` requirements list; no CLI parity replay was executed.

## Evidence

- [S] `packages/shadcn/src/utils/dry-run.ts:161-175,187-247` uses the original path and raw input.
- [S] `packages/registry/src/updaters/update-files.ts:139-144,266-280` applies alternative selection and merge.

## Prior-Art

The actual merge's quote defect is owned by `ISSUE-040`.
Upstream searches remain pending.

## Proposed-Change

Use identical Env-variant selection and new-key merge decisions for preview action and content.

## Scope-and-Constraints

Retain existing recognized values and existing target selection policy.
Do not turn the requirements-only Env list into a separate unproven mutation claim.

## API-and-Compatibility

Preview must describe the actual Env file and retained values, not a hypothetical replacement.

## Verification

Planned smoke: incoming `.env.local` contains an old and new key while only `.env` exists.
Preview must match the actual target, preserved value and appended key.

## Publication-Blockers

A versioned CLI parity replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce Env file preview
Action: Preview and plan one Env Registry file against an existing alternative Env target.
Done-When: Original file, proposed action and actual merge destination/content are captured.
