# Evidence guide: where evidence lives in a plan package

The map between the rubric's check names and the parts of a package
that carry the evidence. In an eval bundle the sections are named;
live, the same evidence lives in the student's `plan.md`/`comment.md`
drafts, their posted repro comment, and the issue thread.

## Diagnosis and grounding

- Where it lives: the plan's diagnosis or stated cause (usually its
  first section), read against the bundle's repro-evidence block —
  live, the student's posted repro comment on the issue. The load-
  bearing facts are each step's output and every control run, because
  a control exists to rule a cause out.
- What good looks like: the stated cause cites behavior the repro
  evidence actually shows, and no control or step output already
  demonstrates the blamed component working. A cause the package's own
  evidence rules out fails even when the plan is confident and long.

## Scope

- Where it lives: the plan's scope statement, files list, and
  not-in-scope or deferred lines. In live mode, `plan.md`'s scope and
  files sections.
- What good looks like: every committed change traces back to what the
  issue asks — one bounded fix. Adjacent work appears only as an
  explicit deferral with a reason. A fix bundled inside a migration,
  redesign, or multi-front campaign fails however good the fix itself
  is.

## Executability

- Where it lives: the plan's files/areas and approach steps.
- What good looks like: a stranger could open the repo and start —
  named files or code areas, a concrete change described in order, and
  every decision already made in the plan. Deferred decisions
  ("investigate", "maybe", "whichever is easier") mean the plan is a
  wish list, not a procedure.

## Test plan

- Where it lives: the plan's test-plan section, mapped onto the repro
  evidence's steps and artifacts.
- What good looks like: the plan re-runs the repro's trigger (or an
  equivalent check through the real code path) and names the
  observable outcome proving the fix — the exit code, the output, the
  state expected after. "Run the suite", "should feel fast", and
  outcome-less checklists are not decisive.

## Honesty

- Where it lives: the plan's risks/unknowns/deviations statements, and
  the gap between what it asserts and what the package establishes.
- What good looks like: every fact the plan depends on but cannot show
  is named as an unknown with how it gets checked; nothing unverified
  is stated as settled. A mid-build change of course is honest only
  when recorded under the plan's Deviations heading.

## Comms

- Where it lives: the candidate plan comment, read against the thread
  highlights (live: the real issue thread) and the repo-facts block's
  template asks and contribution policy.
- What good looks like: the comment engages explicit maintainer
  direction (culprit pointers, posted test artifacts, requests for
  testing), engages related open PRs rather than silently racing them,
  answers what the repo's conventions ask, and carries whatever
  AI-use disclosure the stated policy requires — treat every package as
AI-assisted work, so a stated disclosure requirement is always in
force, not conditioned on proof of AI use. Ignoring a maintainer's
  explicit direction or a required disclosure fails regardless of how
  good the plan underneath is.
