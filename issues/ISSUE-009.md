# ISSUE-009 — init: no-reinstall still adds an implicit starter button

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: High
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E5`.
[S] The implicit starter button depends on `selectedTemplate`, not on actual project creation.

## Reach-and-Impact

[S] Existing template-selected projects can reinstall a customized button despite `--no-reinstall`.
The subsequent component call uses overwrite mode; no CLI overwrite was executed during review.

## Evidence

- [S] `packages/shadcn/src/commands/init.ts:667-672` adds the starter button.
- [S] `packages/shadcn/src/commands/init.ts:833-840` invokes installation with `overwrite: true`.

## Prior-Art

The existing-project cleanup in `ISSUE-023` has a separate condition and side effect.
Upstream prior-art and release searches remain pending.

## Proposed-Change

Add the implicit starter button only when this invocation actually created the project.

## Scope-and-Constraints

Preserve explicitly requested components and starter installation in genuinely new projects.
The option is not a blanket prohibition on every component installation.

## API-and-Compatibility

Existing projects regain the advertised no-reinstall behavior without changing explicit add requests.

## Verification

Planned smoke: run `init --force --no-reinstall --template next` against a customized existing button.
Its bytes must remain unchanged; a genuinely new project must still receive the starter.

## Publication-Blockers

A versioned CLI reproduction and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce implicit button overwrite
Action: Reinitialize an isolated existing template project without an explicit button argument.
Done-When: The exact invocation and before/after button bytes establish whether it is implicitly replaced.
