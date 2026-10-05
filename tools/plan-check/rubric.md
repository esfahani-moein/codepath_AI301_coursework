# Rubric: is this plan ready to post and build from?

My checks cover the failure families from lecture: the diagnosis must
follow from the reproduced evidence (not contradict or ignore it), the
change must stay bounded to what the issue asks, a stranger must be
able to start executing, the test plan must name an observable outcome
that proves the fix, unknowns must be stated as unknowns rather than
dressed up as certainty, and the comment must engage what the thread
and the repo's stated conventions ask.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause, read against the repro-evidence block (symptoms shown, control runs, what the controls rule out). | The plan states a cause AND grounds it in quoted or referenced repro evidence, AND nothing in the repro evidence rules that cause out. FAIL if: the stated cause is contradicted by the package's own evidence (e.g. a control run already shows the blamed component behaves), the plan names no cause at all, or the cause is asserted with no repro evidence behind it. | required |
| scope-bounded | The plan's scope statement, files list, and any not-in-scope lines, read against what the issue asks. | The plan commits to ONE bounded change the issue asks for. FAIL if the plan bundles work the issue did not ask for: drive-by refactors, dependency/runtime migrations, UI or architecture redesigns, a fix wrapped inside a larger campaign. Naming adjacent work as explicitly out of scope or deferred (with a reason) is a pass, not creep. | required |
| executable | The plan's files/areas and approach steps. | A stranger could start without asking the author anything: the files or code areas to change are named, the approach is concrete (what gets changed, in what order), and no load-bearing decision is deferred to build time ("whichever is easier", "investigate", "maybe also"). FAIL on missing files/locations or an unchosen approach. | required |
| test-plan-decisive | The plan's test plan, read against the repro evidence's steps and artifacts. | The test plan re-runs the repro's trigger (or an equivalent check through the real code path) AND states the observable outcome that proves the fix — a specific output, exit code, state, or behavior expected after the change. FAIL on "run the test suite", "should feel fast", vibes, or no observable outcome named. | required |
| unknowns-honest | The plan's claims and its risks/unknowns/deviations statements. | The plan asserts only what the evidence supports: every uncertain fact it depends on is named as an unknown, with how it will be checked or why it does not block. A plan with nothing genuinely uncertain passes vacuously. FAIL when a real unknown (untested mechanism, unverified platform, guessed cause detail) is stated as settled fact. | required |
| thread-conventions | The candidate plan comment, read against the thread highlights (maintainer directions, posted artifacts, open PRs, explicit asks) and the repo-facts block (template asks, contribution policy incl. AI-use rules). | The comment engages what the thread and conventions ask: explicit maintainer direction is acknowledged and followed (or answered), related open PRs/prior art are engaged rather than silently raced, template asks are met, and any stated AI-disclosure requirement is satisfied in the comment. Every plan package is AI-assisted work by default — when the repo's stated policy requires AI-use disclosure and the comment carries none, this check FAILS regardless of how good the plan is. FAIL on ignored maintainer direction or an unmet stated requirement. | required |
| regression-cover | The plan's test plan for durable coverage. | Beyond re-running the repro, the plan adds or updates a test (unit/integration/regression case, or unmarking an xfail) that would catch this bug returning. | preferred |
| deferral-reasoned | The plan's out-of-scope or deferral lines. | Where the plan declines adjacent work, it says why (too risky, separate concern, maintainer direction) rather than silently omitting it. Vacuous pass when nothing adjacent exists to defer. | preferred |

## Verdict rule

Accept (ready to post and build from) if and only if every `required`
check passes. `preferred` checks never change the verdict. A required
check graded `unclear` counts as a fail: a plan you cannot verify from
the package is a plan that is not ready to build from.
