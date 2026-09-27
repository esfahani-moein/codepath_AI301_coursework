# Rubric: is this reproduction package ready to post?

My checks cover the proof families from lecture: the claim comment is
specific and honest, the environment is recorded, the steps are
followable by a stranger, the artifacts show the issue's actual behavior
(not an adjacent one), the outcome is stated honestly, and the comments
respect the repo's stated conventions including AI-disclosure policy.
references/evidence-guide.md says where each proof lives.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| claim-specific | The candidate claim comment, read against the issue context. | The claim names THIS issue's specifics (the symptom or behavior it describes) and states concrete intent — what the author will try or check next, with a promise to report back. FAIL on: a bare "+1"/me-too with no intent, boilerplate interchangeable across issues ("please assign this to me"), promising a fix or a date, or asserting results the package does not show. | required |
| environment-recorded | The repro report's environment record, against the issue's stated or confirmed target (version, OS, driver, build, config). | The report names the environment the attempt ran in: the software version under test and the OS/platform, plus any other detail the issue turns on (driver, build profile, backend). Any deviation from the issue's target must be explicitly acknowledged in the report. FAIL on: no environment record at all, or a silent deviation (e.g. testing an older version without saying so). | required |
| steps-followable | The repro report's steps, read against the issue's described trigger. | A stranger could re-run it: the steps go from a stated starting state through the trigger, every input/config/command is present verbatim, and nothing depends on private or unshared resources (private repos, unshared configs, local-only paths). FAIL if the trigger or a load-bearing precondition is missing or swapped. | required |
| behavior-matches | The repro report's artifacts (output excerpts, logs, test results), read against the issue's described behavior — its expected vs actual. | At least one artifact shows the run's real output, AND that output is the same behavior/symptom/error the issue describes — OR the report is an explicit cannot-reproduce whose artifacts show the genuine attempt and its non-triggering result. FAIL on: no artifact at all, artifacts showing only that the tool runs, or an artifact showing a different error/symptom (wrong target: a validation error narrated as the reported crash, a different exit code, an adjacent symptom). | required |
| honest-outcome | The report's conclusion and expected-vs-actual lines, read against its own artifacts; the claim comment's stated progress. | The stated outcome is exactly what the artifacts show: "reproduced" only when an artifact shows the issue's behavior; a cannot-reproduce is stated as such and names what differed or what the trigger likely needs. FAIL on: narrating behavior the artifacts do not show, asserting a root cause or certainty with nothing shown, or claiming more coverage than was run. | required |
| repo-conventions | The repo-facts block's "bug reports" template asks and "contribution policy" line, read against both comments. | The comments respect the repo's stated conventions: template asks are answered where they apply to a report of this kind, and if the stated policy requires disclosing AI assistance, the comments disclose it (tool and extent). A policy that is silent on AI, or permissive-with-conditions but no disclosure requirement, passes. FAIL on any stated requirement unmet. | required |
| control-run | The repro report for a comparison run: same setup minus the trigger, or a working variant. | A control run is present and isolates the trigger (e.g. passing case vs failing case), making the artifact attributable to the issue's mechanism rather than the environment. | preferred |
| next-step-named | The claim comment and the report's closing. | The package names a concrete next step for the issue — what will be investigated, fixed, or asked — rather than ending at confirmation. | preferred |

## Verdict rule

Accept (ready to post) if and only if every `required` check passes.
`preferred` checks never change the verdict; they only mark a stronger
package. A required check graded `unclear` counts as a fail: proof you
cannot verify is proof that is not ready to post. In a claim-only draft
(live mode), checks needing the repro report are graded `unclear` as
"not yet applicable" and left out of the verdict.
