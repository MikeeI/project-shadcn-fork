# project-shadcn-fork

<essential-rule>
AGENTS.md is the sole authoritative project context file.
Read and edit AGENTS.md directly.
</essential-rule>

## Development Rules

Before launching agents, apply skill-xray, skill-expert, and skill-brutal to the task.
Surface expert-level issues, non-obvious issues, blindspots, stale assumptions, and hidden dependencies.
Also surface missed constraints, edge cases, false positives, verification gaps, overclaims, and weak assumptions.
Identify improvement potential, inefficiencies, and what is wrong without softening.
Use these findings to design safe slices, sequencing, checks, and boundaries for complete agent results.

Every agent prompt must require skill-xray, skill-expert, and skill-brutal for the assigned scope before acting.
It must surface non-obvious issues, blindspots, stale assumptions, hidden dependencies, and edge cases.
It must also surface verification gaps, overclaims, failure modes, weak assumptions, and what is wrong.
The agent must adjust its approach, challenge its assumptions, and flag misleading or incomplete output risks.

Implementation assignments must cover existing patterns, callers, exported-symbol consumers, and failure modes.
They must also cover concurrency safety and lifecycle cleanup.
Each assignment must state `Test decision: none` or `Test decision: update`.
`update` must name the exact existing test that follows an intentional contract change.
Never request new tests.
Prohibit broad edits, unrelated cleanup, and unassigned files.

No vague agents.
Each assignment needs exact targets, non-goals, evidence anchors, acceptance criteria, and an output contract.

Commit completed units continuously.
Before each commit, use skill-git-commit-format to determine whether staged effects are one coherent unit.
The skill owns commit-message format and evidence.
After the boundary is valid, run the repository-owned commit and push workflow.
Do not commit every trivial edit immediately or defer unrelated work into one end-of-session commit.

Every project-level quality command is quiet by default and verbose on demand.
This policy applies regardless of language or toolchain.
It covers Make targets, package scripts, Python CLIs, shell quality gates, and test runners.
Successful checks print only compact status such as `format: ok`, `lint: ok`, `test: ok`, or `check: ok`.
On failure, exit non-zero and print the failing step, exit code, and enough output to act without rerunning.
Full raw output must remain available through `--verbose`, `VERBOSE=1`, or the underlying tool's verbose mode.
New quality commands and future language setup must follow this policy instead of inventing another logging contract.

# Repository Guidelines

## Project Overview

This checkout contains shadcn/ui, an open-source component registry, documentation website, and CLI.
The `shadcn` CLI installs customizable component source into consumer projects rather than a closed component bundle.
The repository is a pnpm workspace coordinated by Turborepo; upstream owns its manifests, tooling, and release process.

## Fork & Upstream Contribution Intent

