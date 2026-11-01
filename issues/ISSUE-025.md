# ISSUE-025 — css: unrelated custom variants suppress dark selector registration

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

Review reference: `E22`.
[S] The v4 updater accepts any `@custom-variant` as proof that the required `dark` variant exists.

## Reach-and-Impact

[S] A stylesheet containing only an unrelated variant misses the requested class-based dark selector.
Other Tailwind dark mechanisms are not thereby universally disabled; no browser rendering was executed.

## Evidence

- [S] `packages/registry/src/updaters/update-css-vars.ts:513-542` checks the at-rule name, not its variant name.
- [S] The v4 transform selects this plugin at `:91-95`.

## Prior-Art

No matching cause exists in the local ledger.
Upstream issues, PRs, discussions and releases remain unsearched for this finding.

## Proposed-Change

Check the variant name in parameters and insert `dark` only when that variant is absent.

## Scope-and-Constraints

Preserve unrelated variants and an existing custom dark definition.
Do not replace all custom selectors with a generated default.

## API-and-Compatibility

Only a missing requested variant is added; meaningful existing selector choices remain authoritative.

## Verification

Planned smoke: transform CSS containing only `hocus`, then CSS containing a custom `dark`.
The first gains dark; the second retains its original rule without duplication.

## Publication-Blockers

A versioned transform-to-browser replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce missing dark variant
Action: Transform v4 CSS with one unrelated custom variant through the actual updater.
Done-When: Input and output show the missing dark selector registration or correct addition.
