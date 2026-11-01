# ISSUE-070 — command menu: documentation guides are assumed installable Registry items

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: Medium
Finding-Category: UI
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E69`.
[S] Every documentation URL containing `/components/` is converted from its final slug into an installation command.

## Reach-and-Impact

[A] A guide without a matching item in the selected base yields an invalid install recommendation.
Current item availability per base still needs replay; not every documentation slug is necessarily wrong.

## Evidence

- [S] `apps/v4/components/command-menu.tsx:271-283` derives installability from the URL category alone.
- [S] Its payload builder at `:142-149` uses the slug as the item name.

## Bug-Reproduction

The 2026-10-30 review observed `add data-table` for the Data Table guide.
The checked endpoint returned 404: https://ui.shadcn.com/r/styles/base-nova/data-table.json
That observation is historical; the deployed commit and browser build were not captured.

## Prior-Art

Unsupported npm runner output is separately tracked as `ISSUE-069`.
Upstream searches remain pending.

## Proposed-Change

Derive installability from authoritative per-base Registry metadata and keep guide-only entries navigational.

## Scope-and-Constraints

Preserve navigation and real component installation offers.
Do not silently substitute a different component or hardcode a speculative universal slug rule.

## API-and-Compatibility

Documentation grouping is not itself an installable-item identity contract.

## Verification

Planned browser replay: compare the guide with an actual item under the same selected base.
Only the verified item should receive an installation offer.

## Publication-Blockers

Current per-base item availability, versioned browser replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce guide install recommendation
Action: Compare the Data Table menu action with current item metadata for the selected base.
Done-When: Browser/build, selected base, copied command and actual item availability are recorded.
