# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->


## Read order

1. Identify the single PR package being evaluated. In live mode, confirm that the issue and repository satisfy `scope.md` before reading the submission. In eval mode, use only the supplied package bundle and ignore external sources.
2. Read the issue context first. Record the reported problem, expected behavior, relevant maintainer instructions, and contribution requirements.
3. Read the original plan, including its scope, implementation approach, test plan, risks, and Deviations. Record the intended changes, excluded work, and any later documented changes to the approach.
4. Read the actual implementation next. In live mode, run `git diff main...HEAD` and `git diff main...HEAD --stat`. In eval mode, read the package's diff and changed-file information. Record every changed file and the behavior each change introduces.
5. Compare the implementation with the plan before reading the PR description. This prevents the description's claims from influencing what the diff actually shows.
6. Read the test evidence, including executed commands, output, before/after reproduction, and any failed or unavailable checks. Compare this evidence with the plan's test plan and the repository's required checks.
7. Read the PR title and description. Record its issue reference, summary, listed changes, testing claims, limitations, and disclosure statements.
8. Read the repository's PR template, contribution policy, and relevant issue-thread guidance. In live mode, also apply the voice guide to the outgoing title and description.

## Evidence gathering

1. For `plan_fidelity`, create a correspondence between every material changed file or behavior in the diff and the plan's scope, approach, or Deviations. Record any unplanned additions, missing core work, or disclosed changes.
2. For `description_matches_diff`, compare every substantive claim in the PR title, summary, change list, and issue reference with the actual diff. Record unsupported claims and material changes the description does not acknowledge.
3. For `test_evidence`, locate the original reproduction trigger and expected behavior in the plan or reproduction evidence. Record the commands actually executed, their observed outputs, and whether the post-fix behavior directly addresses the issue.
4. For `repository_checks`, identify each check explicitly required by the repository's contribution policy or PR template. Record whether it was run, its exact outcome, and any disclosed failure, skip, or unavailable test suite. Do not infer execution from a checked box alone.
5. For `diff_reviewability`, inspect the changed-file list and diff hunks for unrelated refactors, debugging statements, commented-out experiments, generated files, accidental planning documents, or other unnecessary changes.
6. For `standards_and_disclosure`, compare the PR description against the repository's required template content, contribution policy, issue-linking convention, and applicable AI-use disclosure requirements. Record what is satisfied or missing.
7. For `reviewer_clarity`, determine whether the description and diff together identify the problem, implemented behavior, affected files, and actual test results.
8. In live mode, inspect `voice-guide.md` and record any specific rule violated by the title or description. Report it separately unless the rubric makes it part of a required check.
9. Use `references/evidence-guide.md` to locate each evidence family. In eval mode, do not search outside the frozen package.

## Check execution

1. Grade every check in the order listed in `rubric.md`, even if an earlier required check fails.
2. Grade `plan_fidelity` by matching the implementation against the original plan and documented Deviations. Do not fail a relevant, honestly disclosed deviation merely because it differs from the original approach.
3. Grade `description_matches_diff` by verifying that the PR description accurately reports the implemented changes and does not conceal material differences.
4. Grade `test_evidence` by checking that actual commands and outputs demonstrate the reported issue's behavior before and after the change, or provide equivalent decisive regression evidence. Distinguish observed results from intended results.
5. Grade `repository_checks` against the repository's explicitly required commands. A reported failure or empty suite does not automatically fail when it is accurately disclosed and the repository permits submission with that outcome.
6. Grade `diff_reviewability` by determining whether the committed diff is focused and free of unrelated or disposable changes.
7. Grade `standards_and_disclosure` against explicit PR-template and contribution-policy requirements. Never invent a mandatory disclosure rule when the repository is silent, but enforce a stated requirement.
8. Grade `reviewer_clarity` last. This preferred check provides feedback without changing the verdict.
9. Assign `pass` when the evidence satisfies the check's stated condition, `fail` when it contradicts or falls short of the condition, and `unclear` when necessary evidence is genuinely unavailable.
10. Never invent missing test outputs, implementation details, or policy requirements. If evidence is absent, grade it according to the rubric rather than assuming success.
11. Reuse previously gathered evidence for overlapping checks instead of rereading the package and changing the interpretation without a factual reason.

## Verdict assembly

1. Collect all per-check grades and their supporting evidence in rubric order.
2. Apply the rubric's verdict rule exactly. Return `accept` only if every required check passes.
3. Return `reject` if any required check is `fail` or `unclear`. Preferred checks never alter the binary verdict.
4. If several required checks fail, identify the first failing required check in rubric order as the primary blocker. Also report the remaining blockers.
5. Quote the smallest decisive fact, output line, plan statement, diff hunk, or repository requirement supporting each blocking grade. Do not substitute general impressions for evidence.
6. For accepted packages, summarize the concrete evidence supporting the required passes and note any nonblocking limitations.
7. Produce a readable per-check summary, followed by the exact JSON output required by `SKILL.md`.
8. The final fenced JSON block must contain the package identifier, every check with its grade and one-line evidence, and the binary verdict. Preserve the contract's schema and field order. Write nothing after the closing code fence.
