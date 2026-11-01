# ISSUE-047 — registry: explicit namespace JSON items are classified as local files

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: API
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E46`.
[S] JSON-suffix file heuristics run before explicit valid namespace parsing in both classification and fetch dispatch.

## Reach-and-Impact

[S] Root fetches for `@acme/tsconfig.json` read locally instead of using the configured remote registry.
Some recursive dependency paths normalize earlier and are not equally affected.
The review observed the parser result, not a public network fetch.

## Evidence

- [S] `packages/registry/src/registry/address.ts:45-67` classifies the address as file first.
- [S] `packages/registry/src/registry/resolver.ts:91-118` repeats that priority during fetching.

## Prior-Art

Generated GitHub ambiguity has a different correction boundary in `ISSUE-048`.
Upstream searches remain pending.

## Proposed-Change

Recognize parser-valid explicit namespaces before file suffixes and use the same classification for dispatch.

## Scope-and-Constraints

Preserve explicitly local `./@acme/tsconfig.json` and ordinary relative JSON-file behavior.
Do not reinterpret all slash-containing paths as remote repositories.

## API-and-Compatibility

An explicit namespace must retain its remote identity through public item lookup.

## Verification

Planned smoke: call public item lookup with a mapped namespace JSON item and inspect the selected URL.
Use the explicit relative path as a local control.

## Publication-Blockers

A versioned public fetch replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce namespaced JSON dispatch
Action: Fetch a JSON-named root item through a configured namespace using the public API.
Done-When: Classified scheme and remote fetch versus local read are captured.
