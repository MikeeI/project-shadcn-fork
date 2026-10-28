# ISSUE-006 — registry: cssVars values are written into the stylesheet unescaped

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-10-28
Updated: 2026-10-28
Source: `upstream/main@96da9cc7`

## Root-Cause

Root-Cause [S]: `cssVars` values are accepted as arbitrary strings and handed to `postcss.decl` as a declaration value without escaping, so a value containing `;` or `}` becomes additional declarations or rules in the project stylesheet that the application then ships to every visitor.

## Reach-and-Impact

Reach [S]: Any install whose resolved tree carries `cssVars` and a project with a configured stylesheet path; the values land in the same file that later compiles into the delivered CSS.
Impact [S]: Registry-controlled CSS in the global stylesheet of the deployed app, enabling UI redress and resource loads through `url()`, and remote stylesheet loading through `@import` where the at-rule path is reachable. Existing declarations are protected by the overwrite guard, so the injection adds rules rather than replacing protected tokens. Unmeasured boundary: no browser run confirmed which injected rules apply; the serialization defect is established from code.

## Evidence

- [S] `packages/registry/src/registry/schema.ts:147-151` — `registryItemCssVarsSchema` records `z.string()` values for `theme`, `light` and `dark`.
- [S] `packages/registry/src/updaters/update-css-vars.ts:467-469` — `postcss.decl({ prop, value: propValue })` passes the string through unchanged.
- [S] `packages/registry/src/updaters/update-css-vars.ts:273,305,361,436` — every other declaration path also builds nodes from raw registry strings.
- [S] `packages/registry/src/updaters/update-css-vars.ts:350-385` — the v4 path only refuses values for keys that already exist when `overwriteCssVars` is unset, so a new key is always written.
- [S] `packages/registry/src/updaters/update-css.ts:486-499,541-553` — declarations and rules are emitted to the stylesheet as text.
- [S] `packages/registry/src/updaters/update-css-vars.ts:766,784` — `isLocalHSLValue` and `isColorValue` exist but are applied to existing project values at `:357` and `:455`, not to registry input.
- [S] `packages/registry/src/add-components.ts:134-140` — the stylesheet update runs after files are written, so the injected rules are part of the same install.

## Prior-Art

Coverage: issues(open+closed), PRs(open+closed+merged), releases, GitHub advisory REST for npm `shadcn` and `@shadcn/registry`; checked=2026-10-28.
Gaps: no search of upstream issues for the `cssVars` symbol beyond title matching; the advisory query returned an empty array.

- `https://github.com/shadcn-ui/ui/pull/9940` — Related; proposes scanning downloaded files, which would not cover value-level escaping.
- `https://api.github.com/advisories?ecosystem=npm&affects=shadcn` — Distinct; returned an empty array.

Contribution fit: New issue — the serialization gap has no owner and the fix is local to one updater.

## Proposed-Change

Validate `cssVars` values against the character set a custom-property value may contain, rejecting `;`, `}` and newlines, or escape them before building the declaration. Reuse the existing color predicates where a value must be a color.

## Scope-and-Constraints

- Preserve: every value shape the registry docs show, including `hsl()` tuples, `var(--token)` references, and animation shorthands.
- Exclude: the `css` at-rule path, which is recorded as ISSUE-002.
- Cost: published registry items with unusual values start failing, so the accepted grammar needs a documented boundary.

## Verification

- `pnpm exec vitest run packages/registry/src/updaters/update-css-vars.test.ts` → a value containing a semicolon is rejected instead of producing two declarations, and the documented theme values still transform.

## Publication-Blockers

- No reproduction against the shipped CLI showing the resulting stylesheet.
- Whether maintainers treat `cssVars` as trusted author input is unresolved, which decides between rejection and escaping.

## Next-Action

Summary: Confirm cssVars declaration injection
Action: Run a local install whose item carries a `cssVars` value with a semicolon and inspect the written stylesheet.
Done-When: The written stylesheet shows either the injected declaration or the rejection, captured with the exact command and file content.
