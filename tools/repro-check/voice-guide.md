# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I am a student contributor learning to work in an existing open-source codebase. I try to be specific about what I have actually checked and avoid presenting assumptions as confirmed facts. When I claim an issue, maintainers can expect me to investigate it in my own environment and follow up with the evidence I observe.

## Rules I write by

### Rule: Say only what I have verified

I distinguish between what I plan to investigate and what I have already reproduced. Before running a reproduction, I do not write as though the bug has already been confirmed.

- Wrong: "I reproduced the `ZeroDivisionError` and will fix it."
- Right: "I'd like to investigate the reported `ZeroDivisionError` when `KeywordSearcher.index()` receives an empty corpus. I'll reproduce it in my environment and follow up with the commands, environment details, and observed output."

### Rule: Name the actual behavior

I include enough issue-specific detail that my comment could not be copied unchanged onto an unrelated issue.

- Wrong: "I'd like to work on this issue and will investigate the bug."
- Right: "I'd like to investigate the reported `ZeroDivisionError` from calling `KeywordSearcher.index()` with an empty corpus."

### Rule: Do not promise a fix or deadline

A claim promises an investigation and evidence, not a successful fix or a completion date.

- Wrong: "I'll have this fixed by tomorrow."
- Right: "I'll reproduce the reported behavior first and follow up with what I observe."

### Rule: Separate observation from interpretation

In a reproduction report, I show the command or input and the resulting evidence before stating my conclusion.

- Wrong: "The search code is definitely broken because BM25 cannot handle empty indexes."
- Right: "Calling `index([])` produced a `ZeroDivisionError` inside `BM25Okapi` in my environment. This matches the failure described in the issue."

### Rule: State limitations directly

If my environment differs from the repository's documented setup, or if I cannot reproduce the issue, I say that clearly rather than hiding the difference.

- Wrong: "Everything works fine, so the issue must already be fixed."
- Right: "I could not reproduce the reported failure under the environment below. My Python version differs from the documented environment, so I am treating this as a cannot-reproduce result rather than concluding that the issue is fixed."

## Things I never post

- A claim that says I reproduced something before I actually ran the reproduction.
- A promise that I will fix an issue or finish it by a specific date.
- "Same as above" or another student's reproduction presented as my own evidence.
- A confident conclusion that goes beyond the command output, logs, screenshots, or tests I actually observed.
- Generic claim comments that do not identify the issue-specific behavior I intend to investigate.
- AI-use statements that conflict with the repository's contribution or disclosure requirements.