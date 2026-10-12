# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->


## Plan fidelity (harness category: silent-drift)

**Where it lives:**

- In eval mode, read the package's plan-context block, including its scope, implementation approach, test plan, and any documented Deviations. Compare these directly with the candidate PR's changed-file list, unified diff, commits, title, summary, and change claims.
- In live mode, read `plan.md`, including `## Deviations`, and compare it against `git diff main...HEAD` and `git diff main...HEAD --stat` from the Path Review fork. Compare `pr_draft.md` against the same diff.
- Use the issue description to identify the intended bug or feature and relevant maintainer guidance.

**What good looks like:**

Every material code change corresponds to the original plan or a relevant, explicitly documented deviation. The PR description accurately describes the implemented behavior without hiding additional changes or claiming functionality absent from the diff.

An honestly disclosed omission or changed approach does not automatically fail. Silent drift occurs when the implementation adds unplanned behavior, omits promised core work, or contradicts the description without explaining the difference.

## Test evidence (harness category: not-tested)

**Where it lives:**

- In eval mode, compare the candidate PR's test-evidence section, executed commands, and captured outputs against the plan-context test plan and original reproduction evidence. Read the repo-facts block for required checks.
- In live mode, read `test_evidence.md`, the testing section of `pr_draft.md`, and the test plan in `plan.md`. Compare them with the posted reproduction comment and the repository's PR template and `docs/CONTRIBUTING.md`.
- Record the outcome of each required command, including passing tests, failures, expected failures, empty suites, and skipped or unavailable checks.

**What good looks like:**

The evidence demonstrates the reported behavior before the fix and an observable result after the fix, or provides equivalent decisive regression evidence. Commands and their actual results are recorded, rather than unsupported claims such as "all tests pass."

Repository-required checks are attempted and their outcomes are disclosed accurately. A failing check or an empty integration suite does not automatically make a PR unready when the limitation is honestly documented and no repository rule requires a successful result.

## Diff quality (harness category: unreviewable)

**Where it lives:**

- In eval mode, inspect the candidate PR's unified diff, changed-file list, and commit history. Compare the affected areas against the issue and plan.
- In live mode, inspect `git diff main...HEAD`, `git diff main...HEAD --stat`, and the branch's commits.
- Check for changes outside the planned implementation and regression tests.

**What good looks like:**

The fix and its supporting tests are identifiable in the diff, and the changed files are relevant to the reported issue. The implementation contains no accidental local files, debugging output, dead code, commented-out experiments, generated artifacts, unnecessary formatting churn, or unrelated refactors.

A small necessary supporting change is acceptable when its connection to the fix is clear. An unrelated edit remains a reviewability problem even if the PR description discloses it.

## Standards and comms (harness category: standards-wall)

**Where it lives:**

- In eval mode, inspect the repo-facts block for the PR template, contribution requirements, mandatory checks, AI-use policy, and relevant maintainer instructions in the issue thread. Compare these with the candidate PR title, description, testing evidence, and disclosures.
- In live mode, read `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, the issue thread, `pr_draft.md`, and `test_evidence.md`. Also apply `voice-guide.md` to the outgoing title and description.
- Distinguish repository-mandated requirements from optional suggestions.

**What good looks like:**

The PR identifies its issue, addresses each required template section with meaningful content, reports testing accurately, and follows the repository's stated contribution conditions. Mandatory AI-use disclosures are included when required; silence in the repository policy does not create a new mandatory policy.

For this CodePath assignment, the PR's Notes for Reviewers must also describe how AI assistance was used. Explicit maintainer instructions should be acknowledged when relevant. An honest N/A or disclosed shortfall is acceptable when permitted by the repository's requirements.
