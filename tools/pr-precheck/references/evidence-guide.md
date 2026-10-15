# Evidence guide: where evidence lives in a PR package

The map between the rubric's check names and the parts of a package
that carry the evidence. In an eval bundle the sections are named;
live, the same evidence lives in the student's `plan.md`, the branch's
`git diff main...HEAD`, `test_evidence.md`, `pr_draft.md`, and the
issue thread and repo docs on GitHub.

## Plan fidelity

- Where it lives: in a bundle, the Plan context section vs the
  Candidate PR's Diff section and Description. Live: `plan.md`
  (including its Deviations heading — deviations are part of the plan)
  vs the committed branch diff and `pr_draft.md`'s title and
  description.
- What good looks like: every hunk in the diff is covered by the
  plan's scope/files/approach or named in Deviations; every fidelity
  claim in the description ("exactly as planned", "no functional
  changes outside X") survives a check against the hunk list, in both
  directions — nothing extra shipped, nothing promised quietly
  dropped. Drift disclosed in Deviations AND restated in the
  description is honest work, not a fail.

## Test evidence

- Where it lives: in a bundle, the Test evidence section read against
  the Plan context's test plan and the repo-facts' stated checks.
  Live: `test_evidence.md` read against `plan.md`'s test plan and the
  repo's PR template / contributing checks.
- What good looks like: every planned test step appears with its
  commands and outputs, a before/after pair exercises the path the
  diff actually changed (not a sibling path that was never broken),
  the repo's own checks are run with results shown, and any planned
  case that could not run is named with its reason rather than
  omitted. "Tests pass", "verified locally", and vibes are not
  evidence.

## Diff quality

- Where it lives: in a bundle, the Candidate PR's Diff and Commits
  sections. Live: `git diff main...HEAD` and the branch's commit
  list.
- What good looks like: the diff contains only the change — no debug
  prints, commented-out experiments or earlier attempts, dead code
  parked behind suppression attributes, stray TODOs, unrelated file
  hunks, or formatting/import churn. A correct fix wrapped in debris
  is still unreviewable: a maintainer should be able to read the diff
  end to end without triaging junk.

## Standards and comms

- Where it lives: in a bundle, the repo-facts block's `pull requests:`
  and `contribution policy:` lines, the Thread highlights, and the
  Candidate PR's Title and Description. Live: the repo's PR template
  file, contributing guide and stated AI policy, the issue thread,
  and `pr_draft.md`.
- What good looks like: every template section is filled, the issue
  is linked in the form the template wants (`Closes #N`), every
  stated ask is visibly met, and the description carries whatever
  disclosure the stated policy requires — every package is
  AI-assisted work, so a stated AI-disclosure requirement is always
  in force. Maintainer direction and questions in the thread are
  acknowledged or answered, and deferrals cite the discussion behind
  them.
