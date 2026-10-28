# ISSUE-001 — registry: empty-string target bypasses path validation

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

Root-Cause [S]: `validateFilesTarget` chooses the field to validate with `file?.target ?? file?.path` and then skips falsy values, while `resolveFilePath` chooses with `if (file.target)`; an empty-string `target` is therefore validated nowhere and resolves through `file.path`, which reaches `fs.writeFile` after `resolveNestedFilePath` and `path.join` without a second containment check.

## Reach-and-Impact

Reach [S]: Every project and workspace install that resolves a registry tree containing a file with `target: ""` and a `..`-bearing `path`; both paths call `resolveAndValidateRegistryTree`.
Impact [S]: File creation with attacker-chosen content outside `config.resolvedPaths.cwd`, bounded only by the invoking user's permissions; created files never prompt because the overwrite prompt is gated on `existingFile`. Unmeasured boundary: the shipped resolver was not invoked and no file was written during this investigation.

## Evidence

- [S] `packages/registry/src/add-components.ts:463-469` — `const locationField = file?.target ?? file?.path` followed by `if (!locationField) continue` skips validation for an empty string.
- [S] `packages/registry/src/resolve-file-path.ts:46` — `if (file.target)` treats the empty string as absent and falls through to the `file.path` branch.
- [S] `packages/registry/src/resolve-file-path.ts:161-181` — `resolveNestedFilePath` returns every segment after the first match of the target directory's last segment, including `..` segments.
- [S] `packages/registry/src/resolve-file-path.ts:74-78` — `path.join(targetDir, relativePath)` normalizes the traversal out of the target directory.
- [S] `packages/registry/src/updaters/update-files.ts:115-128,262-283` — the resolved path reaches `fs.writeFile` with no `isSafeTarget` or containment recheck.
- [S] `packages/registry/src/updaters/update-files.ts:226` — the overwrite prompt requires `existingFile`, so newly created files are written silently.
- [S] `packages/registry/src/registry/schema.ts:119-134` — both the required-target and optional-target branches accept `""`.
- [S] `packages/registry/src/add-components.ts:88,158` — project and workspace installs both validate through `resolveAndValidateRegistryTree`.
- [O] `grep -rn "isSafeTarget" packages apps/v4` → the only production call site is `packages/registry/src/add-components.ts:468`; environment=Linux x64, checkout 7edc7f33.
- [S] Arithmetic reproduction using a transcription of `resolveNestedFilePath` plus `path.join` with `cwd=/home/dev/app`: `components/../../../../../root/.gitconfig` resolves to `/root/.gitconfig`, `components/../../../../home/dev/.npmrc` to `/home/dev/.npmrc`, `components/shadcn-theme.js` to `/home/dev/app/components/shadcn-theme.js`.
- [O] `node -e` probe printed those resolutions and the displayed paths `../../../root/.gitconfig`, `../.npmrc`, `components/shadcn-theme.js`; environment=node v26.12.1, Linux x64. The probe transcribed the resolver body and did not call it.

## Prior-Art

Coverage: issues(open+closed), PRs(open+closed+merged), releases, GitHub advisory REST for npm `shadcn` and `@shadcn/registry`; checked=2026-10-28.
Gaps: renamed historical paths were not fully walked with local git log; the advisory query for the older `shadcn-ui` package returned HTTP 403.

- `https://github.com/shadcn-ui/ui/pull/11191` — Related; merged 2026-07-16 and shipped in 4.13.1, fixing validation for items without an explicit `target` and explicitly deferring a check of the finally resolved path before write. Distinct root cause: empty string versus absent field.
- `https://github.com/shadcn-ui/ui/pull/11192` — Related; release notes naming #11191, #11195, #11196 under `shadcn@4.13.1`.
- `https://api.github.com/advisories?ecosystem=npm&affects=shadcn` — Distinct; returned an empty array, so no public advisory covers the empty-string variant.

Contribution fit: New issue — the defect is a one-character escape of the guard added for #11191 and no open work owns it.

## Proposed-Change

Validate the path that is actually written: check `isSafeTarget(filePath, config.resolvedPaths.cwd)` immediately before `fs.mkdir` and `fs.writeFile` in `updateFiles`. Reject `target: ""` at the schema boundary and make `validateFilesTarget` and `resolveFilePath` share one presence predicate.

## Scope-and-Constraints

- Preserve: alias-root targets that resolve outside `cwd` for configured workspace aliases, and successful installs for items whose paths stay inside the project.
- Exclude: `--path`, which #11191 deliberately leaves unvalidated as an operator-controlled flag.
- Cost: the write sink is shared by every install path, so the guard must allow configured alias roots or it breaks workspace installs.

## Verification

- `pnpm exec vitest run packages/registry/src/updaters/update-files.test.ts packages/registry/src/is-safe-target.test.ts` → an item with `target: ""` and a traversing `path` is rejected before any write, and existing in-project installs still succeed.

## Publication-Blockers

- No end-to-end reproduction against the shipped CLI; the escape is proven by source and transcribed arithmetic only.
- Fix scope for configured alias roots is undecided.

## Next-Action

Summary: Reproduce write escape end-to-end
Action: Run the built CLI against a local registry item with `target: ""` in a throwaway project and observe the written path.
Done-When: The CLI reports an install that writes outside the project root, captured with the exact command and path.
