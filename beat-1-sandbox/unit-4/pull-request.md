# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/163

**Branch**

`fix/68-handle-empty-keyword-index`

---

## Eval iterations

**Run history**

I completed one full scored evaluation of my `pr-precheck` skill using the Unit 4 evaluation harness.

The evaluation achieved **20/20 agreement** with the instructor's gold labels, exceeding the required 18/20 threshold. All five categories achieved full agreement:

- clear-accept: 7/7
- not-tested: 4/4
- silent-drift: 4/4
- standards-wall: 2/2
- unreviewable: 3/3

The run was saved using `--save-run eval-run.txt`. No rubric revisions or targeted `--only` retries were necessary because the first full evaluation passed every scored package.

**Package analysis**

I analyzed `pkg-03`, which belongs to the `silent-drift` category. My rubric verdict was `reject`, and the instructor's gold verdict was also `reject`.

The issue involved Starship's directory module disappearing when the current working directory was a symlink pointing to a subdirectory inside a Git repository. The accepted plan proposed one bounded fix in `src/modules/directory.rs`: replace the `?` operator on `contract_repo_path` with a fallback to `contract_path` when the logical directory is outside the physical repository root.

The candidate PR implemented that fallback, but its diff also included two additional changes that were not part of the plan:

1. A refactor of the `truncate()` helper that changed how path components were processed.
2. A new `resolve_symlinks` configuration option added to `DirectoryConfig` and its default configuration.

These changes appeared in separate commits, `5b77a1f` and `c2ee8d8`, but the PR description claimed that it implemented the posted plan exactly and that there were no other behavior changes. The plan explicitly excluded configuration changes and did not include a truncation refactor. No deviation was documented to explain either addition.

My `plan_fidelity` check therefore rejected the package because the implementation silently expanded beyond the approved scope. The `description_matches_diff` check also identified that the PR description did not accurately disclose the additional changes.

Although the reproduction evidence showed that the original symlink bug was fixed and `cargo test directory` passed 34 tests, successful tests did not justify the unplanned modifications. This package demonstrates why the actual diff must be compared with the original plan rather than trusting the PR description alone.

**Check rationale**

Check: `plan_fidelity`

> Pass when every material change in the diff is covered by the plan or explicitly explained in Deviations, and the implementation addresses the planned issue. Fail when the diff silently adds unrelated functionality, omits a promised core change without disclosure, or contradicts the plan while claiming fidelity. A relevant, honestly documented deviation does not fail by itself.

I designed this check to identify silent differences between a proposed implementation and the actual pull request. A description may appear complete while the committed code contains additional changes or leaves out something important.

I also wanted the rubric to distinguish between undisclosed scope changes and legitimate implementation adjustments. A change from the original plan can still be acceptable when it is relevant to the issue and clearly documented in the Deviations section.

This makes the rubric evidence-based rather than requiring an implementation to follow its initial plan rigidly.

**Trade-offs**

My rubric prioritizes consistency between the original plan, implementation diff, testing evidence, and PR description. This helps identify pull requests that silently change scope or make unsupported claims.

The trade-off is that a useful implementation change may require additional documentation before the pull request is considered ready. I accept this trade-off because a reviewer should be able to understand why the implementation differs from the original plan.

I also chose not to automatically reject pull requests for honestly reported testing limitations. For example, a repository may have an empty integration test suite. In that situation, accurately documenting the attempted command and its outcome is more useful than claiming all tests passed.

My first full evaluation achieved 20/20, so I made no rubric revisions. The final saved run confirms that the rubric correctly classified all scored packages in every evaluation category.

---

Related paths: `eval-run.txt` in this directory; skill files in `tools/pr-precheck/`.