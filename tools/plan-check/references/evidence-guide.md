# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

**Where it lives:** In eval mode, read the candidate plan's diagnosis together with the issue context and repro-evidence block. In live mode, compare the diagnosis in `plan.md` with the GitHub issue and the reproduction comment already posted for that issue.

**What good looks like:** The stated cause explains the specific behavior demonstrated by the reproduction evidence and does not contradict it. If the evidence establishes only where the failure occurs rather than a proven root cause, the plan distinguishes that observation from a hypothesis that still needs verification.

## Scope

**Where it lives:** In eval mode, use the candidate plan's in-scope and out-of-scope statements, named files or areas, and proposed approach, read against the issue request. In live mode, use the scope and files named in `plan.md` together with the issue and relevant repository code.

**What good looks like:** The plan describes one bounded change that addresses the reproduced issue and names what will not be changed when nearby work could expand the task. Proposed files and areas are relevant to that change rather than unrelated cleanup, redesign, or drive-by refactoring.

## Executability

**Where it lives:** In eval mode, use the candidate plan's files/areas, approach, and ordered implementation actions. In live mode, use those parts of `plan.md` and verify named files or code locations against the repository when needed.

**What good looks like:** Another contributor could identify where to begin and what concrete code behavior to change without asking the author to invent the implementation strategy. The plan names relevant files, functions, or code areas and describes actionable implementation steps rather than only stating the desired end result.

## Test plan

**Where it lives:** In eval mode, compare the candidate plan's test plan with the repro-evidence block, including its trigger, control case, commands, and observed artifacts. In live mode, compare the test plan in `plan.md` with the student's posted reproduction and the repository's relevant tests.

**What good looks like:** The test plan reruns or directly covers the reproduced trigger and states an observable expected result after the change. It also checks relevant non-bug behavior when needed so that success means the reported failure is fixed without simply breaking or bypassing the affected component.

## Honesty

**Where it lives:** In eval mode, inspect the candidate plan's diagnosis, risks, unknowns, assumptions, and any deviations. In live mode, use those sections of `plan.md`; after implementation, compare `## Deviations` with the work actually performed.

**What good looks like:** Unverified causes, uncertain implementation details, and meaningful risks are identified as hypotheses or unknowns rather than facts. If implementation differs from the posted plan, the deviation states what changed and why; if nothing changed, it says so explicitly rather than leaving the section blank.

## Comms

**Where it lives:** In eval mode, read the candidate plan comment against the issue context, thread highlights, repo-facts contribution policy, templates, and disclosure requirements. In live mode, compare `comment.md` with the GitHub issue thread, repository contribution documentation, `scope.md`, and `voice-guide.md`.

**What good looks like:** The comment describes the issue-specific planned change and reflects relevant maintainer/thread guidance instead of using generic boilerplate. It does not promise unverified outcomes or deadlines and follows explicit repository requirements, including AI-use disclosure when required.