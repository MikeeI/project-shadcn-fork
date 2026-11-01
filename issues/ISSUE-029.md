# ISSUE-029 — fonts: aliased imports produce an unbound generated call

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

Review reference: `E26`, revised to High for invalid generated application code.
[S] The font updater uses an imported export name where the existing import binds a different local identifier.

## Reach-and-Impact

[S] An alias such as `Inter as GoogleInter` can be followed by generated `Inter(...)` with no local binding.
Use an existing requested-variable declaration to reach this branch instead of the separate early skip.
No application build was executed.

## Evidence

- [S] `packages/registry/src/codemod/named-imports.ts:14-23` intentionally returns exported import names.
- [S] `packages/registry/src/updaters/update-fonts.ts:284-295,321-341` uses them as call bindings.

## Prior-Art

`ISSUE-028` concerns missing variable installation, not alias identity.
Upstream searches remain pending.

## Proposed-Change

Read the matching ImportSpecifier's local binding in the font updater and use it in generated calls.

## Scope-and-Constraints

Do not change the shared imported-name helper's intentional semantics.
Preserve existing local aliases and other font declarations.

## API-and-Compatibility

Generated code must remain bound and valid for the consumer, not merely parse successfully.

## Verification

Planned smoke: transform aliased Inter with an existing requested root-variable initializer.
Every emitted font call must reference a real local binding.

## Publication-Blockers

A versioned transformation/build replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce aliased font binding
Action: Transform a reachable aliased-font layout containing the requested variable.
Done-When: Original import, rewritten initializer and consumer diagnostics establish binding correctness.
