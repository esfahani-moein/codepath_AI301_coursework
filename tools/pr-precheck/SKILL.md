---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: **is this
pull request ready to submit?** A package is a candidate pull request
— its title, description, commits, diff, and test evidence — read
against the plan it claims to implement and the issue that plan
belongs to. You do not grade the code's cleverness, the author's
enthusiasm, or the write-up's polish: you grade whether this package
is faithful to its plan, decisively evidenced, clean to review, and
compliant with the repo's stated asks. You never answer a different
question and never grade more than one package per run.

## Inputs and modes

Run in exactly one of two modes:

- **Live mode**: the student's own submission before it goes out.
  Inputs: their `plan.md` (deviation notes included — graders read
  the diff against it), the branch's diff, the draft PR title and
  description (`pr_draft.md`), the test evidence (`test_evidence.md`),
  and the issue the plan belongs to. The branch's diff is every
  committed change relative to the default branch: `git diff
  main...HEAD` (three dots) run from the working copy produces it.
  Gather issue-side evidence live — the issue thread, the repo's PR
  template, and its stated contribution/AI policy. A student on the
  house chain reads the house plan and house repro pack in place of
  their own `plan.md` and repro; the same checks grade the same
  things.
- **Eval mode**: a package bundle is the whole world. The bundle
  carries the repo facts, the issue, the thread highlights, the plan
  context, and the candidate PR (title, description, commits, diff,
  test evidence). Fetch nothing; read nothing outside the bundle.
  Eval mode always grades a complete package: every check, the full
  verdict rule.

## The scope seam (live mode only)

Read `scope.md` before anything else in live mode. It names the repo
the student's PR must target and the house rules that apply there;
apply those rules when reading evidence. Refuse to grade a package
whose PR targets a different repository. If the scope's `Repo:` line
still carries an unfilled placeholder, stop without grading and tell
the student to fill it with their section's Path Review repo; never
guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and hold the outgoing text — the
PR title and description — against its rules. Report every rule the
draft breaks in the summary, quoting the rule. The voice guide never
changes the verdict on its own unless the rubric has a check that
reads it. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

The pieces connect like this: `rubric.md` defines the checks (name,
evidence source, pass condition, weight) and the verdict rule;
`references/evidence-guide.md` maps where each evidence family lives
in a package; `procedure.md` is the operating procedure — execute it
as written, in order, without improvising. Where the procedure is
silent on a step, report the gap in your summary instead of inventing
a step. If `rubric.md` has no checks filled in, or `procedure.md` has
no steps filled in, refuse to grade and say so: a tool that invents
checks at runtime produces noise, not judgment.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject`
(hold). There is no third verdict and no score — reservations belong
in check evidence lines, not the verdict. Emit a short readable
per-check summary first if you like, then end your reply with this
fenced JSON block, valid and last, with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse faithful PR can be ready
  and a beautiful confident one can be hiding drift. Every check
  reads the artifact itself against the plan, the issue, and the
  stated standards — never the formatting or the confidence of the
  prose.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension; the
  fix belongs in the rubric, not the run.
- The procedure decides how, not you: follow `procedure.md` as
  written; report its gaps instead of papering over them.
- Unclear defaults to fail: treat `unclear` as the rubric's verdict
  rule directs; where the rule is silent, an unverifiable claim is a
  failing one. A PR you cannot verify from the package is a PR that
  is not ready to submit.
