# ISSUE-034 — dependencies: Expo receives packages after the passthrough separator

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

Review reference: `E31`.
[S] Runtime package names follow Expo's `--`, while Dev mode receives the single malformed argument `-- -D`.

## Reach-and-Impact

[S] Runtime requests bypass Expo's package-list SDK processing and Dev installation can fail argument parsing.
This does not establish that runtime packages never install; no Expo process was executed.

## Evidence

- [S] `packages/registry/src/updaters/update-dependencies.ts:393-408` constructs both argument lists.
- [S] Expo 54 parser distinguishes extras from packages: https://unpkg.com/@expo/cli@54.0.0/build/src/utils/variadic.js
- [S] Supported Dev flags: https://unpkg.com/@expo/cli@54.0.0/build/src/install/resolveOptions.js

## Prior-Art

Package-category errors are separately tracked as `ISSUE-035` and `ISSUE-036`.
Upstream searches remain pending.

## Proposed-Change

Put package names before the separator and pass supported `--dev` as its own Expo argument.

## Scope-and-Constraints

Preserve package-manager passthrough options.
The parser contract was checked against Expo CLI 54; additional supported consumer versions need confirmation.

## API-and-Compatibility

Expo must retain control of SDK-compatible versions and the requested dependency category.

## Verification

Planned smoke: use a supported Expo consumer for SDK-bound runtime and separate Dev requests.
Record exact arguments, selected versions and manifest categories.

## Publication-Blockers

A versioned Expo replay, supported-version check and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce Expo argument handling
Action: Install one SDK-bound runtime package and one Dev package through the affected updater.
Done-When: Expo version, actual argv, SDK decision and package manifest result are captured.
