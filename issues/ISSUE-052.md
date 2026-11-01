# ISSUE-052 — preview: workspace target configuration is not resolved

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

Review reference: `E51`.
[S] Preview uses the invoking configuration while installation selects target-specific workspace configurations.

## Reach-and-Impact

[S] Different App/UI CSS paths or transform options can yield preview changes for the wrong package.
An absolute shared alias alone is not proof of divergence; no workspace replay was executed.

## Evidence

- [S] `packages/shadcn/src/utils/dry-run.ts:95-126,144-175,253-268` plans from the starting configuration.
- [S] `packages/registry/src/add-components.ts:166-178,225-285` selects and groups actual workspace targets.

## Prior-Art

Individual option loss is tracked as `ISSUE-049`; CSS options are tracked as `ISSUE-051`.
Upstream searches remain pending.

## Proposed-Change

Apply the same target-specific workspace selection before preview path and transformation planning.

## Scope-and-Constraints

Keep App-specific fonts/files and UI-owned CSS/files in their proper packages.
Do not replace all destinations with the UI root.

## API-and-Compatibility

Workspace preview should agree with installation without changing existing routing policy.

## Verification

Planned smoke: use deliberately different App/UI CSS targets and transform options.
Compare preview owner, path and content with actual installer target selection.

## Publication-Blockers

A versioned workspace parity replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce workspace preview target
Action: Preview an item in a monorepo whose App and UI configurations intentionally differ.
Done-When: Selected configs and preview versus installer destinations are captured.
