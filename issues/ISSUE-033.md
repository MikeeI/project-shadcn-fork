# ISSUE-033 — scaffold: canceled project prompts fall through to defaults

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

Review reference: `E30`.
[S] A canceled prompt returns partial answers, which project creation fills with defaults before scaffolding.

## Reach-and-Impact

[S] Escape or prompt-handled Ctrl-C can create a default project after the user canceled.
A real external SIGINT and intentionally skipped questions are distinct paths.
No interactive TTY replay was executed.

## Evidence

- [S] `packages/shadcn/src/utils/create-project.ts:36-65,104-108` lacks cancellation handling before defaults.
- [S] Locked prompts 2.4.2 returns partial answers after cancellation: https://unpkg.com/prompts@2.4.2/lib/index.js
- [S] Prompt raw-mode handling: https://unpkg.com/prompts@2.4.2/lib/elements/prompt.js

## Prior-Art

No matching root exists in the local ledger.
Upstream issues, PRs, discussions and releases remain unsearched for this finding.

## Proposed-Change

Handle prompt cancellation as cancellation of project creation before fallback defaults and scaffold.

## Scope-and-Constraints

Preserve fully provided template/name options and intentionally omitted questions.
Do not confuse prompt abort events with process-level SIGINT handling.

## API-and-Compatibility

Cancellation must have no project-creation side effects.

## Verification

Planned smoke: cancel the project-name TTY prompt with Escape and Ctrl-C.
No project directory may appear; fully supplied options must still scaffold normally.

## Publication-Blockers

A versioned interactive CLI replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce canceled project creation
Action: Cancel the new-project name prompt in an isolated writable directory.
Done-When: Terminal interaction and directory contents show whether scaffolding continues after cancellation.
