# ISSUE-002 — registry: css at-rules load build-executed JavaScript

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

Root-Cause [S]: Registry item `css` keys are unvalidated at-rule text that `updateCss` writes into the consumer's stylesheet, and Tailwind resolves `@plugin` and `@config` as JavaScript modules at build time; the same root cause reaches Tailwind v3 consumers through `tailwind.config.plugins`, whose strings are spliced into the config as source.

## Reach-and-Impact

Reach [S]: Every install whose resolved tree carries a `css` object with an `@plugin` or `@config` key, on Tailwind v4 projects; Tailwind v3 projects through `tailwind.config.plugins`.
Impact [S]: Registry data decides which module the consumer's build loads, so the build process executes registry-chosen code with the build's environment and write access. Unmeasured boundary: module loading by Tailwind is documented upstream but was not reproduced locally, and no exploit artifact was built.

## Evidence

- [S] `packages/registry/src/registry/schema.ts:162` — `registryItemCssSchema` is `z.record(z.string(), cssValueSchema)` with no allowlist of at-rule names.
- [S] `packages/registry/src/updaters/update-css.ts:113-119` — any key starting with `@` becomes an at-rule.
- [S] `packages/registry/src/updaters/update-css.ts:153-181` — `@plugin` params are taken verbatim from the registry and emitted as a real at-rule.
- [S] `packages/registry/src/updaters/update-css.test.ts:336-342` — proves the emitted `@plugin "foo";` output.
- [S] `packages/registry/src/add-components.ts:118,134` — component files are written before the stylesheet, so the referenced module exists at build time.
- [S] `packages/registry/src/registry/schema.ts:137-145` — `tailwind.config.plugins` accepts arbitrary strings.
- [S] `packages/registry/src/updaters/update-tailwind-config.ts:343-377` — each plugin string is inserted into `plugins: [...]` as source text.
- [S] `packages/registry/src/updaters/update-tailwind-config.ts:76-79` — the v3 path returns early for Tailwind v4, so the two variants apply to different consumer setups.
- [S] `apps/v4/content/docs/registry/examples.mdx:831-885` — the registry docs document `@plugin` usage including a local file path `./custom-plugin.js`.
- [S] `https://tailwindcss.com/docs/functions-and-directives` — `@plugin` loads a JavaScript-based plugin and accepts a package name or a local path.
- [S] `apps/v4/content/docs/registry/namespace.mdx:422-427` — the security section claims registries are treated as data with no script execution, which the above control flow contradicts in effect.

## Prior-Art

Coverage: issues(open+closed), PRs(open+closed+merged), releases, GitHub advisory REST for npm `shadcn` and `@shadcn/registry`; checked=2026-10-28.
Gaps: the `label:security` search returned zero results, and closed threads were only sampled.

- `https://github.com/shadcn-ui/ui/pull/9940` — Related; open since 2026-03-10, proposing a `--security` scan of downloaded files. Adjacent mitigation, different mechanism, no owner for this path.
- `https://api.github.com/advisories?ecosystem=npm&affects=shadcn` — Distinct; returned an empty array.
- `https://github.com/shadcn-ui/ui/pull/11195` — Distinct; fixed package-manager flag injection in dependency strings, not config-driven build execution.

Contribution fit: New issue — no open work owns registry-controlled build-time module loading, and the shipped security doc claims the opposite.

## Proposed-Change

Reject at-rules that load code (`@plugin`, `@config`) in registry items installed from a remote registry, or require an explicit confirmation that names the module path. Keep inert at-rules such as `@keyframes`, `@layer`, `@import` and `@utility`. Apply the same rejection to `tailwind.config.plugins` strings that are not a bare package name.

## Scope-and-Constraints

- Preserve: legitimate `@plugin` usage for npm packages the item also declares, and every non-code at-rule the registry docs already support.
- Exclude: dependency resolution, postinstall behavior, and package-manager policy.
- Cost: the docs currently teach the rejected forms, so documentation and examples must change with the code.

## Verification

- `pnpm exec vitest run packages/registry/src/updaters/update-css.test.ts` → a remote item with `@plugin "./x.js"` is rejected, and `@plugin "some-npm-package"` plus inert at-rules still install.

## Publication-Blockers

- No local reproduction of Tailwind loading the installed module.
- Whether maintainers consider `@plugin` in registry items intended behavior is unresolved; the docs argue both ways.

## Next-Action

Summary: Verify Tailwind plugin execution locally
Action: Install a local registry item that ships `@plugin "./x.js"` plus the file into a throwaway Tailwind v4 project and run the build.
Done-When: The build executes the shipped module, captured with the exact commands and build output.
