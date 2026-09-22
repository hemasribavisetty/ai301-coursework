# Unit 1 — Issue Selection

## Chosen issue

**Issue link:**  
https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68

**Issue title:**  
Keyword search raises `ZeroDivisionError` when the index is empty

**Skill verdict:**  
accept

**Why I chose this issue:**  
I chose Issue #68 because it is a bounded Python bug in the RAG retrieval system and closely matches the skills I want to strengthen. The issue provides a specific error, identifies the relevant implementation and test files, and already has a covering test marked as expected to fail. This gives me a clear starting point for reproducing the problem in Unit 2 and lets me practice debugging, testing, and AI engineering in an existing codebase.

---

## Run history

My first full evaluation scored 17/20. The disagreements were issue-15, issue-19, and issue-20, all related to scope judgments.

I revised the `newcomer_scope` check to distinguish diagnostic causes from true umbrella work and to reject underspecified feature requests that require an unresolved product or design decision.

I then ran a partial evaluation on issue-15, issue-19, and issue-20. Issue-20 changed to the correct `reject` verdict, while issue-15 and issue-19 still disagreed.

Finally, I ran a complete evaluation with `--save-run eval-run.txt`. The final result was 18/20, which met the required bar and matched at least one verdict in every category.

---

## Issue analysis

**Scored issue analyzed:** issue-15

**My rubric verdict:** accept

**Gold verdict:** reject

My rubric accepted issue-15 because the repository was active, the issue was unassigned, there was no open linked pull request, and the closed pull requests were treated as abandoned attempts rather than active ownership. However, the gold label rejected it because the issue had years of design discussion and multiple abandoned implementation attempts. This showed that my rubric handled active ownership well, but did not treat a long history of failed attempts as strong enough evidence that the issue may be too difficult or unsettled for a first contribution.

---

## Check rationale

**Check:** `newcomer_scope`

**Current wording from my rubric:**

> Pass when the issue asks for one bounded contribution. A numbered list of diagnostic causes, implementation clues, or steps for resolving one reported behavior does not by itself make an issue an umbrella. Fail if the issue is explicitly an umbrella/tracking issue whose sub-items are independently shippable contributions, a pure usage/support question, the thread shows unresolved design debate with no maintainer decision, or a maintainer states that the solution requires changes to core internals. Also fail an underspecified feature request when the issue requires a product or design decision but provides no concrete expected behavior, acceptance criterion, maintainer direction, or implementation target. A short issue body or missing reproduction steps alone does not fail this check when the requested change is otherwise bounded and actionable.

I designed this check to separate bounded first contributions from umbrella issues, unresolved design work, support questions, and changes that require deep core-internal modifications. I also refined it after evaluation so that a maintainer listing multiple causes of one bug would not automatically be treated as multiple independent tasks.

---

## Trade-offs

My rubric favors issues with explicit evidence of repository activity, available ownership, bounded scope, and contribution-policy compatibility. This reduces the chance of choosing an abandoned, already-claimed, or overly broad issue. The trade-off is that some nuanced issues may still be misclassified when their difficulty is visible only through a long discussion history or repeated abandoned attempts rather than through a single clear signal.