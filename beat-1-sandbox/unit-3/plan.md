# Plan: PII scrubber fails to redact parenthesized US phone numbers

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53
Commit: `2f4e82f` on `main`

## Diagnosis

The `phone_us` pattern in `safety/pii_scrubber.py` (line 16) is:

```
r"\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b"
```

After the closing parenthesis `\)?`, the separator class `[-.]?` accepts a
dash, a dot, or nothing — but not a space. So `(555) 123-4567` never
matches: the space after `)` breaks the pattern. Dashed forms like
`555-123-4567` match because `-` is in the class.

My repro on `main` @ `2f4e82f` confirmed exactly this:

- `scrub("Call me at (555) 123-4567 or 555-123-4567")` →
  `"Call me at (555) 123-4567 or [REDACTED]"` — the parenthesized number
  passes through unredacted while the dashed control is redacted.
- `detect("Call me at (555) 123-4567")` → `[]`.
- `pytest tests/unit/test_pii_scrubber.py`: 20 passed, 5 xfailed — the
  five tests marked `xfail` with reason "issue #53".

## Scope

In scope: widening the separator after `\)?` in `phone_us` so a space (or
`-`/`.`) is accepted, and removing the five `xfail` markers citing
issue #53 in `tests/unit/test_pii_scrubber.py` so they run for real.

Not in scope: rewriting the phone patterns wholesale, international
formats beyond `phone_intl`'s existing coverage, or the unused
`pii_type` variable the file itself flags. `(555)123-4567` (no space)
already matches; this change does not touch other formats.

## Files

- `safety/pii_scrubber.py` — the `phone_us` regex, one separator class.
- `tests/unit/test_pii_scrubber.py` — remove the five
  `@pytest.mark.xfail` decorators whose reason cites issue #53.

## Approach

1. In `phone_us`, change the separator after `\)?` from `[-.]?` to
   `[-. ]?` so `(555) 123-4567` matches.
2. Run the repro snippet again and confirm both phone formats redact.
3. Remove the five `xfail` markers citing #53 in
   `tests/unit/test_pii_scrubber.py`
   (`test_us_phone_number_redaction`, `test_us_phone_formats`,
   `test_detect_phone_pii`, `test_phone_at_start_of_text`,
   `test_mixed_pii_and_text`).
4. Run the full test file and confirm the formerly-xfailed tests pass
   with no regressions.

## Test plan

Re-run my week-2 repro steps on the fix:

- `uv run --with structlog python` snippet calling `scrub` on
  `"Call me at (555) 123-4567 or 555-123-4567"` — after the fix both
  numbers must show `[REDACTED]`, and `detect` on the parenthesized form
  must return one `phone_us` hit.
- `uv run --with pytest pytest tests/unit/test_pii_scrubber.py` —
  expect all 25 tests to run with 0 failures and 0 xfails (the five
  markers removed).

## Risks and unknowns

- Widening the separator to include a space could over-match digit
  runs that are not phone numbers (e.g. "call 202 555-0100 today" where
  "202" is a year fragment). The pattern's `\b` boundary plus the
  optional `\(?` limits this, but I will check the
  `test_detect_no_false_positives` test and eyeball real cases; if a
  false positive appears I will tighten to `[-.]\? ?` alternatives
  rather than expanding scope.
- Whether `(555)-123-4567` style variants are covered is already handled
  by `[-. ]?`; formats like `555 123 4567` (all spaces) remain unmatched
  — out of scope unless the tests say otherwise.

## Deviations

Three changes from the plan as posted, found while building:

1. **All three separators widened, not one.** The plan scoped the change
   to the separator after `\)?`, but `test_us_phone_formats` also
   requires `"+1 555 123 4567"` to redact, which needs a space accepted
   after the `+1` prefix and between every group. All three `[-.]?`
   separator classes became `[-. ]?` — still one regex, same scope.
2. **Leading `\b` replaced with `(?<!\d)`.** A first pass with `[-. ]?`
   alone redacted `(555) 123-4567` as `([REDACTED]` — `\b` cannot
   anchor before a non-word `(`, so the match started inside at `555`
   and left the paren behind. A `(?<!\d)` lookbehind lets the optional
   `\(?` join the match while still blocking mid-number matches.
3. **Only four xfail markers removed, not five.** The issue body names
   four tests; the fifth marker on `test_mixed_pii_and_text` cites #53
   but the test actually fails on an unrelated `street_address`
   over-match (`5 years developing Python appl` gets redacted — its
   two-letter suffix alternatives match mid-word). Removing that marker
   would expose a different bug under a false reason, so the marker
   stays with its reason corrected to describe the real failure. That
   bug appears unfiled; I'll flag it on the issue thread.

