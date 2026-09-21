# Rubric: is this a good first issue?

My checks cover the four families from lecture (maintainer alive, repo in
use, scope fits, nobody else on it) plus the fifth surface from the
evidence guide: the repo's contribution policy on AI-assisted work, since
my workflow is AI-assisted. Thresholds are measured against the bundle's
capture date in eval mode, against today in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo-facts block: "archived:" flag on the repo line, the "last 5 default-branch commits" dates, and the "maintainer first-response sample". Live mode: repo front page commit list and recently updated issues. | The repo is NOT archived, AND at least one of the last 5 default-branch commits is dated within 180 days of the capture date, OR the first-response sample shows an owner/member/collaborator reply within 60 days of the capture date. | required |
| repo-in-use | Repo-facts block: "latest release" and "last push to any branch" lines. Live mode: Releases box and Branches page. | At least one of: a latest release dated within 24 months of the capture date, OR a push to any branch within 180 days of the capture date. A repo that only ever shipped code once and went quiet fails. | required |
| scope-bounded | Issue body and comment thread; for abandonment history also the "linked PRs" line in repo facts. | The issue asks for ONE bounded change: a specific bug, a defined docs task, or a small feature whose expected behavior is stated or maintainer-confirmed in the thread. FAIL if any of: (a) the issue is an umbrella/tracking/meta issue or an open-ended codebase-wide effort (a list of sub-items, "add X across the codebase"); (b) the thread shows the design or spec still under debate with no maintainer-settled decision; (c) two or more closed-unmerged linked PRs show abandoned attempts; (d) the issue is a pure usage/support question; (e) it is a feature/enhancement request that no maintainer (owner/member/collaborator, including as the issue's author) has endorsed or engaged with in the thread — an unendorsed wish hides an unmade product decision; bug reports and docs tasks do not need this. | required |
| issue-unclaimed | Repo-facts block: "this issue: assignees:" and "linked PRs:" lines, plus the comment thread for claim comments ("I'll take this", "working on this", "can I work on this", bot claim commands). Live mode: Assignees box, Development box, and thread. | All of: assignees is "none"; no linked PR in state "open" (linked PRs from any fork count); and no claim comment within 180 days of the capture date that still stands. A claim comment older than 1 year with no resulting open PR is stale and passes; a claim a maintainer explicitly released or re-invited takers on passes. | required |
| contribution-policy | Repo-facts block: the "contribution policy" line (CONTRIBUTING.md and linked docs). Live mode: CONTRIBUTING.md in root or .github/, plus AI_POLICY.md / AGENTS.md if present. | No stated policy, or the policy is silent on AI, or it sets conditions only (disclose AI use, personally understand/test/review the change). FAIL only if the policy bans AI-generated code or documentation outright. | required |
| maintainer-responsive | Repo-facts block: "maintainer first-response sample"; this issue's comment thread for owner/member/collaborator comments. | Any first response of 30 days or less in the sample, or a maintainer (MEMBER/OWNER/COLLABORATOR association) has commented on this issue. | preferred |
| newcomer-signaled | Issue labels in the issue header; the comment thread. | The issue carries a newcomer label (e.g. "good first issue", "help wanted", "easy", "Easy to Fix", "beginner") or a maintainer explicitly invites takers in the thread. | preferred |

## Verdict rule

Accept if and only if every `required` check passes. `preferred` checks
never change the verdict; they only rank accepted issues against each
other (more preferred passes = better fit). A required check graded
`unclear` counts as a fail: a first issue I cannot verify is not a first
issue I should take.
