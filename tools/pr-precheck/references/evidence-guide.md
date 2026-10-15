# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives: in an eval bundle, the plan is under `## Plan context`: its `Plan:` text with the in-scope line, `Not in scope`, `Files:`, and approach steps (any deviation note is part of it). What actually changed is `### Diff` under `## Candidate PR`, plus `### Commits`. The claims about the change are in `### Description`. Live, the plan is `plan.md` (its Scope, Files, Approach, and `## Deviations` sections); the diff is `git diff main...HEAD` (and `--stat` for the file list); the claims are in `pr_draft.md` under the title line.

What good looks like: every changed file and hunk is named by the plan or a deviation note, every planned piece (code, test, docs, changelog) is in the diff, and every sentence of the description is true of the diff. Silent drift runs both ways: more than the plan (a new flag or option, a rename pass, an edit to a file the plan never touches), or less (a promised docs update that never appears). A description saying "exactly as planned" or "no other changes" over a diff that contradicts it is drift, not a comms problem. An honest deviation names what changed and why and re-ties the mismatch.

## Test evidence (harness category: not-tested)

Where it lives: in an eval bundle, `### Test evidence` under `## Candidate PR`, read against the `Test plan` and the repro evidence in `## Plan context`, and against the commands the repo facts list (tests, lint, build). Live, `test_evidence.md` next to `plan.md`, plus the Testing section of `pr_draft.md`; the plan's `## Test plan` and the repro in the issue; the repo's checks come from `.github/PULL_REQUEST_TEMPLATE.md` (`make test-unit`, `make test-integration`, `make lint`, `make typecheck`).

What good looks like: the evidence re-runs the plan's repro, including each failure mode the test plan names, and shows a before and an after with an observable result (a value, an exit code, XFAIL becoming PASSED) that meets the plan's expected-after; and the repo's own checks appear with their outcome, or a check that could not run is named with the reason. "Tested locally", "works on my machine", "tests pass" with no output, evidence from the control or an unchanged path, and a before/after for only one of two named failure modes all fail. A failing check reported honestly with its reason still counts.

## Diff quality (harness category: unreviewable)

Where it lives: in an eval bundle, the unified diff in `### Diff` and the messages in `### Commits`. Live, `git diff main...HEAD` and `git log main..HEAD --oneline`.

What good looks like: the fix is visible and nothing unrelated rides along. The tells of debris are debug prints or leftover logging, commented-out code or earlier attempts, dead or unused functions (often behind `allow(dead_code)` or an underscore name), TODO notes, reordered imports, re-indented or re-printed lines, and drive-by edits, with commit messages like "wip", "misc cleanups", or "fmt + cleanup". Churn around a correct fix still buries it. Check that untracked files such as plan.md, comment.md, pr_draft.md, and test_evidence.md are not in the diff (live).

## Standards and comms (harness category: standards-wall)

Where it lives: in an eval bundle, the `## Repo facts` block (`pull requests:` line for the template's sections and asks, `contribution policy` line for the AI rule and other stated policy) read against `### Title` and `### Description`, and against the diff for required files (a changelog or whatsnew entry). Live, `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the repo, read against `pr_draft.md` (its sections, ticked boxes, Notes for Reviewers) and the diff.

What good looks like: every template section has real content for this change (Summary, `Closes #n`, a bullet per change, Testing with pasted results and only true boxes ticked, Notes for Reviewers), every other stated ask is met (issue reference, changelog entry in the diff, tests, title format), and where the policy requires AI disclosure the description states it in plain words. A policy that sets conditions short of disclosure, or no template and no stated asks, is met without a disclosure statement. Boilerplate left as prompt text, an ignored checklist, a missing required entry, a box ticked without evidence, or a missing required disclosure is the wall. (Whether the description's claims match the diff is plan fidelity, above.)
