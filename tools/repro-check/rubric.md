# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment_recorded | Repro report environment record, read against the issue context and repo-facts block | Pass when the report identifies the material environment needed to interpret the result, including the relevant OS/platform, runtime or language version, and repository revision/branch when those can affect reproduction. If the reported environment materially differs from the issue or repository target, the difference must be stated. If the available record is too incomplete to tell what environment produced the evidence, mark unclear. | required |
| steps_followable | Repro report's starting state and reproduction steps, read against the issue description and repository setup expectations | Pass when another contributor could start from the stated environment, perform the described commands/actions in order, and reach the behavior being tested without inventing a missing operation or prerequisite. Fail when a necessary trigger, setup action, input, or starting state is omitted. | required |
| behavior_matches_issue | Repro report artifacts such as output excerpts, logs, tracebacks, screenshots, or test results, compared directly with the behavior described in the issue | Pass when the evidence demonstrates the specific behavior named by the issue, or directly demonstrates that the reported failure did not occur under the stated reproduction attempt. Fail when the evidence shows only an adjacent error, unrelated failure, or behavior from the same component that does not establish the issue being investigated. | required |
| outcome_honest | Repro report's stated outcome compared with its steps, observations, and artifacts | Pass when the conclusion says no more than the evidence establishes. A reproduced outcome requires evidence of the issue's behavior. A cannot-reproduce outcome passes when the attempted conditions are recorded and the evidence shows the reported behavior did not occur; uncertainty or material environment differences must be disclosed. Fail when the report confidently claims reproduction or non-reproduction beyond what its evidence supports. | required |
| repo_conventions | Claim comment and repro comment, read against the issue context, repo-facts contribution policy, bug-report/template requirements, and any AI-use disclosure requirement | Pass when the comments comply with explicit repository requirements. A claim must identify the specific issue and promise an investigation/report without asserting reproduction before it happened or promising a fix/date. A repro comment must state what was actually attempted and observed. If the repository explicitly requires AI-use disclosure, the applicable comment/package must include it; if the repository is silent on AI disclosure, silence passes. | required |
| issue_specificity | Claim and repro comments read against the issue body | Pass when the language contains issue-specific details such as the affected behavior, component, input, error, or intended investigation, rather than generic boilerplate that could be posted on any issue. Otherwise fail. | preferred |

## Verdict rule

Accept a full reproduction package only when every required check passes. A fail on any required check rejects the package. `unclear` on a required check counts as fail because the package is not ready to post until the missing evidence is resolved. Preferred checks never change the accept/reject verdict and are used only as additional quality guidance.


