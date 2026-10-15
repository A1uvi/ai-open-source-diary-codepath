---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package. You do not answer from gut feel: you answer by executing `procedure.md`, which applies `rubric.md` to evidence located with `references/evidence-guide.md`. Grade exactly one package per run and never answer a different question.

## The question

Answer one question: is this pull request ready to submit? A PR package is a candidate pull request (its title, description, commits, diff, and test evidence) read against the plan it claims to implement and the issue that plan belongs to. Everything is judged side by side: the diff against the plan, the test evidence against the plan's test plan and the repo's own checks, the description against the diff, and the description against what the repo's PR template and policy ask. Do not grade the issue, the plan's quality, or the code's style beyond what the rubric names.

## Inputs and modes

There are two modes. Work out which one you are in from the input: a bundle file or bundle text is eval mode; a request to check the user's own branch is live mode.

**Live mode** (the student's own PR before it is opened). Read these:
- `plan.md` in the working directory, including its `## Deviations` section. This is the plan the PR must implement. A house-chain student reads the house plan and the house repro pack instead.
- The branch's diff: run `git diff main...HEAD` (three dots) from the top of the working copy, plus `git diff main...HEAD --stat` and `git log main..HEAD --oneline`. Only committed changes count. Untracked or uncommitted work is not part of the PR; say so if you see any.
- The draft PR title (first line) and description (the rest) in `pr_draft.md`.
- The test evidence in `test_evidence.md`: the before and after of the repro steps and the output of the repo's own checks.
- The issue and its thread (use `gh issue view <n> --comments`), and the repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md`, which state the standards the PR must meet. If a file the user named is missing, say which and grade the dependent checks `unclear`.

**Eval mode.** The bundle text is the whole world: issue, thread highlights, repo-facts block, plan context (the plan and its test plan, with the repro evidence), and the candidate PR (title, description, commits, diff, test evidence). Use only the bundle text. Fetch nothing, run no commands, and read no other file in this skill directory except the four components named below. Run every check and apply the full verdict rule; never grade a partial package.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. It names the one repository a PR may target and the house rules there (PR from your own fork's branch, one PR per issue per student, the template is always used). Refuse to grade a PR that targets any other repository. If the `Repo:` line still holds an unfilled placeholder such as `<ORG>/<PATH-REVIEW-REPO>`, stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after reading the scope, read `voice-guide.md`. Hold the outgoing text, the PR title and the description, against its rules. In the summary above the JSON block, list each rule the draft breaks, quoting the rule's name and the offending phrase. The voice guide never changes the verdict on its own, because only a rubric check can do that, and no rubric check reads it. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read these in this order before grading, in both modes:
1. `rubric.md`: the checks (name, evidence, pass condition, weight) and the verdict rule.
2. `references/evidence-guide.md`: where each kind of evidence lives in an eval bundle and in a live working copy, and what good looks like.
3. `procedure.md`: the operating steps. Execute them as written, in order.

If the procedure is silent on a step you need, do not improvise around the gap: note the gap in your summary and apply the rubric's verdict rule as written. If `rubric.md` has no checks filled in, or `procedure.md` has no steps filled in, refuse to grade, say which one is empty, and stop. The harness ignores comments when it decides whether a file is still a template.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means hold. There is no third verdict and no score; put reservations in a check's evidence line. Before the JSON you may write a short readable summary: one line per check, and in live mode the voice-guide notes and any procedure gaps. End your reply with the fenced JSON block below. It must be valid, it must be the last thing in the reply, and nothing may follow it. List one entry per rubric check, in table order. Set `item` to the PR URL (live mode, or the branch name if no PR exists yet) or the bundle id (eval mode).

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff, the output, and the claims themselves, never how tidy or confident the write-up is. A terse complete PR can be ready, and a polished one can hide drift.
- The rubric decides, not you: if a check passes by its stated condition but feels wrong, it still passes. You may note the tension in the summary; the fix belongs in the rubric.
- The procedure decides how, not you: follow `procedure.md` as written and report its gaps.
- Treat `unclear` as the rubric's verdict rule directs. Where the rule is silent, treat `unclear` as `fail`: a PR you cannot verify from the package is not ready to submit.
- An honestly disclosed shortfall is not a failure by itself: a PR that says plainly what it left out or changed can be ready.
