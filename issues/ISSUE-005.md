# ISSUE-005 — registry: middleware rename overwrites an existing proxy.ts unprompted

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

Review reference: `E1` (retained).
Root-Cause [S]: `updateFiles` checks overwrite protection using the pre-rename path.
It then renames root `middleware.ts` to `proxy.ts` for Next 16 without reevaluating target existence.
An existing `proxy.ts` can therefore be replaced and reported as created despite absent `--overwrite`.

## Reach-and-Impact

Reach [S]: Next 16 or later projects with a root `proxy.ts` installing a file resolved as root `middleware.ts`.
Other frameworks and detected Next versions below 16 do not enter the rename branch.
Impact [S]: Silent replacement of existing `proxy.ts`, where Next applications commonly enforce authentication.
The stale file-status report hides this overwrite in install output.
Unmeasured boundary: no install was executed; overwrite is established from control flow only.

## Evidence

- [S] `packages/registry/src/updaters/update-files.ts:146` — `const existingFile = existsSync(filePath)` runs before any rename.
- [S] `packages/registry/src/updaters/update-files.ts:226` — the overwrite prompt is gated on `existingFile`, so a missing `middleware.ts` skips it.
- [S] `packages/registry/src/updaters/update-files.ts:258-260` — the path is rewritten to `proxy.ts` after the prompt decision.
- [S] `packages/registry/src/updaters/update-files.ts:283` — the write targets the rewritten path.
- [S] `packages/registry/src/updaters/update-files.ts:285-294` — status is recorded from the stale `existingFile`, so the overwrite lands in `filesCreated`.
- [S] `packages/registry/src/updaters/update-files.ts:831-852` — the rename applies only to a root `middleware.ts` in a Next project with major version 16 or higher.
- [S] `packages/registry/src/registry/schema.ts:119-134` — an item may declare `middleware.ts` with any type that permits a `target`, so the file is attacker-choosable within the registry payload.
- [S] Checked 2026-11-01: the updater is unchanged between `96da9cc7` and canonical `c0d63c339d6cd2fc26135590a54cf1f362269451`.

## Prior-Art

Coverage: issues(open+closed), PRs(open+closed+merged), releases; checked=2026-10-28.
Gaps: the rename-specific search and the recorded 2026-10-28 coverage need refresh before publication.

- `https://github.com/shadcn-ui/ui/pull/11191` — Related; same file and same validation theme, different root cause (path containment, not overwrite detection).

Contribution fit: A new issue is a candidate; no owner was identified in the recorded limited search.

## Proposed-Change

Compute the effective write path before existence checks, content comparison and prompts.
Retain original middleware identity for `transformNext`; use the effective path for writing and status.

## Scope-and-Constraints

- Preserve: the Next 16 rename itself, the framework-version gate, and `--overwrite` semantics for every other file type.
- Exclude: the env-file merge path and path containment, which are owned by ISSUE-001 and ISSUE-004.
- Cost: this shared overwrite boundary must preserve ordered processing and project/workspace routing.

## Verification

- Existing suite, not executed: `pnpm exec vitest run packages/registry/src/updaters/update-files.test.ts`.
- Planned CLI replay: deny replacement of an existing proxy and require unchanged bytes plus skipped status.
- With overwrite enabled, require updated rather than created status and the intended proxy transformation.

## Publication-Blockers

- No end-to-end install reproducing the overwrite.
- Whether maintainers consider prompting for the renamed path desirable, or prefer refusing the install, is undecided.

## Next-Action

Summary: Reproduce proxy overwrite silently
Action: Run a local install of an item declaring `middleware.ts` against a Next 16 fixture that already has a root `proxy.ts`.
Done-When: The install output and the file content show an unprompted replacement, captured with the exact command.
