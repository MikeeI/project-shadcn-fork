# ISSUE-032 — scaffold: React Router monorepo retains the template font

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

Review reference: `E29`, limited to the separately selected font-variable payload.
[S] The font step preserves an existing scaffold font token despite an explicitly different selected preset font.

## Reach-and-Impact

[S] React Router monorepo can import the selected font yet continue rendering with its existing Outfit token.
The conflict needs an existing token and a different selection; no scaffold/browser replay was executed.

## Evidence

- [S] `packages/shadcn/src/templates/monorepo.ts:95-134` writes the selected font with overwrite disabled.
- [S] `templates/react-router-monorepo/packages/ui/src/styles/globals.css:119` already defines Outfit.

## Prior-Art

`ISSUE-024` ignores a false policy; here a false policy is followed in the wrong semantic step.
Upstream searches remain pending.

## Proposed-Change

Overwrite only the explicitly selected font variables in the separate font step.

## Scope-and-Constraints

Preserve unrelated preset colors, existing CSS choices and other font definitions.
Do not globally enable overwriting for all preset tokens.

## API-and-Compatibility

A new scaffold's requested font must override its built-in template font default.

## Verification

Planned smoke: scaffold with a non-Outfit preset font.
Check the imported font, effective token and unchanged unrelated CSS choices.

## Publication-Blockers

A versioned scaffold/render replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce scaffold font mismatch
Action: Create an isolated React Router monorepo using a font other than Outfit.
Done-When: Preset input, font import, resulting CSS token and computed font are recorded.
