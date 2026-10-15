# Unit 4: Test and Submit

- **Pull request:** <!-- paste the URL of your PR on codepath/pathreview-ai301-fa26-s3 -->
- **Branch:** `fix/53-parenthesized-phone-redaction`
- **Issue:** https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53

## Run history

1. Full run, first version of the skill: **20/20**, PASS, every
   category matched (clear-accept 7/7, silent-drift 4/4, not-tested
   4/4, unreviewable 3/3, standards-wall 2/2). One run was enough —
   `eval-run.txt` records it. No revision loop needed.

(Grading ran through Devin subagents with the same prompt and output
contract as `run_eval.py`, since `claude` is not installed on this
machine; the eval-run header records this honestly.)

## Package analysis

pkg-20 (ghostty-org/ghostty#13604): gold rejects, my rubric also
rejects. The diff is excellent — a bounded fix implementing the
maintainer-authored diagnosis with decisive mode-2031 before/after
evidence, clean hunks, regression tests. Every plan-fidelity,
test-evidence, and diff-clean check passes. But ghostty's stated
policy requires disclosure of all AI usage (tool and extent), and the
description carries none. `standards-met` fails it on that alone —
exactly the wall this category exists for, and the reason the check
reads "however good the diff is."

## Check rationale

`standards-met` reads: "The description fills every section the
template asks for, names the issue the way the template wants it
linked (e.g. `Closes #N`), and carries whatever disclosure the repo's
stated policy requires. Every PR package is AI-assisted work by
default — when the stated policy requires AI-use disclosure and the
description has none, this check FAILS however good the diff is."
It reads this way because the template and the policy are the repo's
stated definition of submittable — a PR that skips them is held no
matter how good the code is. The AI-assisted-by-default clause came
from my unit-3 rubric revision, where a grader waived disclosure
because AI use wasn't "proven"; the requirement lives in the repo's
policy, not in evidence about how the draft was written.

## Trade-offs

`plan-fidelity`'s both-directions check is strict: it holds a package
for claiming a CHANGELOG entry that isn't in the diff (pkg-12) just as
much as for shipping an unplanned flag (pkg-09). The cost is that it
can't tell harmless adjacent cleanup from real drift without a
deviation note — a student who does the right thing but forgets to
write it down gets held. That's deliberate: the check protects the
reviewer's trust, and a forgotten disclosure is exactly what the
Deviations heading exists for.
