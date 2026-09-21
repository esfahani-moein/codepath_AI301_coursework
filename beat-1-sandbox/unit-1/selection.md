# Unit 1: Issue Selection

## Chosen issue

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53
("PII scrubber fails to redact parenthesized US phone numbers")

## My skill's verdict on it (live mode)

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53",
  "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "archived: no; last default-branch commits dated 2026-09-16, within 180 days of today"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "last push to any branch 2026-09-16, within 180 days"},
    {"name": "scope-bounded", "grade": "pass", "evidence": "one bounded bug: extend phone regex in pii_scrubber.py to cover (555) 123-4567; repro snippet and four named failing tests given"},
    {"name": "issue-unclaimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; comments: 0"},
    {"name": "contribution-policy", "grade": "pass", "evidence": "no CONTRIBUTING.md in root or .github/ — no stated policy, which passes"},
    {"name": "maintainer-responsive", "grade": "pass", "evidence": "issue filed by Aburke225 (COLLABORATOR) — maintainer engagement on the issue"},
    {"name": "newcomer-signaled", "grade": "pass", "evidence": "labels include 'good first issue'"}
  ],
  "verdict": "accept"
}
```

I ran live mode on three candidates (#73 docs mismatch, #68 empty-index
ZeroDivisionError, #53 PII scrubber). All three were accepted; #53 ranked
first on fit because it is a bounded Python bug with a reproduce snippet
and four named failing tests — a complete reproduce → fix → test loop.

## Run history

1. First full run, all 20 scored issues, rubric v1: 19/20 agreement.
   Only disagreement: issue-20 (my rubric accepted, gold rejects).
2. Re-grade of issue-20 alone while revising: the problem was
   scope-bounded fail-condition (e), which required BOTH "no spec" AND
   "no maintainer endorsement". Issue-20's body states superficial
   expected behavior, so it passed the letter of the check while missing
   the point — a feature request nobody with a badge has touched hides an
   unmade product decision.
3. Tightened (e) to "a feature/enhancement request that no maintainer
   has endorsed or engaged with in the thread fails"; re-graded issue-20
   → reject. Verified the change cannot sink any clear-accept (every
   accepted item is a bug or docs task, or was filed by a maintainer).
4. Final full run with the revised rubric: 20/20 agreement, category
   floor met (claimed 4/4, clear-accept 8/8, dead-repo 3/3, policy 1/1,
   scope 4/4). That run is committed as eval-run.txt.

## Issue analysis

issue-20 (excalidraw/excalidraw#11811). Gold: reject. My rubric's final
verdict: reject — after one revision. v1 accepted it because the body
lists expected behavior ("place, resize, and move it like other
elements"), which satisfied my (e) condition as written. Reading the
bundle again, the tell is elsewhere: it was opened by cursor[bot], has
zero comments and zero labels, and says "Logo asset TBD". Nobody with a
maintainer badge has ever engaged with it, so whether a company-logo
shape belongs in the core toolbar at all is an unmade product decision —
exactly what the scope family exists to catch. The fix was making
maintainer endorsement the requirement for feature requests, not the
presence of spec-shaped words.

## Check rationale

My contribution-policy check exists because my contribution workflow is
AI-assisted, and it is the only check that rejects issue-12, an otherwise
perfect-looking issue. Its current wording: "No stated policy, or the
policy is silent on AI, or it sets conditions only (disclose AI use,
personally understand/test/review the change). FAIL only if the policy
bans AI-generated code or documentation outright." The conditions-versus-
ban distinction is the load-bearing part: conda's "AI welcome but review
it" and zulip's "understand and explain every change" pass, while
bookwyrm's "we do not accept AI-generated code or documentation" fails.

## Trade-offs

- The 180-day commit window for maintainer-alive could reject a slow but
  genuinely maintained repo (e.g. one maintainer who commits quarterly);
  I accepted that because a first contribution needs a reviewer, and
  stale repos are where PRs go to die.
- Requiring maintainer endorsement for feature requests will reject some
  legitimately good feature ideas a busy maintainer simply hasn't seen
  yet — I trade those away because guessing product direction is how
  first-time contributors burn a week on a rejected PR.
- Treating `unclear` as fail is deliberately conservative: it costs me
  some acceptable issues but never puts me on an unverifiable one.
- Preferred checks (maintainer-responsive, newcomer-signaled) only rank
  accepted issues, so a great unlabeled issue is never sunk by a missing
  label.
