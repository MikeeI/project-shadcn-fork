# ISSUE-038 — workspace: application envVars are written into the UI package

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

Review reference: `E35`.
[S] Workspace Env updates use the UI-selected main target instead of the invoking application configuration.

## Reach-and-Impact

[S] A standard Next app launched from its own root will not load the newly written UI-package Env file.
Custom loaders or direct UI-package workflows may differ; no Next process was executed.

## Evidence

- [S] `packages/registry/src/add-components.ts:166-168,213-223` routes Env to UI but treats App fonts separately.
- [S] `packages/registry/src/updaters/update-env-vars.ts:39-45` selects the passed configuration's cwd.
- [S] App-root Env loading: https://nextjs.org/docs/app/guides/environment-variables

## Prior-Art

Env serialization is owned by `ISSUE-004`; target routing is independent.
Upstream prior-art search remains pending.

## Proposed-Change

Route App-consumed Env values to the invoking application configuration rather than the UI main target.

## Scope-and-Constraints

Preserve UI routing for shared components, CSS and dependencies.
Do not relocate deliberately UI-owned values without establishing their actual consumer.

## API-and-Compatibility

The generated file must be in a location loaded by the supported application runtime.

## Verification

Planned smoke: add an App-required value from `apps/web` in a standard Next monorepo.
Confirm its App-root file and loaded value, not merely successful writing in the UI package.

## Publication-Blockers

A versioned framework replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce application env misrouting
Action: Add a registry item requiring an App Env value from the Next workspace app.
Done-When: Invoking cwd, selected config, written Env path and loaded App value are recorded.
