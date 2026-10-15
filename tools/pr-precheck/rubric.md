# Rubric: is this pull request ready to submit?

My checks cover the four failure families a maintainer reads for:
silent drift (the diff does more or less than the posted plan, or the
description claims a fidelity the diff contradicts), not tested (the
evidence proves nothing observable or skips the repo's own checks),
unreviewable (debris buries the change), and the standards wall (the
repo's stated template and disclosure asks ignored). Fourth week
running, the honest-outcome rule holds: a shortfall disclosed in both
the plan's deviations and the description is honest work, not drift.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | The diff's changed files and hunks, read against the plan's scope/files/approach plus its Deviations notes; the description's fidelity claims ("as planned", "no other changes") read against the diff. | Every committed change is either in the plan or recorded under its Deviations, AND the description claims no more or less than the diff delivers. FAIL if the diff silently adds unplanned work (a new option, a refactor, unrelated hunks), silently drops planned work, or the description asserts fidelity the diff contradicts. A change recorded in Deviations and restated in the description passes. | required |
| test-evidence | The test evidence block, read against the plan's test plan and the repo's stated checks. | The evidence re-runs the plan's repro (or its named observable checks) with a visible before/after on the path the diff actually changes, AND shows the repo's own required checks run with their results. FAIL on "tests pass" with nothing named, evidence exercising an unchanged path, a planned test case never run, or repo checks absent. An honestly reported failing check with its reason counts as evidence; hiding one does not. | required |
| diff-clean | The diff's hunks and the commit list. | The diff contains only what the change needs: no debug prints, no commented-out experiments or first attempts, no dead code parked behind `allow(dead_code)` or a TODO, no unrelated hunks, no pure formatting/import churn. FAIL when any debris ships in the diff, even around a correct fix. | required |
| standards-met | The PR title and description, read against the repo-facts block's PR template and stated policy. | The description fills every section the template asks for, names the issue the way the template wants it linked (e.g. `Closes #N`), and carries whatever disclosure the repo's stated policy requires. Every PR package is AI-assisted work by default — when the stated policy requires AI-use disclosure and the description has none, this check FAILS however good the diff is. FAIL on any template ask visibly skipped or any stated requirement unmet. | required |
| regression-cover | The diff's test changes and the test evidence. | Beyond re-running the repro, the diff adds or updates a durable test (a new case, or unmarking an xfail/suppression) that would catch the bug returning. | preferred |
| thread-engaged | The description and commits, read against the thread highlights. | Where the issue's thread carries explicit maintainer direction or questions, the description or commits acknowledge it — a deferral cites the maintainer discussion, a question gets answered. Vacuous pass on an empty thread. | preferred |

## Verdict rule

Accept (ready to submit) if and only if every `required` check passes.
`preferred` checks never change the verdict. A required check graded
`unclear` counts as a fail: a PR you cannot verify from the package is
a PR that is not ready to submit.
