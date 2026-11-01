# ISSUE-031 — tailwind: shorthand content prevents configuration discovery

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

Review reference: `E28`.
[S] The v3 config finder requires a nonshorthand property assignment named `content`.

## Reach-and-Impact

[S] Valid `export default { content, theme: {} }` configuration can remain unchanged while update reports success.
Requested theme or plugin additions are absent; v4 skips this updater independently.
No consumer config update was executed.

## Evidence

- [S] `packages/registry/src/updaters/update-tailwind-config.ts:200-242,255-272` returns unchanged text after no match.
- [S] `packages/registry/src/codemod/object-literals.ts:108-114` intentionally distinguishes shorthand properties.

## Prior-Art

Missing preview representation of valid config changes is separate in `ISSUE-054`.
Upstream searches remain pending.

## Proposed-Change

Extend only the config finder to accept a noncomputed shorthand ObjectProperty named `content`.

## Scope-and-Constraints

Preserve shared codemod predicates and existing shorthand variable references.
Do not broaden this into arbitrary dynamic-config rewriting.

## API-and-Compatibility

Equivalent valid v3 configuration syntax should receive the same requested extension.

## Verification

Planned smoke: apply a theme/plugin addition to shorthand and expanded-content forms.
Both must receive the addition without inlining or altering the content variable.

## Publication-Blockers

A versioned config replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce shorthand config omission
Action: Apply a real v3 registry extension to a configuration with shorthand content.
Done-When: Original config, requested addition, result and reported status are recorded.
