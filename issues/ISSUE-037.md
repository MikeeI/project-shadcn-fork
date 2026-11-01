# ISSUE-037 — scaffold: fallback replaces a successfully detected package manager

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

Review reference: `E34`.
[S] With fallback enabled, ordinary detected agents can be replaced by the invoking User-Agent.

## Reach-and-Impact

[S] Scaffolding can select another manager's template adaptation, lockfile handling and install command.
Special early-return agents differ; no scaffold or detector process was executed.

## Evidence

- [S] `packages/registry/src/get-package-manager.ts:11-22` ignores ordinary detected results in fallback mode.
- [S] `packages/shadcn/src/utils/create-project.ts:73-75,104-108` uses that mode for scaffolding.

## Prior-Art

No matching cause exists in the local ledger.
Upstream issues, PRs, discussions and releases remain unsearched for this finding.

## Proposed-Change

Return every successfully normalized detected agent before using User-Agent as a true fallback.

## Scope-and-Constraints

Preserve supported version-specific agent normalization and the missing-detection fallback.
Do not declare the invoking manager universally wrong.

## API-and-Compatibility

Project detection must outrank fallback data when detection succeeds.

## Verification

Planned smoke: detect pnpm, npm and classic Yarn projects under conflicting User-Agents.
The scaffold choice must follow the detected project manager.

## Publication-Blockers

A versioned detector/scaffold replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce detected manager replacement
Action: Invoke scaffold manager selection with a detected project and conflicting invocation agent.
Done-When: Detection input, normalized result and selected scaffold/install manager are recorded.
