# ISSUE-017 — registry: alias rewriting removes legitimate src path segments

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

Review reference: `E13`, with corrected alias-root handling.
[S] `suffix.replace("/src", "")` removes text from an already alias-relative path, including real subdirectories.

## Reach-and-Impact

[S] Generated imports can reference missing or different files.
The helper smoke dropped the inner `src` from `src/components/ui/src/button.tsx`; no consumer build was run.

## Evidence

- [S] `packages/registry/src/updaters/update-files.ts:622-705` computes relative paths then removes `/src`.
- [S] The `cwd` fallback uses the project alias prefix rather than an explicitly matched alias root.

## Prior-Art

Wrong candidate selection is a separate root in `ISSUE-016`.
Upstream searches remain pending.

## Proposed-Change

Derive the actual alias mapping root and preserve its relative remainder without blanket substring removal.

## Scope-and-Constraints

Simply deleting the replacement is insufficient for the existing `@/* → ./src/*` fallback.
Preserve successful package-import handling and extension emission rules.

## API-and-Compatibility

Both specialized roots and the general project alias must retain correct module bindings.

## Verification

Planned smoke: retain inner `src` and `srcset` while keeping the general `@/* → ./src/*` fallback correct.
Check resulting imports resolve to the intended files.

## Publication-Blockers

A versioned public-boundary replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce alias suffix corruption
Action: Resolve the inner-src, srcset and general-fallback cases through the actual import updater.
Done-When: Original targets and emitted imports expose the incorrect removal and required fallback behavior.
