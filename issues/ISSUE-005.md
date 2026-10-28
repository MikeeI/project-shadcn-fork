# ISSUE-005 — registry: middleware rename overwrites an existing proxy.ts unprompted

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

Root-Cause [S]: `updateFiles` decides overwrite protection from `existsSync` on the pre-rename path, then rewrites a root `middleware.ts` to `proxy.ts` for Next 16 projects and writes to the new path, so an existing `proxy.ts` is replaced without `--overwrite` and the change is reported as a created file.

## Reach-and-Impact

Reach [S]: Any Next 16 or later project that already has a root `proxy.ts` and installs an item whose file resolves to the root `middleware.ts`; other frameworks and Next versions below 16 are unaffected because the rename is gated on the detected major version.
Impact [S]: Silent replacement of the file where Next applications commonly enforce authentication, plus a false file-status report that makes the overwrite invisible in install output. Unmeasured boundary: no install was executed, so the overwrite is established from control flow only.

## Evidence

- [S] `packages/registry/src/updaters/update-files.ts:146` — `const existingFile = existsSync(filePath)` runs before any rename.
- [S] `packages/registry/src/updaters/update-files.ts:226` — the overwrite prompt is gated on `existingFile`, so a missing `middleware.ts` skips it.
- [S] `packages/registry/src/updaters/update-files.ts:258-260` — the path is rewritten to `proxy.ts` after the prompt decision.
- [S] `packages/registry/src/updaters/update-files.ts:283` — the write targets the rewritten path.
- [S] `packages/registry/src/updaters/update-files.ts:285-294` — status is recorded from the stale `existingFile`, so the overwrite lands in `filesCreated`.
- [S] `packages/registry/src/updaters/update-files.ts:831-852` — the rename applies only to a root `middleware.ts` in a Next project with major version 16 or higher.
- [S] `packages/registry/src/registry/schema.ts:119-134` — an item may declare `middleware.ts` with any type that permits a `target`, so the file is attacker-choosable within the registry payload.

## Prior-Art

Coverage: issues(open+closed), PRs(open+closed+merged), releases; checked=2026-10-28.
Gaps: the Next 16 rename has no dedicated search term in upstream history, so related reports may exist under the `proxy.ts` rename PR.

- `https://github.com/shadcn-ui/ui/pull/11191` — Related; same file and same validation theme, different root cause (path containment, not overwrite detection).

Contribution fit: New issue — no open work owns the overwrite decision for renamed files.

## Proposed-Change

Compute the final write path before the overwrite decision, or test both the declared and the renamed path, and use the renamed path for the status report.

## Scope-and-Constraints

- Preserve: the Next 16 rename itself, the framework-version gate, and `--overwrite` semantics for every other file type.
- Exclude: the env-file merge path and path containment, which are owned by ISSUE-001 and ISSUE-004.
- Cost: the fix touches the prompt decision that every install depends on, so the ordered loop over files must stay single-pass.

## Verification

- `pnpm exec vitest run packages/registry/src/updaters/update-files.test.ts` → with an existing root `proxy.ts` and an item declaring `middleware.ts`, the write prompts or is skipped, and the status lists an update rather than a creation.

## Publication-Blockers

- No end-to-end install reproducing the overwrite.
- Whether maintainers consider prompting for the renamed path desirable, or prefer refusing the install, is undecided.

## Next-Action

Summary: Reproduce proxy overwrite silently
Action: Run a local install of an item declaring `middleware.ts` against a Next 16 fixture that already has a root `proxy.ts`.
Done-When: The install output and the file content show an unprompted replacement, captured with the exact command.