- Official upstream: [shadcn-ui/ui](https://github.com/shadcn-ui/ui).
- This checkout is the [MikeeI/project-shadcn-fork](https://github.com/MikeeI/project-shadcn-fork) fork, not an independently owned product.
- The goal is to support upstream with evidence-backed issues, comments, and pull requests.
- `ISSUES.md` provides the compact finding overview and global ID allocator.
- `issues/ISSUE-NNN.md` owns the complete durable record for one root cause.
- `FORMAT.md` owns research, drafting, implementation authorization, approval, and publication rules.
- Prefer small, well-scoped corrections with outsized maintainer or user value.
- Recommend a pull request when a bounded verified fix is ready and no active implementation owns it.
- Otherwise recommend a comment, a new issue, or continued investigation according to the evidence.
- Apply `skill-fork-contribution-tracking` for ledger, lifecycle, personal-branch, and upstream handoff work.
- Apply `skill-maintainer-communication` before external issues, pull requests, reviews, comments, or discussions.
- Search existing work first and follow current upstream templates and disclosure rules.
- Apply `skill-semantic-compression-3` when authoring or restructuring tracking content.
- Apply `skill-git-commit-format` while respecting explicit upstream contribution conventions.
- Never choose `Authorized-Work` or `Publication-Target` on the user's behalf.
- `Research-and-Reporting` permits research, issues, and comments but no source implementation.
- `Pull-Request-Implementation` authorizes only the scoped implementation recorded for that finding.
- Base upstream contribution branches on current `upstream/main`.
- Keep fork-only context, ledgers, configuration, and personal commits out of upstream contribution diffs.
- Reproduce claimed bugs against current upstream and run the narrowest conclusive verification.
- Publish one coherent root cause per issue, comment, or pull request.

### Contribution viability

- Checked on 2026-10-28 with `gh pr list -R shadcn-ui/ui --state closed --limit 100`.
- The 100 sampled closed PRs include 1 merged fork PR and 53 closed-unmerged fork PRs: https://github.com/shadcn-ui/ui/pulls?q=is%3Apr+is%3Aclosed
- Sampled closure timestamps span 2026-10-16 through 2026-10-27; this is an indicator, not proof of rejection policy.
- Recommend `Research-and-Reporting` initially; the user may explicitly select `Pull-Request-Implementation`.
- No finding-level work mode or publication target is selected by this setup.
- Upstream flags unsigned PR commits and closes still-unsigned flagged PRs after 14 days.
- Before a source PR, satisfy the current signing policy in `.github/workflows/signed-commits*.yml`.

## Branch Roles

- The canonical upstream contribution branch is `main`.
- The `personal` branch owns fork context and contribution tracking.
- Upstream-ready source work uses separate `feat/issue-NNN`, `fix/issue-NNN`, or `chore/issue-NNN` branches off `upstream/main`.
- Remotes: `origin` is `git@github.com:MikeeI/project-shadcn-fork.git`; `upstream` is `https://github.com/shadcn-ui/ui.git`.
- A global Git URL rewrite may display the upstream HTTPS URL as SSH without changing its stored canonical URL.
- Initialization baseline: `upstream/main@96da9cc7f1aa10580a3741664e2a77c721206c00`.
- `main` matches the upstream default and all five sampled recently merged PR bases.
- Base evidence includes https://github.com/shadcn-ui/ui/pull/12498 and https://github.com/shadcn-ui/ui/pull/12485.
- Never push `personal` to upstream or open an upstream pull request from it.

## Finding and Contribution Ledger

- At the start of every agent session, agents MUST read root `ISSUES.md` before repository work.
- `ISSUES.md` owns the global `Next finding ID` allocator and compact cross-finding overview.
- Each `issues/ISSUE-NNN.md` owns one finding's state, evidence, drafts, and next action.
- `FORMAT.md` is authoritative for research, drafting, implementation boundaries, and publication format.
- Before adding a finding, search the index and every relevant issue record for the same symptom or root cause.
- New findings MUST use `Next finding ID`.
- Create the issue file, add its index row, and increment the allocator together.
- Finding IDs use `ISSUE-NNN`, start at `ISSUE-001`, and remain permanent.
- Never reuse, renumber, or scope IDs by subsystem, status, session, or contribution type.
- Update the issue file and `ISSUES.md` together after state, authorization, target, priority, next action, or reference changes.
- Every issue record MUST use the field and section contract in `FORMAT.md`.
- New findings start with `State: Investigating`, `Authorized-Work: Not-Selected`, and `Publication-Target: Not-Selected`.
- New findings use `External-Reference: Not published.` until an external reference exists.
- Keep findings Investigating until currentness, prior art, impact, and correction value are evidence-backed.
- Clone detectors, AST matches, text similarity, shared names, and TODOs produce candidates only.
- A duplication finding requires shared change pressure, realistic drift, and simpler consolidation.
- The user selects `Authorized-Work` for each finding.
- `Research-and-Reporting` MUST NOT implement the finding.
- `Pull-Request-Implementation` MAY implement only the recorded scope after research resolves callers and failure modes.
- Pull-request work MUST verify behavior, commit, push, and reach `PR-Ready` before publication.
- Show the exact draft and target before publishing an issue, comment, or pull request to official upstream.
- Publish to official upstream only after the user approves the exact current draft and target.
- Any draft or target change requires showing the complete current draft and target again before publication.
- Run the read-only validator bundled with `skill-fork-contribution-tracking` after every ledger mutation.
- Record the final external URL in `External-Reference` immediately after publication.
- Keep `FORMAT.md`, `ISSUES.md`, `issues/`, and fork-only `AGENTS.md` changes out of upstream contribution diffs.

### External publication approval

Only an external issue, comment, review, discussion, or pull request write is approval-gated.
Before publication, read current contribution guidance and explain applicable project policy.
The human must be able to review and own every submission statement.
Fork commits, pushes, tracking updates, and source implementation follow the active repository contract.

## Architecture & Data Flow

- `packages/shadcn/src/index.ts` registers Commander commands implemented in `packages/shadcn/src/commands/`.
- `packages/registry` owns registry fetching, resolution, schemas, project configuration, transforms, and installation.
- `packages/shadcn` depends on `@shadcn/registry`; inspect that package before changing shared installation behavior.
- `apps/v4` is the Next.js documentation and registry-serving application, using React and Tailwind CSS 4.
- Authored components live in `apps/v4/registry/bases/{base,radix,aria}` and the legacy `new-york-v4` registry.
- `apps/v4/registry/styles/` supplies style tokens; `apps/v4/scripts/build-registry.mts` generates registry artifacts.
- Build the authored sources through the registry pipeline instead of editing compiled styles or installable JSON.
- `packages/react` publishes unstyled React components; `packages/helpers` publishes AI integration helpers.
- `packages/tests` owns CLI integration tests; colocated `*.test.ts` files cover package behavior.

## Key Directories

- `apps/v4/app/`, `components/`, and `content/docs/`: website routes, website components, and MDX documentation.
- `apps/v4/registry/`, `examples/`, `styles/`, and `public/r/`: authored registry data, demos, and generated artifacts.
- `packages/shadcn/src/`: CLI commands, preflights, migrations, presets, templates, and MCP integration.
- `packages/registry/src/`: registry APIs, schemas, resolvers, updaters, and source transformations.
- `packages/react/`, `packages/helpers/`, and `packages/tests/`: supporting packages and integration coverage.
- `templates/`: consumer application and monorepo scaffolds used by the CLI.
- `.changeset/` and `.github/workflows/`: release metadata and upstream automation.

## Development Commands

Run from the repository root unless a command explicitly selects a workspace.

- `pnpm install --frozen-lockfile`: install the checked-in dependency graph.
- `pnpm shadcn:build`: build the CLI and its workspace dependencies.
- `pnpm shadcn:dev`: watch the CLI and its workspace dependencies.
- `pnpm shadcn <command> -c <consumer-project>`: run the built CLI against the local registry server.
- `pnpm --filter=v4 registry:build --style all`: generate missing styles before starting a fresh website checkout.
- `pnpm --filter=v4 dev`: serve the website and local registry on port 4000.
- `pnpm registry:build`: run the full canonical registry build and its repository-owned formatting steps.
- `pnpm exec vitest run <test-file>`: run focused Vitest coverage from the root configuration.
- `pnpm typecheck`, `pnpm lint`, and `pnpm format:write`: existing repository-wide quality owners.
- `pnpm check`: run the upstream lint, typecheck, and formatting checks.
- `pnpm test`: build the registry, start the local website, and run the upstream test workflow.

## Code Conventions

- Follow `CONTRIBUTING.md`, `.editorconfig`, existing package configuration, and neighboring TypeScript or TSX code.
- Existing formatting uses two spaces, LF, double quotes, no semicolons, and ES5 trailing commas.
- Preserve package aliases, package exports, and source ownership; do not introduce a second installation implementation.
- Make component changes across affected bases and styles, update documentation, and regenerate through the owning pipeline.
- Read `apps/v4/registry/README.md` and the relevant base documentation before touching authored or generated registry files.
- Use Conventional Commits in the upstream `category(scope): message` shape.
- Preserve upstream contribution and license files; this checkout is an open-source fork, not a solo private product.

## Important Files

- `CONTRIBUTING.md`: development, component regeneration, tests, and contribution conventions.
- `package.json`, `pnpm-lock.yaml`, `pnpm-workspace.yaml`, and `.npmrc`: dependency and workspace authority.
- `turbo.json`, `vitest.config.ts`, and `vitest.workspace.ts`: task dependencies and test configuration.
- `prettier.config.cjs`, `.editorconfig`, and package-specific configuration: formatting and code conventions.
- `apps/v4/registry/README.md`: pipeline ownership and targeted versus canonical build modes.
- `.github/ISSUE_TEMPLATE/bug_report.yml`: affected components, reproduction, logs, system information, and prior research.
- `.github/ISSUE_TEMPLATE/feature_request.yml`: feature description, affected components, context, and prior research.
- No pull request template was present at initialization; recheck current upstream before drafting a submission.
- `.github/workflows/signed-commits.yml` and `signed-commits-close.yml`: unsigned-commit labeling and closure policy.
- `RELEASING.md` and `.changeset/config.json`: package publication and versioning; do not publish during fork setup.

## Runtime/Tooling Preferences

- Preserve the upstream `pnpm@10.33.4` package-manager contract and `pnpm-lock.yaml`; do not migrate this fork to Bun.
- The registry build invokes Bun internally, so a registry-development environment also needs Bun.
- `.nvmrc` contains `v20.5.1`, but the CLI and registry package engines require Node `>=20.18.1`.
- Treat that version mismatch as unresolved upstream configuration, not permission to downgrade below package engines.
- Before executing source workflows, choose a supported Node version satisfying every involved package and dependency.
- `pnpm shadcn` sets `REGISTRY_URL=http://localhost:4000/r` and a local `SHADCN_TEMPLATE_DIR`.
- `pnpm shadcn:prod` instead selects the public registry; keep reproductions explicit about their registry source.
- Preserve upstream automation output as inherited behavior; the canonical quiet-output rule governs new quality commands.

## Testing & QA

- Use the narrowest existing test or workspace check covering the changed behavior before broader required submission gates.
- Root Vitest excludes templates, fixtures, and the separate integration-test workspace.
- Integration and local-registry scenarios need built packages and the website served on port 4000.
- For component or registry changes, upstream requires the full `pnpm registry:build` before committing.
- Targeted registry builds may skip formatting and do not replace that canonical component-change gate.
- Run upstream-required checks and tests before submitting source contributions; include coverage for new features as requested.
- Verify changed component interactions in the actual browser, including relevant keyboard behavior and affected bases.
- Personal tracking-only changes require ledger validation, not a registry rebuild or a source test-suite run.
