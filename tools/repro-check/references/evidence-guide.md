# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

**Where it lives:** In eval mode, use the repro report's environment record together with the issue context and repo-facts block. In live mode, use the student's draft repro report and compare it with the issue's target environment and the repository's setup documentation.

**What good looks like:** The report identifies enough concrete environment information for another contributor to understand the conditions of the run, including the relevant operating system/platform, runtime or language version, and repository revision or branch when those can affect the result. If the environment differs materially from what the issue or repository targets, the difference is explicitly called out rather than silently treated as equivalent.

## Steps

**Where it lives:** In eval mode, use the reproduction steps in the repro report and any starting-state information supplied by the issue context. In live mode, use the student's draft repro report together with the issue description and repository setup instructions.

**What good looks like:** A stranger can start from the stated environment or starting state, perform the described actions or commands in order, and reach the trigger being tested without having to invent a missing operation. The steps exercise the behavior named by the issue rather than merely reaching the same component.

## Behavior shown

**Where it lives:** In eval mode, use output excerpts, logs, tracebacks, screenshots, test results, or other artifacts in the repro report, read against the behavior described in the issue context. In live mode, use the artifacts included in the student's draft report and compare them directly with the issue's stated failure or expected behavior.

**What good looks like:** The artifact visibly demonstrates the behavior the issue is about, or provides direct evidence that the expected failure did not occur. A nearby error, unrelated failing test, or output from the same component is not sufficient unless it demonstrates the specific behavior being investigated.

## Honesty

**Where it lives:** Compare the repro report's stated outcome with its commands, observations, and artifacts. Also compare any conclusion in the claim or repro comment with what the attached evidence actually establishes.

**What good looks like:** The conclusion does not claim more than the evidence supports. A successful reproduction is reported only when the artifact demonstrates the issue's behavior. A cannot-reproduce result is valid when the report records the attempted conditions and shows that the reported behavior did not occur; uncertainty or material environment differences are stated explicitly.

## Comms

**Where it lives:** In eval mode, use the claim comment and repro comment together with the issue context, repo-facts contribution policy, and any stated comment or contribution conventions. In live mode, compare the student's draft with the GitHub issue thread, repository contribution documentation/templates, `scope.md`, and `voice-guide.md`.

**What good looks like:** The claim identifies the specific issue and promises the investigation/report without pretending reproduction has already happened or promising a fix or deadline. The repro comment states what was actually attempted and observed, includes enough issue-specific detail to distinguish it from boilerplate, and follows any explicit repository requirements such as AI-use disclosure. Silence on AI passes when the repository has no disclosure requirement.