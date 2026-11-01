# ISSUE-027 — fonts: Next Pages reports success without a usable layout integration

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

Review reference: `E24`.
[S] The Google-font pipeline accepts Next Pages, searches only App Router layouts and treats missing layout as success.

## Reach-and-Impact

[S] Font-related styles can reference a variable whose Next font integration was never installed.
Only the supported Google provider is established here; no Next application was run.

## Evidence

- [S] `packages/registry/src/updaters/update-fonts.ts:146-160,199-208` includes Next Pages in this path.
- [S] Layout discovery at `:240-260` does not find a Pages Router integration point.

## Prior-Art

Existing-import and alias-binding problems have separate owners in `ISSUE-028` and `ISSUE-029`.
Upstream searches remain pending.

## Proposed-Change

Return a specific missing-integration skip and provide manual guidance instead of reporting font installation success.

## Scope-and-Constraints

Preserve working App Router integration.
Implementing Pages Router support or creating an ineffective app layout is outside this correction.

## API-and-Compatibility

Report actual completion status without inventing a new font-provider contract.

## Verification

Planned smoke: process a Google font in a Pages-only project lacking `app/layout`.
Require a clear incomplete/skip result and retain the successful App Router control.

## Publication-Blockers

A versioned framework replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce Pages font success
Action: Add a Google-font item to an isolated Pages-only Next project.
Done-When: Generated styles, entry files and reported completion status show whether integration occurred.
