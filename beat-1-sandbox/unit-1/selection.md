# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72

**Verdict output**

````
All three are in scope (`codepath/pathreview-ai301-fa26-s1`). Repo-level evidence, shared by all three: `archived: no`, 0 stars, no releases ever published, last default-branch commits 2026-09-16 (3 commits) and 2026-08-24, all by `Aburke225` (human, pushes to `main`). `docs/CONTRIBUTING.md` and the PR template say nothing about AI-generated code — they require green CI, conventional commits, and removal of the seeded `@pytest.mark.xfail` marker. The repo has **zero** pull requests (open or closed), and every issue in the tracker was opened by the maintainer, so there is no non-maintainer thread that could demonstrate response latency.

**All three pass every required check → all three accept.** Preferred checks are identical across them (`responds-to-issues` unclear→fail, `in-real-use` fail at 0 stars/no package, `no-abandoned-attempts` pass), so ranking is entirely fit.

**Ranked for your profile (back-end Python, no front-end):**

1. **#72 — `verify_password` raises `UnknownHashError`** (`core/security.py`). Pure back-end Python, and the fullest spec of the three: names both relevant files, the manifest id (H-05) so you know exactly which xfail marker to delete, and a 1–2h estimate. Fail-closed auth behavior is squarely the kind of back-end work you're targeting, and nobody else has touched it.
2. **#61 — health probe passes raw SQL** (`api/routes/health.py`). Smallest diff of the three (wrap `"SELECT 1"` in `sqlalchemy.text()`) and on-profile for FastAPI + SQL. Ranked below #72 only because its body omits the relevant-files list and manifest id that the other two carry, so you'd have to locate the covering test yourself — and CONTRIBUTING.md notes `api/routes/health.py` also carries a mypy suppression belonging to a *different* seeded issue (#62), so the file has a neighboring trap to leave alone.
3. **#68 — `ZeroDivisionError` on empty keyword index** (`rag/retriever/keyword_search.py`). Well-specified (manifest id H-01) and closest to your AI-application interest, but the largest at 2–4h, and classmate `yulijasso` pushed a commit referencing it on 2026-09-16 in their own coursework repo. That does not block it — the Path Review house rule says classmates' claims don't, and there's no PR against this repo — but it's shared ground.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "gh repo view: \"isArchived\": false"},
      {"name": "commits-alive", "grade": "pass", "evidence": "3 commits by human Aburke225 on 2026-09-16, plus 2 on 2026-08-24 — all within 90 days"},
      {"name": "shipped-recently", "grade": "pass", "evidence": "latestRelease: null (none published); newest default-branch commit 2026-09-16, within 60 days"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI restriction; conditions are green CI and xfail-marker removal"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], closedByPullRequestsReferences: [], comments: []; repo has zero PRs; timeline shows only 4 label events"},
      {"name": "bounded-change", "grade": "pass", "evidence": "One deliverable: make verify_password return False on malformed hash and drop the H-05 xfail; 'Estimated effort: 1-2 hours'"},
      {"name": "spec-present", "grade": "pass", "evidence": "Expected vs actual stated: 'Verification against a malformed hash should fail closed (return `False`), not raise'"},
      {"name": "responds-to-issues", "grade": "unclear", "evidence": "Every issue in the repo was opened by maintainer Aburke225, so no non-maintainer thread exists to sample; no maintainer comment in this thread"},
      {"name": "in-real-use", "grade": "fail", "evidence": "stargazerCount: 0; no published release or package"},
      {"name": "no-abandoned-attempts", "grade": "pass", "evidence": "gh pr list --state all returns no PRs in the repo at all"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/61",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "gh repo view: \"isArchived\": false"},
      {"name": "commits-alive", "grade": "pass", "evidence": "3 commits by human Aburke225 on 2026-09-16, plus 2 on 2026-08-24 — all within 90 days"},
      {"name": "shipped-recently", "grade": "pass", "evidence": "latestRelease: null (none published); newest default-branch commit 2026-09-16, within 60 days"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI restriction; conditions are green CI and xfail-marker removal"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], closedByPullRequestsReferences: [], comments: []; timeline shows only 4 label events by Aburke225"},
      {"name": "bounded-change", "grade": "pass", "evidence": "One deliverable: wrap the literal \"SELECT 1\" in sqlalchemy.text() in api/routes/health.py"},
      {"name": "spec-present", "grade": "pass", "evidence": "Cause named plus repro: 'SQLAlchemy 2.x requires textual SQL to be wrapped in sqlalchemy.text(), so the probe raises' with the exact ArgumentError"},
      {"name": "responds-to-issues", "grade": "unclear", "evidence": "Every issue in the repo was opened by maintainer Aburke225, so no non-maintainer thread exists to sample; no maintainer comment in this thread"},
      {"name": "in-real-use", "grade": "fail", "evidence": "stargazerCount: 0; no published release or package"},
      {"name": "no-abandoned-attempts", "grade": "pass", "evidence": "gh pr list --state all returns no PRs in the repo at all"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "gh repo view: \"isArchived\": false"},
      {"name": "commits-alive", "grade": "pass", "evidence": "3 commits by human Aburke225 on 2026-09-16, plus 2 on 2026-08-24 — all within 90 days"},
      {"name": "shipped-recently", "grade": "pass", "evidence": "latestRelease: null (none published); newest default-branch commit 2026-09-16, within 60 days"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI restriction; conditions are green CI and xfail-marker removal"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], no linked PRs, comments: []; only a 'referenced' event from classmate yulijasso's own repo commit (2026-09-16), which is neither an open PR nor a blocking claim under the Path Review house rule"},
      {"name": "bounded-change", "grade": "pass", "evidence": "One deliverable: make KeywordSearcher.index([]) not raise and drop the H-01 xfail; 'Estimated effort: 2-4 hours'"},
      {"name": "spec-present", "grade": "pass", "evidence": "Expected vs actual stated: 'search() handles the empty case and returns an empty list, and index() shouldn't raise on an empty corpus either'"},
      {"name": "responds-to-issues", "grade": "unclear", "evidence": "Every issue in the repo was opened by maintainer Aburke225, so no non-maintainer thread exists to sample; no maintainer comment in this thread"},
      {"name": "in-real-use", "grade": "fail", "evidence": "stargazerCount: 0; no published release or package"},
      {"name": "no-abandoned-attempts", "grade": "pass", "evidence": "gh pr list --state all returns no PRs in the repo at all"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

**Run history**

2/3 (smoke run, `--limit 3`), then 17/20 (first full run), then 20/20 (confirming
full run). The 20/20 is the run committed as `eval-run.txt`.

**Issue analysis**

`issue-01` (conda/conda#16475, "Add permanent docs for installing PyPI packages
with `conda install`"). Gold label: `accept`. My rubric's first version decided
`reject`, failing it on `bounded-change` with the evidence line "Issue lists 5
sub-item changes (new page, manage-pkgs.rst, pip-interoperability.rst,
new-features.md, troubleshooting.rst) as a checklist, matching the umbrella-issue
failure condition", and grading `spec-present` `unclear` because the page title was
given only "for example" and the troubleshooting entry was marked "lower priority".
The reasoning that produced that result was my own failure condition: I had written
`bounded-change` to fail any issue whose body enumerated sub-items, on the theory
that a list of parts means a tracking issue. In this eval set the opposite is true —
the enumerated file list *is* the specification, and the issue is one docs
deliverable with its parts named. After the rewrite, my rubric decides `accept` and
matches gold.

**Check rationale**

The check as currently written in `tools/issue-select/rubric.md`:

> | bounded-change | the issue body and the full comment thread, including how the
> issue describes itself and what its linked items are | the issue asks for one
> deliverable. Fail only if the issue calls itself an umbrella, tracking, or mega
> issue, or its sub-items are separate linked issues or PRs meant to be taken
> independently; the scope is open-ended with no enumerable end ("anywhere in the
> codebase", "PRs welcome both big and small"); a maintainer says the fix touches
> core internals; or it is a usage question rather than a change. An enumerated list
> of the files, pages, or steps that make up one deliverable is a specification, not
> an umbrella, and passes, as do extras the issue itself marks optional, "consider",
> or lower priority | required |

It is in this form because its first form measured the wrong thing. Keying the
failure on "lists sub-items" cost three of the eight clear-accepts (`issue-01`,
`issue-04`, `issue-19`) while passing `issue-15` and `issue-20`, the two issues the
answer key rejects on scope — it was rejecting good issues and clearing bad ones at
the same time. The signal that actually separates them is not how many parts an
issue names but whether its scope has an end someone can enumerate: `issue-10` calls
itself a "megaissue" and links over 100 sub-issues, and `issue-05` says "PRs are
welcome both big and small... not necessarily anywhere in the codebase". So the
check now reads the issue's self-description and the open-endedness of its scope,
and says explicitly that an enumerated list of one deliverable's parts passes.

**Trade-offs**

It gives up carrying the scope family by itself. In its current form
`bounded-change` passes both `issue-15` and `issue-20`, so every scope rejection now
rests on `spec-present` (`issue-20`: "the delivery mechanism itself is undecided")
or on the liveness checks (`issue-05` and `issue-10` also fail `shipped-recently`).
A large change that is enumerated part by part and never calls itself an umbrella
will now be accepted, and that is a case I accept it will miss.

I confirmed the change with a canary: after the rewrite I re-ran
`--only issue-01,issue-04,issue-19`, which returned 3/3, and the grader's evidence
cited the new distinction rather than a lucky guess ("marks the troubleshooting.rst
entry 'Lower priority' as optional, not a separate umbrella item"). The confirming
full run then went 20/20 with all five categories matched, so nothing else moved.

---

## Selection rationale

**Selection rationale**

1. Fit to my interests and to the time available. #72 is back-end Python in
   `core/security.py` — password-hash verification that should fail closed instead of
   raising. That is the kind of back-end work I am aiming at for next year's
   internship, it is in a language I have actually written real code in, and it has
   no front-end surface. The issue estimates 1–2 hours, the smallest of the three I
   graded, which fits the week I have alongside my internship.

2. What the verdict identified correctly, and what I weighed that the rubric could
   not. The verdict was right that the repo is alive and the issue is free: commits
   from `Aburke225` on 2026-09-16, no assignee, no linked PRs, and no PRs in the
   repository at all. It was also right that `in-real-use` fails — 0 stars, no
   release — and correctly treated that as a ranking signal rather than a rejection,
   which is the whole point of a classroom repo. What the rubric could not weigh is
   what the spec hands me as a newcomer: #72 names both files to touch and the
   manifest id (H-05) of the `@pytest.mark.xfail` marker to remove, so I know where
   the covering test is before I start. #61 is a smaller diff but omits that, and
   `api/routes/health.py` carries a mypy suppression belonging to a different seeded
   issue, so there is a neighboring trap. #68 is closest to my AI-application
   interest but is the largest at 2–4 hours, and a classmate has already referenced
   it from their own repo.

3. Anticipated difficulty in claiming it. Low. The Path Review house rule says
   classmates' claim comments do not block an issue, and #72 has no comments, no
   assignee, and no referencing commits from anyone. The real difficulty is not the
   claim but the hand-off: CONTRIBUTING requires green CI, conventional commits, and
   removal of the seeded xfail marker, so the fix has to include deleting that marker
   and leaving the suite green — a passing test is part of the deliverable, not an
   extra.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
