# Issue and Pull Request Tracking

Read this index at the start of every agent session before repository work.
`FORMAT.md` owns research, lifecycle, drafting, implementation, and publication rules.
Each linked `issues/ISSUE-NNN.md` is the complete authoritative record for one root cause.
This file owns `Next finding ID` and projects current issue-file state.
`Next-Action` is the 2–6 word `Next-Action/Summary` projection from the issue record.
When a row disagrees with its issue file, correct the row from the issue file in the same task.

Next finding ID: ISSUE-007

## Open-Findings

| ID | Finding | State | Authorized-Work | Publication-Target | Contribution-Priority | Next-Action | External-Reference |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [ISSUE-001](issues/ISSUE-001.md) | registry: empty-string target bypasses path validation | Investigating | Not-Selected | Not-Selected | High | Reproduce write escape end-to-end | Not published. |
| [ISSUE-002](issues/ISSUE-002.md) | registry: css at-rules load build-executed JavaScript | Investigating | Not-Selected | Not-Selected | High | Verify Tailwind plugin execution locally | Not published. |
| [ISSUE-003](issues/ISSUE-003.md) | chart: ChartStyle interpolates config into a style element unencoded | Investigating | Not-Selected | Not-Selected | High | Verify SSR style breakout | Not published. |
| [ISSUE-004](issues/ISSUE-004.md) | registry: envVars values are written unescaped and without confirmation | Investigating | Not-Selected | Not-Selected | Medium | Confirm env newline injection | Not published. |
| [ISSUE-005](issues/ISSUE-005.md) | registry: middleware rename overwrites an existing proxy.ts unprompted | Investigating | Not-Selected | Not-Selected | High | Reproduce proxy overwrite silently | Not published. |
| [ISSUE-006](issues/ISSUE-006.md) | registry: cssVars values are written into the stylesheet unescaped | Investigating | Not-Selected | Not-Selected | High | Confirm cssVars declaration injection | Not published. |

## Archived-Findings

| ID | Finding | Authorized-Work | Publication-Target | Contribution-Priority | Archive-Reason | External-Reference |
| --- | --- | --- | --- | --- | --- | --- |
