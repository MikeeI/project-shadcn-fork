# ISSUE-024 — css: Tailwind v3 ignores the preserve-existing-variables option

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

Review reference: `E21`.
[S] The v3 CSS-variable plugin does not receive `overwriteCssVars` and always replaces matching declarations.

## Reach-and-Impact

[S] Ordinary component installation can reset customized v3 values despite a false overwrite option.
Theme, style and font operations can intentionally request replacement; no stylesheet replay was executed.

## Evidence

- [S] `packages/registry/src/updaters/update-css-vars.ts:85,271-284` omits the option in the v3 path.
- [S] `packages/registry/src/add-components.ts:426-451` selects preservation for ordinary components.

## Prior-Art

`ISSUE-006` concerns serialization, not whether an existing declaration should be replaced.
Upstream prior-art search remains pending.

## Proposed-Change

Pass the overwrite option into the v3 plugin and replace existing declarations only when it is true.

## Scope-and-Constraints

Continue adding missing variables and preserve explicitly requested theme replacement.
Do not globally disable CSS updates to avoid overwrites.

## API-and-Compatibility

The public option should have consistent preservation semantics across supported Tailwind versions.

## Verification

Planned smoke: transform the same v3 CSS with false and true; preserve or replace accordingly.
Both cases must still add a missing variable.

## Publication-Blockers

A versioned consumer replay and upstream thread search remain pending.

## Next-Action

Summary: Reproduce v3 variable overwrite
Action: Install an ordinary component into v3 CSS with a customized conflicting variable.
Done-When: Option value, input CSS and output CSS show whether preservation is honored.
