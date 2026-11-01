# ISSUE-018 — registry: universal content preservation ends before final import rewrite

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

Review reference: `E14`, including the prewrite comparison boundary.
[S] Universal files bypass initial transforms but lose their type protection during comparison and final import rewriting.

## Reach-and-Impact

[S] Framework-agnostic `registry:file` or `registry:item` code can differ from its supplied original content.
The affected rewrite requires supported code syntax, loaded project aliases and a resolvable import.
No installation replay was executed.

## Evidence

- [S] `packages/registry/src/updaters/update-files.ts:165-172` explicitly promises original universal content.
- [S] The comparison and final rewrite at `:200-210,297-300,393-424` do not preserve that exemption.

## Prior-Art

User refusal and end-name conversion are distinct in `ISSUE-013` and `ISSUE-019`.
Upstream searches remain pending.

## Proposed-Change

Keep universal type metadata through comparison and exclude universal contents from the final import rewrite.

## Scope-and-Constraints

Preserve ordinary component alias adaptation and Env-specific merge semantics.
Do not treat every skipped file as a universal file.

## API-and-Compatibility

Universal byte preservation must hold for both interactive CLI and noninteractive API installation.

## Verification

Planned smoke: install universal code containing a normalizable alias into new and existing targets.
Original bytes must remain authoritative under the applicable overwrite decision.

## Publication-Blockers

A versioned public-boundary replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce universal import mutation
Action: Install a universal code file with a resolvable alias in an isolated configured consumer.
Done-When: Registry bytes, comparison decision and final target bytes are recorded.
