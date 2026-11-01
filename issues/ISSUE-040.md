# ISSUE-040 — env: merging removes quotes required to preserve new values

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

Review reference: `E38`.
[S] Merge parsing strips incoming quotes and then rebuilds new assignments from unquoted parsed values.

## Reach-and-Impact

[S] A valid incoming `MESSAGE="hello # world"` becomes a different value when merged into an existing file.
The review observed helper output decoding to hello; this was not an end-to-end updater write.

## Evidence

- [S] `packages/registry/src/env-helpers.ts:45-49,74-86` dequotes and reserializes.
- [S] `packages/registry/src/updaters/update-files.ts:266-283` uses this merge for an existing Env target.

## Prior-Art

`ISSUE-004` concerns initial serialization of an object, not preservation of already formatted file content.
Upstream prior-art search remains pending.

## Proposed-Change

Preserve complete new assignments, including quotation and multiline structure, when appending them.

## Scope-and-Constraints

Retain existing target lines and append-only key policy.
Coordinate with `ISSUE-004` so a newly correct encoding is not destroyed by this merge.

## API-and-Compatibility

Semantically valid incoming Env file syntax must survive merge without a changed value.

## Verification

Planned smoke: merge the quoted hash value into an existing file and decode with the actual consumer loader.
The value must roundtrip exactly; a newly created file is the control.

## Publication-Blockers

A versioned public-boundary replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce env quote loss
Action: Install an Env registry file containing a quoted hash value into an existing Env target.
Done-When: Incoming syntax, written content and consumer-decoded value are captured.
