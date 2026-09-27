# Evidence guide: where proof lives in a reproduction package

The map between the rubric's check names and the parts of a package
that carry the proof. In an eval bundle the sections are named; in live
mode the same proof lives in the student's draft comments and on the
issue thread / repo docs.

## Environment

- Where it lives: the repro report's environment record — usually a
  line or short block near the top naming versions and platform. The
  issue's stated target lives in the issue body and thread highlights
  (versions confirmed affected, OS, driver, build profile); repo-facts
  may carry what the bug-report template asks for. Live mode: the
  student's draft env line against the issue thread.
- What good looks like: the software version under test and the
  OS/platform are named, plus any detail the issue turns on. If the
  attempt ran somewhere different from the issue's target, the report
  says so in words — a deviation that appears only in the artifact's
  fine print is silent and fails.

## Steps

- Where it lives: the repro report's steps/commands, read in order
  against the issue's described trigger. Live mode: the student's
  draft; the issue body holds the reference steps.
- What good looks like: a stranger could copy the sequence and hit the
  same trigger — starting state stated, every input file, config, and
  command present verbatim, nothing behind private access. The trigger
  itself must appear; a step list that skips or rewrites the trigger is
  unfollowable even if it is neat.

## Behavior shown

- Where it lives: the report's artifacts — output excerpts, logs,
  screenshots, test output — compared to the issue's expected-vs-actual.
- What good looks like: the artifact is the run's actual output, and it
  is the SAME behavior the issue describes (same symptom, error, or
  observable difference). For an honest cannot-reproduce, the artifact
  shows the real attempt and its non-triggering result. A graceful
  validation error where the issue reports a crash, a different exit
  code, or a version banner proving only that the tool runs are all
  wrong-target evidence.

## Honesty

- Where it lives: the report's conclusion and expected/actual lines,
  read strictly against its own artifacts; the claim comment's stated
  progress against what the package contains.
- What good looks like: the words claim no more than the artifacts
  show. "Cannot reproduce" with the differences named is a complete,
  honest outcome. Red flags: certainty adjectives doing the work of
  evidence ("definitely", "fully reproducible", "I verified"), a root
  cause asserted with nothing shown, or expected-vs-actual stated
  backwards from the artifact.

## Comms

- Where it lives: the candidate claim comment and the report's framing
  lines, checked against the repo-facts block — the "bug reports"
  template asks and the "contribution policy" line (which carries any
  AI-use policy). Live mode: the issue thread plus the repo's
  CONTRIBUTING.md, templates, and any AI_POLICY.md.
- What good looks like: the claim names this issue's specifics and a
  concrete next step — nothing interchangeable with any other issue's
  comment. The comments answer what the repo's template asks. When the
  stated policy requires disclosing AI assistance, the comments say the
  tool and the extent; silence or conditions-without-disclosure need
  nothing added.
