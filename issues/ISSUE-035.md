# ISSUE-035 — dependencies: dev-only presence suppresses a runtime requirement

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

Review reference: `E32`.
[S] Installed runtime and Dev packages are combined before a bare runtime request is deemed already satisfied.

## Reach-and-Impact

[S] A production dependency can remain Dev-only and be absent when Dev packages are omitted.
The review observed an empty normalized runtime list for dev-only zod; no installation was executed.
Bundled consumers are not uniformly affected.

## Evidence

- [S] `packages/registry/src/updaters/update-dependencies.ts:156-176,194-201` ignores the installed category.
- [S] Registry dependency categories are distinct in `apps/v4/content/docs/registry/registry-item-json.mdx:133-158`.

## Prior-Art

`ISSUE-036` concerns simultaneous new requests rather than an existing Dev-only dependency.
Upstream searches remain pending.

## Proposed-Change

Normalize by category, preserve the existing version spec and explicitly promote required Dev-only packages.
Use the package manager's production-save option where needed, including npm `--save-prod`.

## Scope-and-Constraints

Preserve existing user version constraints and ordinary idempotent requests.
Do not assume omitting `--save-dev` alone promotes an existing npm entry.

## API-and-Compatibility

Required runtime packages must be declared for production installation.

## Verification

Planned smoke: start with only `devDependencies.zod`, then add a bare runtime requirement.
The runtime field must retain the original spec and resolve after Dev packages are omitted.

## Publication-Blockers

A versioned installation replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce missing runtime promotion
Action: Add a runtime dependency already present only in devDependencies in a throwaway consumer.
Done-When: Normalized request, manifest category and production-only resolution are recorded.
