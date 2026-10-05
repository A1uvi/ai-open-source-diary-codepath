# Rubric: is this plan package ready to post and build from?

Grade every check below, then apply the verdict rule. Read the repro evidence
before the plan: the plan is only as good as the behavior it claims to explain.
The deciding evidence is whether the plan's own words, held against the repro
evidence, the issue thread, and the repo-facts block, give a stranger something
they can build and a maintainer something they can say yes to. Length,
confidence, and polish are not evidence.

## Checks

| Check | Evidence | Pass condition | Weight |
| --- | --- | --- | --- |
| diagnosis-grounded | The plan's stated cause, held against every observation in the repro-evidence block, including control runs and "what happened instead" lines. | The stated cause explains the repro's actual observations and is not contradicted by any of them. A control run that behaves correctly (same input, one thing changed, no failure) rules out any cause located in the component that control exercised. A cause that only repeats the symptom, or that adopts a thread comment's theory without checking it against the repro, fails if the repro evidence points elsewhere. If the repro evidence neither supports nor contradicts the cause, grade unclear. | required |
| bounded-scope | The plan's in-scope statement, its not-in line, and everything its approach actually does. | One change the issue and repro justify, with a stated not-in line, and the approach stays inside it. A bounded core fix that explicitly defers a harder or untestable part, and says so with a reason, passes. Fails when the approach adds work the issue never asked for (a refactor, migration, new option, new framework, CI or test-harness rework, "while I'm here" cleanup), even when the core fix inside it is right. Also fails when the plan changes something other than what the repro shows is broken (a docs-only workaround for a code defect the thread is already fixing). | required |
| executable | The plan's named files or areas, its chosen approach, and its order of work. | A stranger could start from the plan without asking the author anything: it names the files, functions, or layers it will touch and commits to one approach. Fails when the real decisions are deferred to build time ("somewhere", "upstream or vendored, whichever is easier", "investigate first", "maybe also check"), when no file or area is named, or when the approach is a list of options. | required |
| test-decisive | The plan's test plan, held against the repro steps. | The test re-runs the repro (or a named equivalent) and states an observable expected-after that a stranger can check: a value, an output, an exit code, a state change. A test plan whose only outcome is the full suite passing, "should feel fast", "nothing else should break", or "see if it works" fails. Extra regression checks are fine as long as the repro re-run is there. | required |
| thread-aware | The plan comment, held against the issue's thread highlights (maintainer signals) and the repo-facts block's contribution asks. | The comment engages what a maintainer already said in the thread: it follows or explicitly answers stated direction (a requested approach, a named culprit file, a prior PR, a confirmed design decision), and where it departs from that direction it says why. It promises only what the plan contains: no timeline it doesn't control, no extra scope the plan lacks, no outcome it hasn't shown. Fails when explicit maintainer direction in the thread is ignored, or when the comment commits to something the plan doesn't hold. A thread with no maintainer signal passes when the comment is specific to this issue. | required |
| ai-disclosure | The repo-facts block's contribution policy, held against the plan comment's own text. | Treat every package as AI-assisted work. If the repo's stated policy requires disclosing AI use, the plan comment must state it explicitly, and a missing disclosure fails. If the policy sets only conditions short of disclosure (human review, understanding the change), pass without a disclosure statement. If the policy states nothing about AI, pass. An outright ban on AI-generated contributions fails. A comment that reads as human-written does not satisfy a stated disclosure requirement. | required |
| unknowns-stated | The plan's risks and unknowns section, or any line naming something it hasn't verified. | At least one real unknown or risk is named specifically (a caller that might depend on the old behavior, a platform not tested), rather than none or generic "might break things". | preferred |

## Verdict rule

Accept only if every `required` check passes. `unclear` counts as `fail` on
every check: a fact the package's own text doesn't let me decide is not
evidence for either side, and a plan I can't verify isn't ready to build from.
`preferred` checks never change the verdict; `unknowns-stated` only ranks plans
that already pass everything required.

A plan that defers part of the issue on purpose and says so is graded through
the same checks as any other, and accepts when it passes them. A polished,
confident plan rejects on a single required failure, however complete the rest
reads. Confidence is not a check; nothing in this rubric grades it.
