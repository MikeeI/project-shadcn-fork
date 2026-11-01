# ISSUE-036 — dependencies: overlapping Dev requests demote required runtime packages

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

Review reference: `E33`.
[S] Runtime and Dev requests are normalized independently, then installed in an order that can demote overlaps.

## Reach-and-Impact

[S] npm can finish with a required runtime package declared only as Dev.
The helper preserved both requests in the review; npm behavior is source-checked, not locally installed.

## Evidence

- [S] `packages/registry/src/updaters/update-dependencies.ts:34-39,343-369` normalizes separately and installs Dev last.
- [S] npm's category move: https://raw.githubusercontent.com/npm/cli/v11.6.2/workspaces/arborist/lib/add-rm-pkg-deps.js

## Prior-Art

Existing Dev-only suppression is independently tracked as `ISSUE-035`.
Upstream searches remain pending.

## Proposed-Change

Resolve category overlaps before installation, prefer runtime for compatible specs and explicitly handle conflicts.

## Scope-and-Constraints

Preserve independent nonoverlapping requests.
Reordering alone is not a complete category policy and must not silently choose an incompatible version.

## API-and-Compatibility

A package required at runtime must remain a production dependency after the entire operation.

## Verification

Planned smoke: request the same compatible package in both categories in an empty npm project.
Its final manifest location and production-only resolution must remain correct.

## Publication-Blockers

A versioned npm replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce dependency category demotion
Action: Install one package requested simultaneously as runtime and Dev through the updater.
Done-When: Both argument lists and final manifest category are captured with the npm version.
