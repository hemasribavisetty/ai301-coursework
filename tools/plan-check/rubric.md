# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis_grounded | Candidate plan's stated diagnosis read against the issue context and repro-evidence block, using the Diagnosis and grounding section of the evidence guide | Pass when the diagnosis explains the specific reproduced behavior without contradicting the evidence. If the evidence establishes only a failure location or correlation rather than a proven root cause, the plan must identify the proposed cause as a hypothesis or verification target instead of presenting it as certain. | required |
| scope_bounded | Candidate plan's in-scope and out-of-scope statements, named files/areas, and proposed approach read against the issue request, using the Scope section of the evidence guide | Pass when the proposed work is one bounded change directed at the reproduced issue and the named files/areas are relevant to that change. Fail when the plan expands into unrelated cleanup, redesign, or independently shippable work not needed to address the issue. | required |
| targets_cause | Candidate plan's diagnosis and proposed implementation read together against the repro evidence and relevant issue context | Pass when the proposed implementation acts on the code path or condition the grounded diagnosis identifies as responsible for the reproduced behavior, rather than merely suppressing an error or altering an unrelated symptom. If the exact cause remains uncertain, pass only when the plan includes a concrete verification step before committing to the proposed change. | required |
| executable_approach | Candidate plan's named files/functions/code areas and implementation actions, using the Executability section of the evidence guide | Pass when another contributor could identify where to start and what concrete behavior to change without inventing the implementation strategy. The plan must identify relevant code locations and actionable implementation steps; a statement of the desired end state alone fails. | required |
| test_plan_decisive | Candidate plan's test plan read against the repro-evidence trigger, control case, commands, and artifacts, using the Test plan section of the evidence guide | Pass when the test plan directly reruns or covers the reproduced trigger, states an observable expected result after the fix, and checks relevant non-bug behavior when needed to distinguish a real fix from bypassing or breaking the component. | required |
| uncertainty_honest | Candidate plan's diagnosis, assumptions, risks, unknowns, and deviations, using the Honesty section of the evidence guide | Pass when material uncertainty is identified as uncertainty rather than stated as fact. Known risks or unresolved implementation questions that could change the approach are surfaced with a way to investigate them. Do not fail merely because the plan has no unknowns when the evidence actually resolves the relevant questions. | required |
| comment_conventions | Candidate plan comment read against the issue context, thread highlights, repo-facts contribution policy/templates, and disclosure requirements, using the Comms section of the evidence guide | Pass when the comment is specific to the planned change, does not contradict relevant maintainer/thread guidance, and follows explicit repository requirements including AI-use disclosure when required. Silence on AI use passes when the repository states no disclosure requirement. | required |
| concise_comment | Candidate plan comment read against the issue and candidate plan | Pass when the comment communicates the diagnosis, intended change, and validation approach without unnecessary unrelated detail. | preferred |

## Verdict rule

Accept the plan package only when every required check passes. A fail on any required check rejects the package. `unclear` on a required check counts as fail because unresolved evidence means the plan is not yet ready to post or build from. Preferred checks never change the binary verdict; they provide additional quality guidance only.