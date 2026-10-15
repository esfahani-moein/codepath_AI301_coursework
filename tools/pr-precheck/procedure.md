# Procedure: how this skill grades a PR package

## Read order

1. Read the repo-facts block first. Note the PR template's asks
   (required sections, issue-linking form, checklist items) and the
   stated contribution policy — especially any AI-disclosure
   requirement. The standards check is graded against these later.
2. Read the issue and the thread highlights: what behavior is wrong,
   and what the thread settled or asked — maintainer direction,
   questions the PR should answer.
3. Read the plan context. Note its stated scope, named files, test
   plan, and every Deviations entry — Deviations are part of the plan.
   This is the reference the diff must match.
4. Read the candidate PR's diff next, before its description. List
   every file it touches and what each hunk does. The diff is the
   ground truth of what ships; read it before the prose that describes
   it so the description can be checked against it, not believed.
5. Read the test evidence block: each command, its output, and which
   path it exercises.
6. Read the title, description, and commit list last — they are
   graded against everything gathered above.

## Evidence gathering

1. plan-fidelity: list every changed file and each hunk's effect from
   the diff. For each, record whether the plan or a Deviations entry
   covers it. Then extract every fidelity claim in the description
   ("as planned", "no changes beyond X", "docs now do Y") and check
   each against the hunk list — in both directions: unplanned work in
   the diff, and planned work absent from it.
2. test-evidence: extract each planned test step from the plan's test
   plan, then match it against the evidence block: is the step run,
   on the path the diff changes, with an observable before/after
   (output, exit code, state)? Separately, list the repo's stated
   checks and whether each appears in the evidence with its result.
3. diff-clean: scan the hunk list gathered in step 1 for anything the
   change does not need: debug prints, commented-out code, dead
   functions, stray TODOs, hunks in files outside the change's reach,
   re-indent or import churn.
4. standards-met: extract each template ask and policy item from the
   repo facts, then check the description against each: every section
   filled, the issue linked in the form the template wants, required
   checklist items present, required disclosures made.
5. Preferred checks: regression-cover — look in the diff for a test
   case that exercises the fixed behavior. thread-engaged — match
   each maintainer direction or question from the thread against the
   description and commits.

## Check execution

1. Grade the checks in table order, one at a time, using only the
   facts gathered for that check. Quote the deciding line or hunk for
   every grade.
2. Grade `unclear` only when the evidence a check names is genuinely
   absent from the package — not when it was merely tedious to
   compare. A missing artifact the check reads (no test evidence, no
   description section) is a `fail` per the pass condition, not an
   `unclear`.
3. Do not re-read the package for a check whose evidence is already
   gathered; pull exactly the piece needed when it is not.
4. Never grade prose quality, length, or confidence. A disclosed
   shortfall is honest work: what the description admits, plan
   fidelity does not punish — grade only what stays silent.

## Verdict assembly

1. Apply the verdict rule in rubric.md: accept iff every required
   check passes; a required `unclear` counts as a fail; preferred
   checks never enter the verdict.
2. On a reject, the summary names each failing required check with
   its deciding evidence; on an accept, one line per check.
3. Emit the fenced JSON block last — item id, per-check name, grade,
   one-line evidence, verdict — and nothing after it.
