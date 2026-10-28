# ISSUE-004 — registry: envVars values are written unescaped and without confirmation

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-10-28
Updated: 2026-10-28
Source: `upstream/main@96da9cc7`

## Root-Cause

Root-Cause [S]: `updateEnvVars` writes `${key}=${value}` into the project's env file without quoting, escaping or key validation, and unlike the file writer it has no interactive gate, so a value containing a newline appends additional assignments.

## Reach-and-Impact

Reach [S]: Every install whose resolved tree carries `envVars`, on any framework, including named-registry installs with confirmation disabled.
Impact [S]: Registry data can add environment variables the item never declared, in the highest-precedence env file Next.js reads; keys already present in the file are preserved, so no existing value is overwritten. Unmeasured boundary: whether an injected key changes application behavior depends on the consumer reading that key.

## Evidence

- [S] `packages/registry/src/registry/schema.ts:164` — `registryItemEnvVarsSchema` is `z.record(z.string(), z.string())`, so keys and values admit any characters.
- [S] `packages/registry/src/updaters/update-env-vars.ts:51-53` — `Object.entries(envVars).map(([key, value]) => `${key}=${value}`)` produces the written text with no escaping.
- [S] `packages/registry/src/updaters/update-env-vars.ts:15-33,96-104` — the function takes only `silent`; there is no confirmation or `interactive` option.
- [S] `packages/registry/src/updaters/update-env-vars.ts:41-49` — the target defaults to `.env.local` at the project root and prefers an existing env file.
- [S] `packages/registry/src/env-helpers.ts:28-53` — `parseEnvContent` splits on newlines, so an embedded newline reads back as an additional assignment.
- [S] `packages/registry/src/env-helpers.ts:74-99` — `mergeEnvContent` appends only keys absent from the existing file, which bounds the impact to new keys.
- [S] `packages/registry/src/add-components.ts:108-110` — `updateEnvVars` runs before `updateFiles` and before the stylesheet write.
- [S] `apps/v4/content/docs/registry/examples.mdx:963-986` — the docs present `envVars` with concrete values and warn only against production variables.

## Prior-Art

Coverage: issues(open+closed), PRs(open+closed+merged), releases, GitHub advisory REST for npm `shadcn` and `@shadcn/registry`; checked=2026-10-28.
Gaps: no search of closed issues by the `envVars` symbol beyond title matching.

- `https://api.github.com/advisories?ecosystem=npm&affects=shadcn` — Distinct; returned an empty array.
- `https://github.com/shadcn-ui/ui/pull/11191` — Related; same validation theme for file paths, different field and sink.

Contribution fit: New issue — the unescaped write and the missing confirmation have no owner.

## Proposed-Change

Validate keys against the env-identifier pattern, reject values containing newlines, quote or escape values once on write, and route the write through the same confirmation the file writer uses for non-interactive installs.

## Scope-and-Constraints

- Preserve: the documented merge-only semantics, existing values, and the choice between `.env.local` and an existing env file.
- Exclude: the `.env` handling in `updateFiles`, secret detection, and any change to env-file discovery.
- Cost: rejection rules change the accepted schema surface, so published registry items with unusual values start failing.

## Verification

- `pnpm exec vitest run packages/registry/src/updaters/update-env-vars.test.ts` → a value containing a newline is rejected instead of appending a second assignment, and the documented examples still write.

## Publication-Blockers

- No reproduction against the shipped CLI showing the resulting env file.
- Whether maintainers accept rejection as a breaking schema change is undecided.

## Next-Action

Summary: Confirm env newline injection
Action: Run a local install with an `envVars` value containing a newline and inspect the resulting env file.
Done-When: The written file shows either the injected assignment or the rejection, captured with the exact command and file content.
