# ISSUE-010 — add: dry-run initializes or scaffolds before preview

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

Review reference: `E6`.
[S] Missing-config initialization and empty-project scaffolding execute before the dry-run branch.

## Reach-and-Impact

[S] Preview flags can write configuration, install components or scaffold a project after prerequisite prompts.
Configured projects do not necessarily take these paths; no end-to-end preview was executed.

## Evidence

- [S] `packages/shadcn/src/commands/add.ts:74,178-277,304-306` promises no writes but branches after initialization.
- [S] `packages/shadcn/src/commands/init.ts:782-783,832-841` writes config and installs components.

## Prior-Art

The preview-model divergences have independent owners and are tracked separately.
Upstream issues, PRs, discussions and releases remain unsearched for this cause.

## Proposed-Change

Enter read-only preview before initialization and report missing prerequisites without mutating them.

## Scope-and-Constraints

Preserve ordinary initialization and the fact that `--diff` and `--view` imply dry-run.
Existing prompts do not silently remove the no-write contract.

## API-and-Compatibility

Unprepared projects receive a prerequisite error rather than an unexpected setup operation.

## Verification

Planned smoke: preview without `components.json` and in an empty directory, accepting existing prompts.
The file tree and package-manager invocation count must remain unchanged.

## Publication-Blockers

A versioned CLI replay with filesystem and process observations remains pending, as does upstream prior art.

## Next-Action

Summary: Reproduce dry-run setup writes
Action: Run a preview in an isolated project missing configuration and capture its side effects.
Done-When: The invocation, prompt answers, file changes and package-manager calls are recorded.
