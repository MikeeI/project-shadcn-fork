# ISSUE-004 — registry: envVars values are written unescaped and without confirmation

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-10-28
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E39` (retained here rather than allocated a duplicate ID).
Root-Cause [S]: `updateEnvVars` writes `${key}=${value}` without quoting, escaping or key validation.
Embedded newlines can add assignments, while an ordinary value containing `#` can be truncated by the loader.
The writer also lacks the interactive gate available in file installation; confirmation policy remains unresolved.

## Reach-and-Impact

Reach [S]: Installs whose resolved tree carries `envVars`, including named-registry installs without confirmation.
Impact [S]: Raw values can decode differently or add assignments not declared as separate keys.
Ordinary recognized keys in the chosen file are preserved; the export-prefix exception is owned by `ISSUE-039`.
Application impact depends on the consumer loader and which keys the application reads.
Boundary: the review used helper/codec observations, not a shipped CLI write reproduction.

## Evidence

- [S] `packages/registry/src/registry/schema.ts:164` — `registryItemEnvVarsSchema` is `z.record(z.string(), z.string())`, so keys and values admit any characters.
- [S] `packages/registry/src/updaters/update-env-vars.ts:51-53` — `Object.entries(envVars).map(([key, value]) => `${key}=${value}`)` produces the written text with no escaping.
- [S] `packages/registry/src/updaters/update-env-vars.ts:15-33,96-104` — the function takes only `silent`; there is no confirmation or `interactive` option.
- [S] `packages/registry/src/updaters/update-env-vars.ts:41-49` — the target defaults to `.env.local` at the project root and prefers an existing env file.
- [S] `packages/registry/src/env-helpers.ts:28-53` — `parseEnvContent` splits on newlines, so an embedded newline reads back as an additional assignment.
- [S] `packages/registry/src/env-helpers.ts:74-99` — `mergeEnvContent` appends only keys absent from the existing file, which bounds the impact to new keys.
- [S] `packages/registry/src/add-components.ts:108-110` — `updateEnvVars` runs before `updateFiles` and before the stylesheet write.
- [S] `apps/v4/content/docs/registry/examples.mdx:963-986` — the docs present `envVars` with concrete values and warn only against production variables.
- [S] Unquoted hash handling in the supported dotenv parser: https://unpkg.com/dotenv@17.2.1/lib/main.js
- [S] The separate merge defect in `ISSUE-040` can destroy newly correct encoding and must be coordinated.

## Prior-Art

Coverage: issues(open+closed), PRs(open+closed+merged), releases, GitHub advisory REST for npm `shadcn` and `@shadcn/registry`; checked=2026-10-28.
Gaps: the symbol-specific search and the recorded 2026-10-28 coverage need refresh before publication.

- `https://api.github.com/advisories?ecosystem=npm&affects=shadcn` — Distinct; returned an empty array.
- `https://github.com/shadcn-ui/ui/pull/11191` — Related; same validation theme for file paths, different field and sink.

Contribution fit: A new issue is a candidate; no owner was identified in the recorded limited search.

## Proposed-Change

Validate keys and encode values according to the supported Env-loader grammar before writing.
Keep that encoding intact through the merge owned by `ISSUE-040`.
Rejecting or encoding embedded newlines, and adding confirmation, require an explicit compatibility decision.

## Scope-and-Constraints

- Preserve: documented merge-only semantics, existing recognized values and existing target-file selection.
- Own: object-to-Env encoding; the independently formatted Registry-file merge defect belongs to `ISSUE-040`.
- Cost: lossless writing needs a defined loader grammar; rejection may narrow accepted Registry inputs.

## API-and-Compatibility

Do not equate arbitrary JSON string escaping with the supported dotenv grammar.
Accepted values must roundtrip or fail explicitly before writes; no implementation policy is selected yet.

## Verification

- Existing suite, not executed: `pnpm exec vitest run packages/registry/src/updaters/update-env-vars.test.ts`.
- Planned CLI replay: install hash and multiline values into new and existing Env files.
- Require exact consumer-decoded values or explicit prewrite rejection, never accidental extra assignments.

## Publication-Blockers

- No versioned shipped-CLI reproduction captures the resulting file and consumer-decoded value.
- Accepted value grammar and confirmation-policy adoption remain undecided.

## Next-Action

Summary: Reproduce env value roundtrip
Action: Install a hash-containing Env value through the CLI into new and existing target-file states.
Done-When: Exact command, CLI/loader versions, written contents and decoded values are captured.
