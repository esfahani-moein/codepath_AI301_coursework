# Procedure: how this skill grades a plan package

## Read order

1. Read the repo-facts block first. Note the repo's stated bug-report
   asks and its contribution policy — especially any AI-use disclosure
   requirement. The comms check is graded against these later.
2. Read the issue context: what behavior is wrong, and what the thread
   highlights settled — maintainer diagnoses, posted binaries or
   patches, explicit requests, and open PRs touching the same ground.
3. Read the repro-evidence block. Note exactly which behavior the
   evidence pins down: each step's output, every control run, and what
   each control rules out. This block is the ground truth the plan's
   diagnosis must answer to.
4. Read the candidate plan. Note its stated cause, scope and
   not-in-scope lines, named files, approach order, test plan, and any
   risks or unknowns.
5. Read the candidate plan comment last: it is graded against the
   thread and conventions gathered in steps 1-2, so it must be read
   with those still fresh. Order matters because every check compares
   a candidate artifact against reference evidence — read the
   references before the candidates, never the reverse.

## Evidence gathering

1. diagnosis-grounded: extract the plan's stated cause verbatim, then
   each repro-evidence fact that bears on it — the symptom shown and
   every control's result. Record whether any shown fact rules the
   stated cause out (a control where the blamed component behaves).
2. scope-bounded: extract every distinct change the plan commits to
   (each file, each subsystem, each named task) and every explicit
   not-in-scope/deferred line. Compare the committed list against what
   the issue asks for.
3. executable: extract the named files or code areas and the approach
   steps. For each step, record whether the decision is already made
   in the plan or deferred ("investigate", "maybe", "whichever is
   easier").
4. test-plan-decisive: extract the test steps and their stated
   expected outcome. Check the steps traverse the repro's trigger (or
   an equivalent path) and the expected outcome is observable: an
   output, exit code, state, or behavior — not an adjective.
5. unknowns-honest: list facts the plan depends on that the package
   does not establish (untested mechanisms, other platforms, guessed
   internals). For each, record whether the plan names it as an
   unknown or asserts it as settled.
6. thread-conventions: extract every maintainer signal in the thread
   highlights — directions, posted artifacts, explicit asks, related
   open PRs — plus the repo-facts template asks and contribution/AI
   policy. Check the plan comment against each: engaged, answered, or
   ignored; disclosed if required.

## Check execution

1. Grade the checks in table order, one at a time, using only the
   facts gathered for that check. Quote the deciding line or fact for
   every grade.
2. Grade `unclear` only when the evidence a check names is genuinely
   absent from the package — not when it was merely hard to find. If
   the plan omits the thing a check reads (no stated cause, no test
   plan), that is a `fail` per the pass condition, not an `unclear`.
3. Do not re-read the package for a check whose evidence was already
   gathered; do pull exactly the piece needed when a check's evidence
   was not yet collected.
4. Never grade formatting, length, or confidence. Grade what the text
   says against the reference evidence.

## Verdict assembly

1. Apply the verdict rule in rubric.md: accept iff every required
   check passes; a required `unclear` counts as a fail; preferred
   checks never enter the verdict.
2. On a reject, the summary names each failing required check with its
   deciding evidence; on an accept, one line per check.
3. Emit the fenced JSON block last — item id, per-check name, grade,
   one-line evidence, verdict — and nothing after it.
