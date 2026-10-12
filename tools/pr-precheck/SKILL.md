---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->


## The question

Determine whether exactly one candidate pull request is ready to submit. Compare its title, description, committed branch diff, test evidence, and implementation against the issue, the original plan, and any documented deviations. Do not grade unrelated issues or substitute general impressions for the rubric.

## Inputs and modes

**Live mode:**
1. Read `plan.md`, including `## Deviations`, from the working repository.
2. Run `git diff main...HEAD` from the repository root to obtain the complete committed change against the default branch. Also inspect `git diff main...HEAD --stat` and relevant commits.
3. Read the PR title and description from `pr_draft.md`.
4. Read commands, outputs, and before/after results from `test_evidence.md`.
5. Read the issue URL supplied by the user, including its body, relevant discussion, and maintainer guidance.
6. Read `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` for repository-specific requirements.
7. For a house-chain submission, use the provided house plan, reproduction package, and working branch in place of the student's own artifacts.

Do not claim that a check passed unless the available evidence demonstrates it. Do not open a PR, modify code, or post comments while grading.

**Eval mode:**
1. Read only the supplied frozen evaluation package.
2. Treat the package as the complete source of facts about the issue, plan, diff, tests, PR description, thread, and repository policies.
3. Do not access GitHub, the local repository, other packages, or external files for issue facts.
4. Grade every rubric check and apply the complete verdict rule, even if an earlier required check fails.
5. Identify the evaluated item using its supplied package ID.

## The scope seam (live mode only)

Read `scope.md` before collecting any other live-mode evidence. Confirm that the configured `Repo:` line names the repository containing the issue and intended PR.

If the repository line is still a placeholder, stop without grading and instruct the user to fill it. Refuse to grade a live PR belonging to another repository. Apply the Path Review house rules described in the scope.

Eval mode ignores `scope.md` entirely.

## The voice seam (live mode only)

Read `voice-guide.md` before grading the outgoing PR title and description. Compare both against the user's stated communication rules and identify any violated rule with the exact offending text.

Report voice-guide violations in the readable summary. They do not independently change the final verdict unless an explicit rubric check makes them relevant.

Eval mode ignores `voice-guide.md` entirely.

## Component reads

Read `rubric.md` to obtain the exact checks, evidence requirements, weights, and verdict rule.

Read `references/evidence-guide.md` to locate each evidence family in the candidate package and understand what observable evidence supports a check.

Execute `procedure.md` exactly as written, including its read order, evidence gathering, check execution, and verdict assembly stages.

If `rubric.md` or `procedure.md` contains only template comments or has no substantive authored instructions, refuse to grade and identify the missing component.

If a procedural step is missing or ambiguous, report that gap. Do not invent a replacement procedure or silently add new checks.

## Verdict and output

The only permitted verdicts are `accept` and `reject`.

`accept` means the candidate PR meets every required rubric check and is ready to submit.

`reject` means at least one required check fails or is treated as failing under the rubric's verdict rule.

Provide a readable summary with each check's name, grade, and decisive evidence. Explain blocking failures and relevant reservations without changing the binary verdict.

End with the exact JSON schema supplied below. Preserve its field names and order, use valid JSON, and place nothing after the closing code fence.


```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```


## Grading discipline

1. Grade evidence before conclusions. Every check must cite a concrete fact, artifact, command result, diff location, or relevant quotation. Never use "looks good" as sufficient evidence.
2. Grade the actual PR, not its writing style or length. A concise but complete submission can pass; a polished description that conceals unplanned changes must fail the relevant check.
3. Apply the rubric's pass conditions exactly. Do not override a check because the overall submission feels ready or unready.
4. Follow `procedure.md` to gather evidence and assign grades. Report procedure gaps instead of improvising new grading steps.
5. Use `pass` when evidence satisfies the condition, `fail` when evidence contradicts or falls short of it, and `unclear` when essential evidence is unavailable.
6. Apply the rubric's explicit rule for `unclear`. If the verdict rule does not address it, treat `unclear` on a required check as failing.
7. Compare every material change in the diff against the plan and its Deviations. A disclosed, relevant deviation is not automatically a failure; unrelated changes or concealed drift must be graded under the appropriate rubric check.
8. Distinguish tests actually executed from tests merely proposed. Do not assume a command passed because the PR description checks a box.
9. Respect repository contribution policies and PR-template requirements, including any mandatory disclosure of AI assistance.
10. Return one verdict for one package, with the complete per-check JSON block as the final content.
