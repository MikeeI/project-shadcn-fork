# ISSUE-054 — preview: applicable Tailwind v3 configuration changes are absent

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

Review reference: `E53`; the unsupported broader font-layout claim is excluded.
[S] Preview has no file-plan representation for an applicable Registry Tailwind-config transform.

## Reach-and-Impact

[S] A config-only item can show `No changes.` while installation modifies a parseable v3 config.
V4 or nonapplicable config shapes do not establish this example; no CLI replay was executed.

## Evidence

- [S] `packages/shadcn/src/utils/dry-run.ts:62-70,95-129` omits the transformed configuration.
- [S] `packages/registry/src/add-components.ts:103-106` calls the actual Tailwind updater.

## Prior-Art

The shorthand recognition defect is independently tracked as `ISSUE-031`.
Upstream searches remain pending.

## Proposed-Change

Include the applicable nonmutating v3 config transform as a planned file change under existing updater gates.

## Scope-and-Constraints

Preserve v4 skipping, supported syntax and existing nonapplicable-config behavior.
Do not claim every requirement summary must be a file diff.

## API-and-Compatibility

A no-changes result must not omit an actual applicable configuration mutation.

## Verification

Planned smoke: a v3 item supplies only `theme.extend`, with no Registry files or CSS.
Its config diff must match the updater output instead of showing no changes.

## Publication-Blockers

A versioned CLI parity replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce missing Tailwind preview
Action: Preview a config-only item in a supported parseable v3 consumer.
Done-When: Requested extension, preview result and actual pure config transform are captured.
