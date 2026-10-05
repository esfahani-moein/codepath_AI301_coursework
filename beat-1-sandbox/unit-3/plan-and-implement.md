# Unit 3: Plan and Implement

- **GitHub username:** <!-- your GitHub username -->
- **Issue:** https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53

## Plan comment

- **Link:** https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-6000254982


Posted text:

```markdown
I reproduced this on `main` @ `2f4e82f` (repro comment above): `(555) 123-4567` passes through unredacted and `detect` returns `[]`, while `555-123-4567` in the same string is redacted.

Diagnosis: the `phone_us` pattern's separator after `\)?` is `[-.]?` — it accepts a dash, a dot, or nothing, but not the space in `(555) 123-4567`.

Plan:

1. Widen the `[-.]?` separators in `phone_us` to accept a space in `safety/pii_scrubber.py`.
2. Remove the `xfail` markers on the four tests this issue names (`test_us_phone_number_redaction`, `test_us_phone_formats`, `test_detect_phone_pii`, `test_phone_at_start_of_text`) so they run for real. A fifth marker on `test_mixed_pii_and_text` also cites #53, but that test's phone is already dashed — it actually fails on an unrelated `street_address` over-match, so I'll leave its marker, correct the reason, and flag it here rather than fix an unfiled bug in this issue.
3. Re-run my repro snippet (expect both formats redacted, `detect` returns a `phone_us` hit) and the full test file (expect all named tests to pass with no regressions).

In scope only that regex and the test markers — not a broader rewrite of the phone patterns. One unknown I'm watching: a space separator could over-match non-phone digit runs, so I'll check the false-positive test and tighten rather than expand if it trips. Happy to adjust if maintainers prefer a different shape for the pattern.
```

## Branch

`fix/53-parenthesized-phone-redaction` on my fork of `codepath/pathreview-ai301-fa26-s3`.

## Evidence

Before (on `main` @ `2f4e82f`, from the posted unit-2 repro):

```
$ uv run --with structlog python -c 'from safety.pii_scrubber import PIIScrubber; s = PIIScrubber(); print(s.scrub("Call me at (555) 123-4567 or 555-123-4567")); print(s.detect("Call me at (555) 123-4567"))'
Call me at (555) 123-4567 or [REDACTED]
[]
$ uv run --with pytest python -m pytest tests/unit/test_pii_scrubber.py
20 passed, 5 xfailed
```

After (on `fix/53-parenthesized-phone-redaction`, same commands):

```
AFTER scrub: Call me at [REDACTED] or [REDACTED]
AFTER detect: [{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]
======================== 24 passed, 1 xfailed in 0.08s =========================
```

The one remaining xfail is `test_mixed_pii_and_text`, kept deliberately: it
fails on an unrelated `street_address` over-match (its marker was
mislabeled as #53), which is out of scope for this issue.

## Run history

1. Full run, first revision of the skill: **19/20** (pkg-20 graded
   accept, gold rejects — the comment carried no AI-use disclosure and
   ghostty's stated policy requires disclosing all AI usage).
2. Tightened `thread-conventions` so every package counts as
   AI-assisted work by default; re-graded pkg-20 alone: flipped to
   reject. Re-verified the rubric change could not flip any other
   package (no other package's repo facts state an AI-disclosure
   requirement).
3. Final full run: **20/20**, PASS, every category matched
   (clear-accept 7/7, scope-creep 4/4, wrong-cause 4/4, unbuildable
   3/3, thread-convention 2/2). `eval-run.txt` records this run.

## Package analysis

pkg-04 (junegunn/fzf#4260): gold rejects, my rubric also rejects. The
plan itself is bounded, grounded, and executable — a docs-only
workaround scoped away from the code fix. But the thread carries
explicit owner direction: the culprit file and lines were named
(`src/tui/light_windows.go:70-84`), a patched test binary was posted,
and the maintainer asked for testing while flagging the remaining
first-key problem needs a proper fix. The plan comment engages none of
that, so `thread-conventions` fails — exactly the check this category
exists to catch, since a reader following only the plan-quality checks
would have accepted it.

## Check rationale

`thread-conventions` reads: "The comment engages what the thread and
conventions ask: explicit maintainer direction is acknowledged and
followed (or answered), related open PRs/prior art are engaged rather
than silently raced, template asks are met, and any stated
AI-disclosure requirement is satisfied in the comment. Every plan
package is AI-assisted work by default — when the repo's stated policy
requires AI-use disclosure and the comment carries none, this check
FAILS regardless of how good the plan is." It reads this way because a
plan can be technically perfect and still wrong to post: the comment is
the maintainer-facing surface, and ignoring explicit direction or a
stated policy is a hold signal regardless of plan quality. The
AI-assisted-by-default sentence exists because the first run let a
grader decide AI use was "unproven" and waive the disclosure rule —
the requirement lives in the repo's policy, not in proof about how the
draft was written.

## Trade-offs

That same check costs some robustness: whether prior art was
"engaged" or "silently raced" is a judgment call when the thread's
analysis comes from a non-maintainer (pkg-07, pkg-11 were still
rejected on other checks, but a package could ride on that call
alone). It also means an excellent plan like pkg-20's gets held for a
one-line omission — strict by design, since convention violations are
what get contributions bounced upstream.
