# Unit 2: Claim and Reproduce

- GitHub username: esfahani-moein github
- Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53
  ("PII scrubber fails to redact parenthesized US phone numbers")

## Claim comment

Link: [Github](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5858419288)

Posted text:

> Hi! I'd like to work on this issue as my first contribution to PathReview.
>
> The report describes `(555) 123-4567` passing through `PIIScrubber.scrub()`
> unredacted while the dashed `555-123-4567` form is caught, with the gap
> covered by four xfail-marked tests in `tests/unit/test_pii_scrubber.py`.
> My next step is to set up the repo, reproduce the behavior on current
> main, and post a repro report here with my environment and the output I
> get. I'll report back either way.

## Repro comment
 
Link: [Github](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5858436507)


Posted text:

> Reproduced on current main — the parenthesized US phone format passes
> through the scrubber unredacted, matching this issue.
>
> **Environment:** macOS 26 (Darwin 25.6.0, arm64), Python 3.14.7, repo at
> commit `2f4e82f` on `main` (cloned 2026-09-27). Only dependency needed
> for the repro is `structlog`, run via `uv run --with structlog`.
>
> **Steps** (from the repo root, with the repo on `sys.path`):
>
> ```python
> from safety.pii_scrubber import PIIScrubber
> s = PIIScrubber()
> print(s.scrub('Call me at (555) 123-4567 or 555-123-4567'))
> print(s.detect('Call me at (555) 123-4567'))
> ```
>
> **Output:**
>
> ```
> Call me at (555) 123-4567 or [REDACTED]
> []
> ```
>
> **Expected:** both phone numbers redacted; `detect()` returns a phone
> detection for the parenthesized number.
>
> **Actual:** the parenthesized `(555) 123-4567` is returned unchanged and
> `detect()` reports no PII for it, while the dashed `555-123-4567` in the
> same string is redacted (control). Same behavior the issue describes.
>
> The covering tests tell the same story: `pytest
> tests/unit/test_pii_scrubber.py` on `main` gives 20 passed, 5 xfailed —
> the four tests named in this issue plus `test_mixed_pii_and_text`, all
> `xfail(strict=True)` citing it.
>
> Looking at `safety/pii_scrubber.py`, the `phone_us` pattern
> `\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b`
> requires the post-`)` separator to be `-` or `.` — a space doesn't match,
> so `(555) 123-4567` fails after the area-code group. Next step: widen the
> separator to accept a space after `)`, and drop the xfail markers from
> the covering tests in the same PR per the contributing guide.

## Run history

1. One full run, all 20 scored packages, rubric + evidence guide as
   written: 20/20 agreement, category floor met (clear-accept 8/8,
   no-evidence 4/4, wrong-target 4/4, unfollowable-comms 3/3,
   disclosure 1/1). No revise loop was needed; the committed
   eval-run.txt is that run.

## Package analysis

pkg-20 (ghostty-org/ghostty#13604). Gold: reject. My verdict: reject.
Every proof check passed — the claim named the mode-2031 symptom, the
environment was recorded with the release-build deviation acknowledged,
the steps were verbatim and re-runnable, and the artifact showed the
exact wrong response (`997;2` under a dark scheme) with a
conditional-pair control. It still failed, on repo-conventions:
ghostty's stated policy requires disclosing all AI usage with the tool
and extent, and neither comment disclosed anything. It is the set's only
disclosure-wall package — exactly the category the floor exists to
protect, and the reason conventions is a required check rather than a
nicety.

## Check rationale

behavior-matches, as it now reads in my rubric.md: "At least one
artifact shows the run's real output, AND that output is the same
behavior/symptom/error the issue describes — OR the report is an
explicit cannot-reproduce whose artifacts show the genuine attempt and
its non-triggering result. FAIL on: no artifact at all, artifacts
showing only that the tool runs, or an artifact showing a different
error/symptom." The second clause is load-bearing twice over: it rejects
pkg-02's graceful validation error narrated as the reported panic, and
it lets pkg-09 and pkg-10's honest cannot-reproduce reports pass —
judging the artifact against the issue, never the write-up's confidence.

## Trade-offs

- claim-specific rejects terse-but-honest claims; I accepted that
  because a claim with no specifics tells the maintainer nothing and
  reads as a parking attempt.
- environment-recorded treats an unacknowledged deviation as a fail,
  which costs packages that tested a different version in good faith —
  but a silent deviation is how wrong-target results sneak in, so naming
  it is the minimum bar.
- `unclear` counts as fail: conservative, and it means a partial package
  is never waved through on the strength of the half that is shown.
- Preferred checks (control-run, next-step-named) reward stronger
  packages without gating a faithful minimal one — a cannot-reproduce
  has no control to run and should not need one.
