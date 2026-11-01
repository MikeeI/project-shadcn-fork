# ISSUE-023 — init: existing Next apps receive new-project CSS cleanup

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

Review reference: `E20`, revised to High for removal of existing user styling.
[S] `runInit` treats every `next-app` as new even when its caller explicitly passes `isNewProject: false`.

## Reach-and-Impact

[S] Existing single-project CSS-variable updates can remove user background gradients as supposed scaffold residue.
The cleanup needs CSS variables and is not propagated identically in the workspace path.
No actual stylesheet update was executed.

## Evidence

- [S] `packages/shadcn/src/commands/apply.ts:234-246` passes false; `commands/init.ts:839-840` overrides it.
- [S] `packages/registry/src/updaters/update-css-vars.ts:189-227` removes matching background declarations.

## Prior-Art

Implicit starter installation has a separate predicate and effect in `ISSUE-009`.
Upstream searches remain pending.

## Proposed-Change

Use actual project-creation state for cleanup and remove framework identity from that decision.

## Scope-and-Constraints

Preserve known scaffold cleanup for newly created apps.
Do not broaden CSS deletion or treat a font-only apply as permission to remove unrelated styling.

## API-and-Compatibility

An explicit existing-project flag must remain authoritative through the install pipeline.

## Verification

Planned smoke: apply fonts or reinitialize an existing Next app with a custom body gradient.
The gradient must remain; a newly scaffolded app must still receive intended cleanup.

## Publication-Blockers

A versioned apply/init replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce existing Next cleanup
Action: Run the CSS-variable update path against a throwaway existing Next app with a custom gradient.
Done-When: Flags, framework detection and before/after stylesheet contents expose the cleanup decision.
