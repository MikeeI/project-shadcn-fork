# ISSUE-046 — registry: accepted namespace names cannot be resolved later

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

Review reference: `E45`, with the corrected CLI argument-parser owner.
[S] CLI/config acceptance and item-address parsing apply incompatible namespace grammars.

## Reach-and-Impact

[S] A successfully stored `@acme-` mapping is not recognized in `@acme-/button` and can fall back to built-ins.
The review exercised parser helpers, not a persisted CLI-to-fetch roundtrip.

## Evidence

- [S] `packages/shadcn/src/commands/registry/add.ts:52-66` requires only a leading `@`.
- [S] `packages/registry/src/registry/parser.ts:1-23` requires an alphanumeric end character.
- [S] `packages/registry/src/registry/builder.ts:26-54` uses the parsed namespace or its built-in fallback.

## Prior-Art

JSON suffix classification is separate in `ISSUE-047`.
Upstream prior-art search remains pending.

## Proposed-Change

Use one intentional namespace grammar for CLI acceptance, config loading and item parsing before persistence.

## Scope-and-Constraints

Either support documented edge names or reject them before writing; the intended accepted grammar needs confirmation.
Do not add another independent regex or silently route an accepted namespace elsewhere.

## API-and-Compatibility

Previously accepted persisted names require an explicit compatibility decision.

## Verification

Planned smoke: add an edge-name namespace with an explicit URL, then resolve its item publicly.
Expect prewrite rejection or exactly the stored mapping, never an unrelated fallback.

## Publication-Blockers

A versioned persisted roundtrip, intended grammar decision and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce namespace grammar mismatch
Action: Add a trailing-hyphen namespace through the CLI and resolve an item from it.
Done-When: Stored key, parser result and selected source URL are captured.
