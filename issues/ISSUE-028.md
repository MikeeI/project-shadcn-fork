# ISSUE-028 — fonts: an existing import suppresses the requested root variable

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

Review reference: `E25`, with declaration-based rather than import-based handling.
[S] A matching imported Google font can trigger early continuation before the requested root variable is installed.

## Reach-and-Impact

[S] Existing Inter usage without the requested variable can leave new theme references unbound.
The guard has heading/declaration exceptions; it does not prevent every installation with an existing import.
No consumer transform was executed.

## Evidence

- [S] `packages/registry/src/updaters/update-fonts.ts:284-318` uses the import-based skip.
- [S] Root-variable integration and declaration lookup are handled at `:369-375,476-519`.

## Prior-Art

Local import binding is independently incorrect in `ISSUE-029`.
Upstream searches remain pending.

## Proposed-Change

Install the requested root variable by declaration identity and use collision-free names when another instance is needed.

## Scope-and-Constraints

Preserve other deliberate font instances and variables.
Simply deleting the guard can introduce duplicate local declarations.

## API-and-Compatibility

An existing import is not proof that a newly requested CSS variable already exists.

## Verification

Planned smoke: start with an imported and used Inter lacking the requested root variable.
The requested variable and layout binding must appear without damaging other instances.

## Publication-Blockers

A versioned layout replay and upstream thread search remain pending.

## Next-Action

Summary: Reproduce imported font omission
Action: Apply a root-font item to a layout already using the same imported font without that variable.
Done-When: Before/after declarations, CSS references and layout classes expose the skipped integration.
