# ISSUE-026 — css: prefixed light and dark keys generate double-prefixed references

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

Review reference: `E23`.
[S] New v4 theme aliases normalize a leading `--` in the property name but not in the referenced variable.

## Reach-and-Impact

[S] A valid light/dark key such as `--brand` can define one variable and reference `var(----brand)`.
This concerns newly generated mappings, not every existing theme entry; no compiled CSS run was executed.

## Evidence

- [S] `packages/registry/src/updaters/update-css-vars.ts:350-355` normalizes the root declaration name.
- [S] The new alias path at `:454-475` constructs its reference from the unnormalized key.

## Prior-Art

`ISSUE-006` owns value escaping; this defect is variable-name/reference inconsistency.
Upstream prior-art search remains pending.

## Proposed-Change

Use the same normalized variable name for the new declaration and its `var()` reference.

## Scope-and-Constraints

Preserve sidebar-specific mapping and intentionally existing theme values.
Do not rewrite arbitrary CSS references globally.

## API-and-Compatibility

Accepted prefixed and unprefixed spellings must refer to the same defined variable.

## Verification

Planned smoke: transform `light: {"--brand": "oklch(0.5 0.1 20)"}` without an existing mapping.
The new color alias must use `var(--brand)` and leave existing intentional mappings unchanged.

## Publication-Blockers

A versioned transform replay and upstream thread search remain pending.

## Next-Action

Summary: Reproduce prefixed variable reference
Action: Generate a missing v4 theme alias from a prefixed light variable.
Done-When: Root definition and generated alias are captured with their exact names and references.
