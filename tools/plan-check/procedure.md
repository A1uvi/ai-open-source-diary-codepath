# Procedure: how plan-check grades one plan package

Follow these steps in order, exactly as written. They are for grading a plan,
not for making one. If a step cannot be done because the package lacks the
thing it names, say so in the summary and apply the rule in step 3.4 rather
than inventing a fill-in.

## Read order

1. In eval mode, read only the bundle text, top to bottom, and fetch nothing.
   In live mode, read `scope.md` first (stop if it still holds a placeholder),
   then `voice-guide.md`, then the drafts and the thread as step 2 says.
2. Read the **Repo facts** block first. Note the contribution policy line and
   whether it states any AI-use rule: disclosure required, conditions short of
   disclosure, ban, or silent.
3. Read the **Issue** and **Thread highlights**. List each maintainer signal
   (a named culprit file, a requested approach, a prior PR, a confirmed
   decision) as a short bullet.
4. Read the **Repro evidence** block next, before any plan text. List every
   observation, with control runs on their own lines, and write down what each
   control rules out.
5. Only then read the **Candidate plan**, then the **Candidate plan comment**.
   Reading the repro before the plan keeps the plan's story from colouring
   what the evidence shows.

## Evidence gathering

1. Diagnosis: copy the plan's one-sentence cause. Beside it, list the repro
   observations from step 4 above, including each control run. In live mode
   the repro evidence is the student's posted repro comment on the issue (or
   the house repro pack as quoted in the drafts); take only what the drafts
   quote.
2. Scope: copy the plan's in-scope line and its not-in line (write "none" if
   missing). Then list every concrete action in the approach. Mark each action
   as inside or outside the in-scope line.
3. Executability: list every file, function, or layer the plan names, the one
   approach chosen, and any phrase that defers a decision ("somewhere",
   "whichever", "investigate", "maybe").
4. Test plan: copy the plan's test steps and the expected-after. Compare with
   the repro steps and write down whether the repro is re-run and what
   observable result is promised.
5. Comms: copy the sentences in the plan comment that (a) refer to a thread
   signal, (b) promise something (timeline, extra work, outcome), and (c)
   mention AI use. Match each thread signal from read-order step 3 to the
   sentence that engages it, or write "not engaged".
6. Honesty: copy the lines naming risks, unknowns, or things not yet checked
   (write "none" if absent).

## Check execution

1. Run the checks in the rubric's table order. Grade each from the lists made
   in evidence gathering; do not re-read the whole package unless the list for
   that check is empty.
2. For each check, apply the pass condition literally. Record `pass`, `fail`,
   or `unclear`, and one line of evidence: the quote or fact that decided it.
3. For `diagnosis-grounded`, test the cause against each control run in turn:
   if a control behaves correctly with the suspected component exercised, the
   cause fails. Do not accept a diagnosis because the thread or the plan
   sounds sure of it.
4. If evidence for a check is genuinely absent from the package, grade
   `unclear`, name what is missing in the evidence line, and continue. Never
   fill the gap from outside knowledge of the project.
5. For `ai-disclosure`, use the policy classification from read-order step 2.
   Require explicit disclosure text in the comment only when the policy
   requires disclosure; a human-sounding comment never counts as disclosure.
6. Grade the plan, not the polish: a short complete plan can pass every check
   and a long confident one can fail on one.

## Verdict assembly

1. Collect the grades of every `required` check. Apply the rubric's verdict
   rule: `accept` only if all of them are `pass`; otherwise `reject`.
2. Count `unclear` as `fail`, as the rubric's verdict rule says.
3. Ignore `preferred` checks for the verdict; still list them in the output.
4. In the JSON, put one entry per check in table order. For the check or
   checks that decided a `reject`, make the evidence line a direct quote from
   the package (the plan or repro text itself), not a paraphrase.
5. Emit the fenced JSON block last, with nothing after it. In live mode, put
   any voice-guide rule the comment breaks in the summary above the block.
