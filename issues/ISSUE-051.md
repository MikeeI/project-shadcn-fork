# ISSUE-051 — preview: CSS transformation uses different context and overwrite policy

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

Review reference: `E50`.
[S] CSS preview omits actual Tailwind version/context and the installer's item-dependent overwrite decision.

## Reach-and-Impact

[S] A v4 project can preview v3-shaped CSS or different retained values from its actual installation.
No end-to-end CSS parity replay was executed.

## Evidence

- [S] `packages/shadcn/src/utils/dry-run.ts:253-285` supplies only limited plugin options.
- [S] `packages/registry/src/add-components.ts:426-452` selects policy by resolved item type.
- [S] `packages/registry/src/updaters/update-css-vars.ts:77-112` defaults to the v3 plugin path.

## Prior-Art

Workspace routing and overview omission have separate causes in `ISSUE-052` and `ISSUE-050`.
Upstream searches remain pending.

## Proposed-Change

Use the same resolved version, Tailwind context and overwrite decisions for preview and installation.

## Scope-and-Constraints

Preserve ordinary-component variable retention and deliberate theme replacement.
Do not make every preview overwrite merely to simplify parity.

## API-and-Compatibility

A preview must describe the selected installation policy rather than a different default transformation.

## Verification

Planned smoke: compare focused v4 preview against actual pure transforms for a Theme and ordinary component.
Use existing conflicting variables to expose policy differences.

## Publication-Blockers

A versioned CLI/transform parity replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce CSS preview divergence
Action: Preview the Theme and component cases in the same v4 consumer with existing variables.
Done-When: Effective options, preview CSS and installer transform outputs are recorded.
