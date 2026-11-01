# ISSUE-039 — env: exported existing keys are duplicated during merge

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

Review reference: `E37`.
[S] The Env parser interprets `export KEY` as a key rather than recognizing the existing assignment to `KEY`.

## Reach-and-Impact

[S] Both Env merge callers can append a duplicate assignment that overrides the original effective value.
The review observed `export KEY=old` followed by `KEY=new` decoding as new; no updater file write was executed.

## Evidence

- [S] `packages/registry/src/env-helpers.ts:28-49,56-65,74-86` uses the incorrect key identity.
- [S] Supported export syntax: https://unpkg.com/dotenv@17.2.1/lib/main.js

## Prior-Art

`ISSUE-004` owns initial object serialization; value-quote loss has its own boundary in `ISSUE-040`.
Upstream searches remain pending.

## Proposed-Change

Recognize key identity with supported dotenv grammar and retain original source when computing and applying the merge.

## Scope-and-Constraints

Preserve comments and existing values in the same selected file.
Do not infer a guarantee spanning every framework Env file.

## API-and-Compatibility

A valid exported key must satisfy the existing-key check just like an ordinary assignment.

## Verification

Planned smoke: merge `export KEY=old` with incoming `KEY=new` through both updater callers.
No duplicate assignment may appear and the loaded value must stay old.

## Publication-Blockers

A versioned public merge replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce exported key overwrite
Action: Install an incoming Env key already present with an export prefix.
Done-When: Existing content, merge output and consumer-decoded value are captured.
