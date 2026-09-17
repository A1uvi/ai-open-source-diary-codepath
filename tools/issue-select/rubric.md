# Rubric: is this a good first issue?

Grade every check below, then apply the verdict rule. All recency windows are
measured against the capture date stamped on the bundle's repo-facts block in
eval mode, and against today in live mode.

Terms used by the pass conditions:

- **Maintainer**: an account whose comment carries an Owner, Member, or
  Collaborator badge (`author_association` in a bundle), or an account that has
  merged a pull request in the repo within the last 12 months.
- **Human commit**: a commit whose author is not a bot (no `[bot]` suffix, and
  not dependabot, renovate, github-actions, or pre-commit-ci). A bot commit
  that merges a human's pull request counts as human.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| not-archived | the `archived:` flag on the repo line of the repo-facts block; on github.com, the archived banner across the top of the repo page | `archived: no`. An archived repo is read-only and cannot accept a pull request at all | required |
| commits-alive | the "last 5 default-branch commits" list in the repo-facts block, with author and date per commit | at least 2 human commits dated within the last 90 days | required |
| shipped-recently | the "latest release" line in the repo-facts block; if it reads `none published`, the newest date in the "last 5 default-branch commits" list | a release dated within the last 180 days; for a repo that has never published a release, a default-branch commit within the last 60 days | required |
| policy-allows-ai | the "contribution policy" line under Repo facts, which quotes CONTRIBUTING.md and any contributor docs it links; on github.com, `CONTRIBUTING.md` in the root or `.github/`, plus any `AI_POLICY.md` and the PR template | no outright ban on AI-generated code or documentation. Silence passes; conditions such as disclosure, human review, personal understanding, or testing pass and are terms to follow, not reasons to walk away | required |
| unclaimed | the `this issue: assignees:` and `linked PRs:` fields in the repo-facts block, plus every comment in the thread | all three hold: no assignee; no open linked PR, and no open PR for this issue mentioned in the thread; no comment claiming the issue ("I'll take this", "working on this") within the last 90 days. A closed unmerged PR does not block. A stale claim no maintainer answered does not block | required |
| bounded-change | the issue body and the full comment thread, including how the issue describes itself and what its linked items are | the issue asks for one deliverable. Fail only if the issue calls itself an umbrella, tracking, or mega issue, or its sub-items are separate linked issues or PRs meant to be taken independently; the scope is open-ended with no enumerable end ("anywhere in the codebase", "PRs welcome both big and small"); a maintainer says the fix touches core internals; or it is a usage question rather than a change. An enumerated list of the files, pages, or steps that make up one deliverable is a specification, not an umbrella, and passes, as do extras the issue itself marks optional, "consider", or lower priority | required |
| spec-present | the issue body, its labels, the opener's `author_association`, and any maintainer reply | the deliverable is determinable without anyone having to decide it first: expected versus actual behavior, an acceptance criterion, a maintainer-named cause, or content spelled out in enough detail to start work, whoever filed it and whether or not a maintainer has replied. Items the issue marks optional, and cosmetic details left open (an example page title, a badge, an asset "TBD"), do not make a specified deliverable unspecified. Fail when the core of the work is a wish whose behavior or mechanism someone still has to settle | required |
| responds-to-issues | the "maintainer first-response sample" in the repo-facts block, and the badges in this issue's thread | at least one sampled thread not opened by a maintainer drew a maintainer reply within 30 days, or a maintainer has commented in this issue's thread | preferred |
| in-real-use | the star count on the repo line; on github.com, the "Used by" counter and the package page | 300 or more stars, or a published package with downloads in the last 30 days, or 10 or more dependent repositories | preferred |
| no-abandoned-attempts | closed pull requests named in the `linked PRs:` field or in the thread | no closed unmerged PR for this issue, or a closing comment that gives a reason unrelated to the approach | preferred |

## Verdict rule

Accept only if every `required` check passes. `Preferred` checks never change
the verdict; they rank the issues that are accepted. `Unclear` counts as
`fail` on a required check and as `fail` on a preferred one: a first issue
whose liveness, claim state, or scope cannot be verified from the evidence in
front of me is not one to take.

## Calibration notes

Not graded by the harness; kept here so the reasoning behind each threshold
survives to the write-up.

- **`responds-to-issues` is preferred, not required, and this is the rubric's
  biggest bet.** The lecture's dead repo was caught by unanswered threads, but
  a response-rate threshold also fails repos that are unmistakably alive: a
  project whose recent issues were all opened by its own maintainers has
  nobody to reply to, and a large project can leave four of five threads
  silent while merging pull requests daily. `commits-alive`, `not-archived`,
  and `shipped-recently` carry the liveness verdict instead, and they catch
  the stopped-shipping trap on their own. What this gives up: a repo that
  commits briskly and never answers contributors is accepted, and the cost of
  that shows up later as an unreviewed pull request.
- **`unclaimed` needs all three conditions** because each fails alone: an
  unassigned issue can still have an open PR against it, and a claim comment
  is a claim whether or not anyone set the field. The 90-day staleness line
  keeps a years-old "I'll take this!" from locking an issue forever while
  still catching a fresh race. In live mode the Path Review house rule in
  `scope.md` overrides the claim-comment condition for classmates.
- **`bounded-change` was rewritten after the first full run (17/20).** Its
  first form failed any issue that enumerated sub-items, and in this eval set
  enumeration is what a well-specified issue looks like: it rejected all
  three clear-accepts it touched (issue-01's five docs files, issue-04's
  "remove identity, fuse spiders, ... etc.", issue-19's two causes plus three
  suggestions) while passing issue-15 and issue-20, the two the answer key
  rejects on scope. Its only true catches, issue-05 and issue-10, also fail
  `shipped-recently` and `spec-present` on their own, so the check was
  costing points and earning none. It now keys on how the issue describes
  itself and whether scope has an enumerable end, not on list length.
- **`spec-present` is the arguable one.** Written tighter it rejects terse
  maintainer-filed bugs that are perfectly takeable, and it graded issue-01
  `unclear` on an example page title and a "lower priority" extra; written
  looser it accepts one-line wishes that hide a product decision. The line
  now drawn is whether the *core* deliverable can be determined without
  someone deciding it first, with optional extras and cosmetic blanks
  explicitly not counting against it. issue-20 still fails: its delivery
  mechanism is what is undecided.
- **`unclaimed` has a known soft spot.** On issue-15 it graded `unclear`
  because the bundle showed 40 of 97 comments, which produced the right
  verdict for the wrong reason: that issue's reject rests on nothing else
  required. A rule for truncated threads is the next thing to write.
- **Thresholds most likely to move after an eval run:** the 90 days and 2
  commits in `commits-alive`, the 180 days in `shipped-recently`, the 90-day
  staleness line in `unclaimed`, and the boundary in `spec-present`.
