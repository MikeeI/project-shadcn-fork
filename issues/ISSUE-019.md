# ISSUE-019 — registry: universal TypeScript is renamed to JavaScript without conversion

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

Review reference: `E15`.
[S] `tsx: false` changes a universal file's extension before its original content is exempted from transpilation.

## Reach-and-Impact

[S] Explicit `.ts` targets can become `.js` files still containing TypeScript syntax.
This can fail when the consumer loads the generated JavaScript; no such load was executed.

## Evidence

- [S] `packages/registry/src/updaters/update-files.ts:130-137,165-179` separates renaming from universal preservation.
- [S] Planning and preview repeat the conversion at `:430-460` and `packages/shadcn/src/utils/dry-run.ts:181-195`.

## Prior-Art

Universal content rewriting is separately tracked as `ISSUE-018`.
Upstream searches remain pending.

## Proposed-Change

Exclude universal files from TS/JS extension conversion in installation, planner and preview.

## Scope-and-Constraints

Preserve ordinary component transpilation and explicit target names.
Do not force TypeScript conversion on framework-agnostic raw files.

## API-and-Compatibility

A universal file's explicit TypeScript target must remain TypeScript even in a JavaScript component project.

## Verification

Planned smoke: install `~/tool.config.ts` with a type annotation into a `tsx: false` project.
The `.ts` file must retain original bytes; an ordinary component must still convert normally.

## Publication-Blockers

A versioned consumer replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce universal extension mismatch
Action: Install an explicitly targeted TypeScript universal file in a JavaScript-configured project.
Done-When: Planned target, written filename and exact file content are captured.
