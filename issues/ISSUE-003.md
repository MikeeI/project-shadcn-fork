# ISSUE-003 — chart: ChartStyle interpolates config into a style element unencoded

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: Medium
Finding-Category: UI
Created: 2026-10-28
Updated: 2026-10-28
Source: `upstream/main@96da9cc7`

## Root-Cause

Root-Cause [S]: `ChartStyle` builds a stylesheet by template-string interpolation of `config` keys, `color` values and the `id` prop and injects it with `dangerouslySetInnerHTML`, with no encoding at any point between the public prop and the sink, so `}`, `</style>` and selector characters pass through unchanged.

## Reach-and-Impact

Reach [S]: Every consumer of the shipped chart component whose `ChartConfig` or `id` carries data the application does not author statically; the shipped examples and docs use constants, so default usage is unaffected.
Impact [S]: Document-wide CSS injection in the rendered page, because the emitted style element lives in the light DOM and its rules are not scoped to the chart. Impact [A]: stored script execution in the origin when the markup is server-rendered and `</style>` closes the raw-text element; this is derived from the parser and React SSR semantics and was not reproduced in a browser.

## Evidence

- [S] `apps/v4/registry/bases/base/ui/chart.tsx:83-114` — `ChartStyle` maps `Object.entries(config)` into `--color-${key}: ${color};` inside a single template string.
- [S] `apps/v4/registry/bases/base/ui/chart.tsx:93-94` — the string is injected through `dangerouslySetInnerHTML` on a `style` element.
- [S] `apps/v4/registry/bases/base/ui/chart.tsx:59,65,98` — `chartId` is derived from the optional `id` prop and used unquoted in the selector `[data-chart=${id}]`.
- [S] `apps/v4/registry/bases/radix/ui/chart.tsx:94`, `apps/v4/registry/bases/aria/ui/chart.tsx:94`, `apps/v4/registry/new-york-v4/ui/chart.tsx:94` — the same sink exists in all four shipped bases.
- [O] `grep -rn "dangerouslySetInnerHTML" apps/v4/registry templates packages/react` → exactly four hits, all the `ChartStyle` block; environment=Linux x64, checkout 7edc7f33, rechecked 2026-10-28.
- [S] `apps/v4/content/docs/components/base/chart.mdx:355-357` — the docs state the config is intentionally decoupled from chart data and works when data or color tokens live remotely, so remote-sourced config is documented usage.
- [O] `grep -rin "content-security-policy|csp|nonce" apps/v4/content/docs` → matches only in the `message-scroller` docs, so no CSP guidance covers this sink; environment=Linux x64, checkout 7edc7f33.
- [S] `templates/next-app/next.config.ts:1-5` — the scaffold ships an empty config, so no CSP or header baseline is installed with the components.

## Prior-Art

Coverage: issues(open+closed), PRs(open+closed+merged), releases, GitHub advisory REST for npm `shadcn` and `@shadcn/registry`; checked=2026-10-28.
Gaps: no full-text scan of all 333 `security`-matching issues; repository search does not follow file renames.

- `https://api.github.com/advisories?ecosystem=npm&affects=shadcn` — Distinct; returned an empty array.
- `https://github.com/shadcn-ui/ui/pull/9940` — Related; open proposal to scan downloaded files for harmful patterns, which would not cover a config value written after install.

Contribution fit: New issue — no open work owns the encoding gap in the chart sink.

## Proposed-Change

Emit per-chart colors as CSS custom properties on the chart element's inline `style` instead of a generated stylesheet, or validate `color` and `theme` values against a color-token pattern and quote or escape `id` as an attribute value. Reject or warn on values that cannot be a single CSS value.

## Scope-and-Constraints

- Preserve: theme switching through `THEMES`, the `--color-<key>` variable names that chart parts consume, and `var(--chart-N)` token values.
- Exclude: the CSS variable naming scheme, `chart.mdx` guidance beyond the correction, and unrelated component sinks flagged by the same audit but not reachable.
- Cost: the emitted stylesheet is shared by four bases, and any change must keep the rendered CSS equivalent for constant configs.

## Verification

- Server-render a Next.js page containing a chart whose `config` comes from request data, with a `color` value that closes the rule block, then inspect the served HTML.
- Confirm a constant `ChartConfig` still yields identical `--color-*` declarations in light and dark themes.

## Publication-Blockers

- The `</style>` breakout and its server-render dependency are unverified in a browser; only the CSS injection is established.
- Maintainers may treat an untrusted `config` as unsupported usage, which would reduce this to a hardening request.

## Next-Action

Summary: Verify SSR style breakout
Action: Render the chart in a throwaway Next.js app with a hostile `config` color and inspect the server-sent HTML and the browser result.
Done-When: A captured HTML response and browser observation show either script execution or blocked markup, resolving the open impact claim.
