# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer_alive | Repo facts: last 5 default-branch commits and maintainer first-response sample; issue Comments section `author_association` | Pass if at least one of the last 5 default-branch commits is by a non-bot maintainer within 90 days of the bundle capture date, OR the maintainer first-response sample shows an Owner, Member, or Collaborator response within 30 days. A bot commit alone does not satisfy this check. If neither dated signal is available, mark unclear. | required |
| repo_in_use | Repo facts: `archived`, latest release, and last push to any branch | Fail if `archived: true`. Otherwise pass if the latest release OR last push to any branch occurred within 180 days of the bundle capture date. If the repository is not archived but neither dated activity signal is available, mark unclear. | required |
| newcomer_scope | Issue body and Comments section | Pass when the issue asks for one bounded contribution. A numbered list of diagnostic causes, implementation clues, or steps for resolving one reported behavior does not by itself make an issue an umbrella. Fail if the issue is explicitly an umbrella/tracking issue whose sub-items are independently shippable contributions, a pure usage/support question, the thread shows unresolved design debate with no maintainer decision, or a maintainer states that the solution requires changes to core internals. Also fail an underspecified feature request when the issue requires a product or design decision but provides no concrete expected behavior, acceptance criterion, maintainer direction, or implementation target. A short issue body or missing reproduction steps alone does not fail this check when the requested change is otherwise bounded and actionable. | required |
| ownership_available | Repo facts: `this issue: assignees` and `linked PRs`; issue Comments section for claim statements and PR mentions | Pass when there is no assignee, no open linked or mentioned PR implementing the issue, and no still-active claim in the thread. A closed unmerged PR is an abandoned attempt and does not fail this check. Historical claim comments alone do not fail when later thread evidence shows the attempt was abandoned. If current ownership cannot be determined, mark unclear. | required |
| contribution_policy | Repo facts: `contribution policy`, including CONTRIBUTING/AI policy summaries and template requirements | Fail only if the policy explicitly bans AI-generated or AI-assisted contributions. Pass if AI-assisted work is allowed with conditions such as disclosure, testing, personal understanding, or human review; also pass if the available policy evidence is silent about AI use. | required |
| issue_starting_point | Issue body and Comments section | Pass if the issue provides at least one concrete expected behavior, reproduction step, failing test, error message, example input/output pair, acceptance criterion, or named code location. Otherwise fail. | preferred |

## Verdict rule

Accept an issue only when every required check passes. A fail on any required check rejects the issue. `unclear` on a required check counts as fail. Preferred checks never change the binary verdict; they are used only to rank issues that pass every required check.