# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: in an eval bundle, the plan's `Cause:` or `### Diagnosis` text under `## Candidate plan`. The behavior that cause must explain is in `## Repro evidence`: the numbered steps, the `Control runs` or step-3/4 control lines, and the `Expected` / `Actual` pair. Live, the cause is the Diagnosis section of `plan.md`; the repro evidence is my posted repro comment on the issue thread (the house repro pack for a house issue), as quoted in the drafts.

What good looks like: the cause accounts for every observation, controls included, and names the stage or component where the behavior breaks. A control that works correctly while exercising the suspected component rules that component out. A cause copied from a thread comment still has to survive the repro.

## Scope

Where it lives: the plan's `Scope` / `In:` / `Out:` lines, plus the numbered `Approach` or `Change` steps that show what will really be touched. Live, the Scope section of `plan.md`.

What good looks like: one change, a not-in line a reviewer can hold the diff to, and approach steps that all sit inside the in-scope line. A deferred part stated with a reason is still bounded. A drive-by rewrite shows up as approach steps (migration, new option, redesign, CI rework) that the issue and repro never mention.

## Executability

Where it lives: the plan's `Files` list and `Approach` steps; in compact plans the file path inside the `Change:` sentence. Live, the Files and Approach sections of `plan.md`.

What good looks like: named files or functions, one chosen approach, and an order of work. A stranger could open the repo and begin. Warning signs are "somewhere", "whichever is easier", "investigate the stack", and options instead of a decision.

## Test plan

Where it lives: the plan's `Test:` / `### Test plan` lines, read next to the numbered repro steps in the repro-evidence block. Live, the Test plan section of `plan.md`.

What good looks like: it re-runs the repro steps and states a checkable expected-after (a value, a message, an exit code, a color flip), the way the repro states its `Expected` line. "Run the full suite", "should feel fast", and "nothing else should break" name no observable outcome for this fix.

## Honesty

Where it lives: the plan's `Risks` / `Unknowns` lines, any "not yet verified" or "checked unknown" phrases in the plan or comment, and, live, the `## Deviations` heading at the end of `plan.md`.

What good looks like: specific unknowns ("tool-by-tool `--` support is unverified", "callers that counted the old page") instead of none or "might break stuff". Confidence with no named unknown on a hard problem is a warning. After a build, a changed approach is recorded under Deviations with the reason.

## Comms

Where it lives: the `## Candidate plan comment` section, read against `## Thread highlights` (the maintainer lines) and the `## Repo facts` block (bug-report template asks, contribution policy, AI-use rule). Live, `comment.md` read against the real issue thread and the repo's CONTRIBUTING file.

What good looks like: the comment cites what a maintainer already said (the named culprit file, the requested approach, the prior PR) and either follows it or says why not. It promises only what the plan holds: no dates, no extra scope. If the repo's policy requires AI disclosure, the comment states it in plain words; otherwise a plain description of the work is fine. Boilerplate like "I can fix this, please assign me" that would fit any issue is the opposite.
