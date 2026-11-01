# ISSUE-045 — registry: directory lookup precedes an already-configured skip

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E44`.
[S] URL-less registry-add arguments require public-directory resolution before local already-configured classification.

## Reach-and-Impact

[S] Re-adding an existing private namespace can fail because it is absent from the public directory.
Directory failure can also block the local no-op; no CLI replay was executed.

## Evidence

- [S] `packages/shadcn/src/commands/registry/add.ts:94-155` loads the directory before the selected config.
- [S] Existing-key and built-in handling is reached later at `:162-178`.

## Prior-Art

Transitive namespace discovery is independently tracked as `ISSUE-043`.
Upstream searches remain pending.

## Proposed-Change

Read and classify existing or built-in namespaces before collecting genuinely new directory-dependent arguments.

## Scope-and-Constraints

Preserve lookup for new URL-less entries and explicit URL behavior.
The existing mapping must be in the actual selected target configuration.

## API-and-Compatibility

An already-configured skip should not depend on external directory availability.

## Verification

Planned smoke: re-add private `@acme` already configured in the selected file, absent from the directory.
Require unchanged config, successful skip and no directory access.

## Publication-Blockers

A versioned CLI replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce configured registry failure
Action: Re-add an existing private namespace without an explicit URL while it is absent from the directory.
Done-When: Target config, directory access and resulting skip or failure are captured.
