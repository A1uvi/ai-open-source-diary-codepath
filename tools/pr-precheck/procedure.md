# Procedure: how pr-precheck grades one PR package

Follow these steps in order, exactly as written. They are for grading a PR, not for making one. If a step cannot be done because the package lacks the thing it names, say so in the summary and apply the rule in Check execution step 4 rather than inventing a fill-in.

## Read order

1. In eval mode, read only the bundle text, top to bottom, and fetch nothing. In live mode, read `scope.md` first (stop if its `Repo:` line is a placeholder), then `voice-guide.md`, then gather the live inputs as the evidence guide says.
2. Read the repo facts (live: the PR template and `docs/CONTRIBUTING.md`) first. List every ask the repo makes of a PR: template sections, required references (`Closes #n`), changelog or whatsnew entries, required checks to run, title format. Write down the AI-use policy as one of: disclosure required, conditions short of disclosure, ban, or silent.
3. Read the issue and thread highlights. Note any maintainer direction (named file, requested approach, constraint).
4. Read the plan context (live: `plan.md`) before the PR. Note: the in-scope line, the not-in line, the files list, each approach step, the test plan with its expected-after and every failure mode it names, and any deviation notes. Reading the plan first fixes what "matches" means before the description's confident wording can colour the diff.
5. Read the diff next, before the description. List every changed file and, per file, what each hunk does in one line. Read the commit list.
6. Read the test evidence.
7. Read the title and description last, and treat each sentence as a claim to be checked against the lists above.

## Evidence gathering

1. Plan against diff: build two lists. List A is every file and planned change the plan names (code, tests, docs, changelog). List B is every file and hunk in the diff. Mark each B entry as planned, covered by a deviation note, or unplanned. Mark each A entry as present or missing, and for missing ones note whether the description or deviations disclose it.
2. Description against diff: copy every claim the description makes about what changed or what was delivered, including fidelity phrases ("exactly as planned", "no other changes", "docs updated"). Beside each, write the diff fact that confirms or contradicts it, or "no support".
3. Test evidence against test plan: copy the plan's test plan and list the repro steps and each failure mode it names. For each, find the part of the evidence that re-runs it and write the observable before and after. Write "not shown" where the evidence is silent, and flag any evidence that exercises a control or an unchanged path instead.
4. Repo checks: list the commands the repo asks for (from step 2 of Read order). For each, copy the output line that shows its outcome, or the stated reason it could not run, or "not shown".
5. Debris: scan every added line, deleted line, and hunk header in the diff for debug prints, commented-out code, unused or dead functions, TODO notes, reordered imports, reformatted or re-printed lines unrelated to the fix. Note any commit message such as "wip", "misc cleanups", or "fmt". Write "none" if clean.
6. Standards: copy each template section as the description fills it, and mark it real content, prompt-text only, or empty. Check the diff for any file the contributing guide requires (changelog, whatsnew, tests). Check that checkboxes ticked are backed by evidence. Find the AI disclosure sentence if any.
7. Honesty: copy any line naming something left out, deferred, not run, or changed from the plan (write "none" if absent).

## Check execution

1. Run the checks in the rubric's table order. Grade each from the lists made in evidence gathering; do not re-read the whole package unless the list for that check is empty.
2. For each check, apply the pass condition literally. Record `pass`, `fail`, or `unclear`, and one line of evidence: the quote or fact that decided it.
3. For diff-matches-plan, any unplanned B entry with no deviation note fails the check, and so does a missing A entry with no disclosure. Do not excuse an unplanned hunk because it is small, adjacent, or well meant. For description-matches-diff, one false claim fails the check, even if every other sentence is true.
4. If the evidence for a check is genuinely absent from the package, grade `unclear`, name what is missing in the evidence line, and continue. Never fill the gap from outside knowledge of the project.
5. For repro-rerun-before-after, the plan's primary repro must be re-run with a decisive before and after. A secondary scenario the test plan lists passes when the evidence states its observed result or an added test covers it. Evidence from only the control or an unchanged path never passes, and neither does a named failure mode that the evidence is silent about.
6. For ai-disclosure, use the policy classification from Read order step 2. Require explicit disclosure text only when the policy requires disclosure; a human-sounding description never counts as disclosure.
7. Grade the PR, not the polish: a short complete PR can pass every check and a long confident one can fail on one.

## Verdict assembly

1. Collect the grades of every `required` check. Apply the rubric's verdict rule: `accept` only if all of them are `pass`; otherwise `reject`.
2. Count `unclear` as `fail`, as the rubric says.
3. Ignore `preferred` checks for the verdict; still list them in the output.
4. In the JSON, put one entry per check in table order. For the deciding check, the first failing required check in table order, make the evidence line a direct quote from the package (the diff line, the description sentence, or the evidence text itself), not a paraphrase. Other failing checks get a one-line fact.
5. Emit the fenced JSON block last, with nothing after it. In live mode, put any voice-guide rule the description or title breaks in the summary above the block, and list any procedure gaps there too.
