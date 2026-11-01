# ISSUE-006 — registry: cssVars values are written into the stylesheet unescaped

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-10-28
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Root-Cause [S]: `cssVars` values are accepted as arbitrary strings and passed to `postcss.decl` without encoding.
A value containing `;` or `}` can become additional declarations or rules in the consumer stylesheet.
The application can subsequently ship that stylesheet to visitors.

## Reach-and-Impact

Reach [S]: Installs carrying `cssVars` and a configured stylesheet write values into the later compiled stylesheet.
Impact [S]: Registry-chosen CSS reaches the application's global stylesheet.
It can alter UI, load resources via `url()` and load remote styles via `@import` where at-rules are reachable.
The v4 preservation guard protects existing tokens when overwrite is false, but does not sanitize new values.
Tailwind v3 does not honor that preservation option; the independent policy defect is tracked as `ISSUE-024`.
Unmeasured boundary: no browser run confirmed which injected rules apply; this is source-level serialization evidence.

## Evidence

- [S] `packages/registry/src/registry/schema.ts:147-151` — `registryItemCssVarsSchema` records `z.string()` values for `theme`, `light` and `dark`.
- [S] `packages/registry/src/updaters/update-css-vars.ts:467-469` — `postcss.decl({ prop, value: propValue })` passes the string through unchanged.
- [S] `packages/registry/src/updaters/update-css-vars.ts:273,305,361,436` — every other declaration path also builds nodes from raw registry strings.
- [S] `packages/registry/src/updaters/update-css-vars.ts:350-385` — the v4 path only refuses values for keys that already exist when `overwriteCssVars` is unset, so a new key is always written.
- [S] `packages/registry/src/updaters/update-css.ts:486-499,541-553` — declarations and rules are emitted to the stylesheet as text.
- [S] `packages/registry/src/updaters/update-css-vars.ts:766,784` — `isLocalHSLValue` and `isColorValue` exist but are applied to existing project values at `:357` and `:455`, not to registry input.
- [S] `packages/registry/src/add-components.ts:134-140` — the stylesheet update runs after files are written, so the injected rules are part of the same install.
- [S] Review `E21`, tracked as `ISSUE-024`, limits the earlier blanket claim that existing tokens are protected.

## Prior-Art

Coverage: issues(open+closed), PRs(open+closed+merged), releases, GitHub advisory REST for npm `shadcn` and `@shadcn/registry`; checked=2026-10-28.
Gaps: no search of upstream issues for the `cssVars` symbol beyond title matching; the advisory query returned an empty array.

- `https://github.com/shadcn-ui/ui/pull/9940` — Related; proposes scanning downloaded files, which would not cover value-level escaping.
- `https://api.github.com/advisories?ecosystem=npm&affects=shadcn` — Distinct; returned an empty array.

Contribution fit: A new issue is a candidate; the recorded limited search identified no owner for this local sink.

## Proposed-Change

Validate `cssVars` values against the supported grammar, or encode them before creating a declaration.
Rejection may include `;`, `}` and newlines; the accepted grammar remains a maintainer decision.
Reuse existing color predicates where the value must be a color.

## Scope-and-Constraints

- Preserve: every value shape the registry docs show, including `hsl()` tuples, `var(--token)` references, and animation shorthands.
- Exclude: the `css` at-rule path, which is recorded as ISSUE-002.
- Cost: published registry items with unusual values start failing, so the accepted grammar needs a documented boundary.

## API-and-Compatibility

Changing accepted value syntax requires an explicit compatibility decision before source implementation.

## Verification

- `pnpm exec vitest run packages/registry/src/updaters/update-css-vars.test.ts` → a value containing a semicolon is rejected instead of producing two declarations, and the documented theme values still transform.

## Publication-Blockers

- No reproduction against the shipped CLI showing the resulting stylesheet.
- Whether maintainers treat `cssVars` as trusted author input is unresolved, which decides between rejection and escaping.

## Next-Action

Summary: Confirm cssVars declaration injection
Action: Run a local install whose item carries a `cssVars` value with a semicolon and inspect the written stylesheet.
Done-When: The written stylesheet shows either the injected declaration or the rejection, captured with the exact command and file content.
