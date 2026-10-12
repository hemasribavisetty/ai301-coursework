# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->


## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan_fidelity | Candidate plan's scope, approach, and Deviations read against the PR diff's changed files and behavior; PR description's claims about the implementation | Pass when every material change in the diff is covered by the plan or explicitly explained in Deviations, and the implementation addresses the planned issue. Fail when the diff silently adds unrelated functionality, omits a promised core change without disclosure, or contradicts the plan while claiming fidelity. A relevant, honestly documented deviation does not fail by itself. | required |
| description_matches_diff | Candidate PR title, summary, change list, and issue reference read against the actual diff and plan | Pass when the description accurately identifies the issue, describes the implemented behavior, and does not claim changes absent from the diff or hide material changes. An honestly disclosed limitation or unfinished item does not fail solely because the implementation is narrower than originally planned. | required |
| test_evidence | Candidate test-evidence block and PR description's testing claims read against the plan's test plan and original reproduction evidence | Pass when the evidence includes executed commands and observable results that directly address the original failure and demonstrate the implemented behavior. Before/after observations or equivalent decisive regression evidence must establish whether the targeted behavior changed. Fail when the PR merely promises testing, gives unsupported pass claims, or shows only unrelated tests. An honestly reported failing check is acceptable when its relevance and limitations are explained. | required |
| repository_checks | Candidate test evidence read against the repo-facts block's stated checks, contribution policy, and PR template | Pass when the repository-required checks have been run and their actual outcomes are recorded, including failures, skipped suites, or unavailable tests. Fail when required checks are omitted, falsely reported as passing, or replaced by unrelated commands without explanation. A disclosed failure or an empty test suite does not automatically fail if the evidence accurately reports what happened. | required |
| diff_reviewability | PR diff, changed-file list, and commit contents read against the planned scope and issue | Pass when the diff contains only issue-relevant implementation, tests, and necessary supporting changes, with no debug statements, accidentally committed local files, commented-out experiments, generated debris, or unrelated refactors that obscure review. Fail when unrelated or disposable changes remain in the submitted diff. | required |
| standards_and_disclosure | Candidate PR title and description read against the repo-facts contribution policy, PR template, issue thread, and AI-use disclosure requirements | Pass when all explicitly required PR-template content and repository contribution conditions are satisfied, including AI-use disclosure when required. Fail when a mandatory section, issue reference, required disclosure, or stated policy condition is missing. A concise answer, N/A explanation, or honestly disclosed shortfall is acceptable when the repository permits it. | required |
| reviewer_clarity | Candidate PR description and diff read together | Pass when a reviewer can identify the problem, changed behavior, affected files, and test outcomes without reconstructing the submission from unrelated material. Otherwise fail. | preferred |

## Verdict rule

Return `accept` only when every required check passes. A `fail` on any required check returns `reject`. An `unclear` grade on a required check also returns `reject`, because the available package does not establish readiness.

Preferred checks provide feedback and may help explain quality differences, but never change the final binary verdict.

Grade all checks even when an earlier required check fails. Use only the available evidence and the pass conditions above. Do not reject a PR merely because a relevant deviation, limitation, test failure, or unavailable test is honestly documented; determine whether the evidence is sufficient and whether repository requirements were respected.
