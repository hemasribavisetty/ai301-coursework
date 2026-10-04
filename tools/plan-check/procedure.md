# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read the issue context first and record the reported behavior, expected behavior, requested scope, and any maintainer direction.
2. Read the repro-evidence block next. Record the exact trigger, observed artifact or failure, control behavior, environment/code state, and what the evidence proves versus what it does not prove.
3. Read the candidate plan after establishing that baseline. Record its diagnosis, proposed scope, named files or code areas, implementation approach, test plan, risks, assumptions, and unknowns.
4. Read the candidate plan comment after the full plan so its claims can be checked against both the plan and the issue thread.
5. Read repo facts, contribution policy, templates, and thread highlights before grading communication conventions.
6. Do not let a confident plan statement override contradictory issue or reproduction evidence; earlier evidence establishes the baseline against which the plan is judged.

## Evidence gathering

1. For `diagnosis_grounded`, pair each causal statement in the plan with the specific reproduction observation it is intended to explain. Record whether the evidence proves the cause or only supports it as a hypothesis.
2. For `scope_bounded`, record the issue's requested change, the plan's in-scope work, out-of-scope boundaries, and every named file or area. Flag unrelated cleanup, redesign, or independently shippable additions.
3. For `targets_cause`, record the diagnosed cause or hypothesis and the exact code path or condition the implementation proposes to change. If the cause is uncertain, record the proposed verification step.
4. For `executable_approach`, record the files/functions/code areas and concrete implementation actions in their proposed order. Note any step that requires the executor to invent a missing strategy.
5. For `test_plan_decisive`, record the original reproduction trigger and artifact, then map each planned test to an observable expected post-fix result. Record any control/regression check needed to show the component still works.
6. For `uncertainty_honest`, collect assumptions, risks, unknowns, and statements of certainty. Compare them with what the reproduction and issue actually establish.
7. For `comment_conventions`, record relevant thread guidance, contribution/template requirements, AI-disclosure policy, and the issue-specific claims made in the candidate comment.
8. For the preferred `concise_comment` check, compare the comment's content with the diagnosis, change, and validation plan and note unrelated material.

## Check execution

1. Grade `diagnosis_grounded` first because later implementation and test checks depend on what cause the plan claims.
2. Grade `scope_bounded` next to establish the boundaries of the proposed contribution.
3. Grade `targets_cause` by comparing the proposed implementation with the grounded diagnosis. Do not pass a symptom-only workaround unless the evidence supports that as the appropriate cause-level change.
4. Grade `executable_approach` using only the concrete actions and code locations actually stated. Do not invent missing implementation steps for the author.
5. Grade `test_plan_decisive` against the recorded reproduction trigger and expected post-fix behavior. A generic statement such as "run tests" does not establish an observable fix.
6. Grade `uncertainty_honest` by checking whether confidence matches the available evidence and whether material unknowns have an investigation path.
7. Grade `comment_conventions` against the recorded thread and repository requirements. Apply explicit disclosure requirements exactly; do not invent requirements when the repo is silent.
8. Grade `concise_comment` last because it is preferred and cannot change readiness.
9. Use `pass` when the stated evidence satisfies the check's pass condition, `fail` when it contradicts or falls short of it, and `unclear` when the package genuinely lacks enough evidence to decide. Do not convert missing required evidence into a pass.
10. Reuse the gathered evidence for checks that reference the same facts instead of reinterpreting the package from scratch.

## Verdict assembly

1. Collect the grade for every rubric check.
2. Apply the rubric's verdict rule exactly: every required check must pass for `accept`.
3. Treat `unclear` on a required check as a failing condition and return `reject`.
4. Never let a preferred check change an otherwise determined accept/reject verdict.
5. For a rejected package, identify the required check or checks that caused rejection and quote the smallest specific plan, repro, issue, thread, or policy evidence that supports each grade.
6. For an accepted package, briefly cite the evidence supporting each required pass.
7. End with the required JSON block using the rubric check names and the final binary verdict `accept` or `reject`.