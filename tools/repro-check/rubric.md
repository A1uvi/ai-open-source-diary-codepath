# Rubric: is this reproduction package ready to post?

Grade every check below, then apply the verdict rule. Read the whole package
before grading anything: the issue context first, then the claim comment,
then the repro report. The deciding evidence is almost always whether the
report's own artifacts, read literally, show the failure the issue describes
— not how long the report is, how confident it sounds, or whether it hits a
template's headings.

## Checks

| Check | Evidence | Pass condition | Weight |
| --- | --- | --- | --- |
| env-recorded | The repro report's stated environment, held against whatever the repo's own bug-report template asks for (repo-facts block: "bug reports:"). | Names, at minimum, the version of the software under test and the operating system, plus any other environment fact the repo's own template specifically asks for (a driver, a build profile, an installation method) when the issue's failure could plausibly depend on it. A version alone with no OS fails. No environment record at all fails, however good the rest of the report is. | required |
| steps-followable | The report's steps, held against the issue's own stated command/flags and the repo's template. | Steps name a starting state (fresh install, or an explicitly named existing one) and give commands or actions a stranger could execute in order, using the same command and flags the issue's trigger uses (an `-f <file>` stays `-f <file>` whatever the file holds; a `--driver`/`--cleanup` flag the issue's trigger includes is not dropped without saying so). This check is purely mechanical — could a stranger re-run exactly what's written down — and does not ask whether doing so actually reached the behavior the issue describes; that question belongs to `target-matches` and `deviation-disclosed` below, and an honest, precisely-documented attempt that turns out not to trigger the bug still passes here. Narrated summary instead of commands fails; an unstated starting point fails; substituting a different flag or subcommand than the issue's trigger (not its input's content, its actual command shape) without saying so fails, even when the resulting log looks plausible. | required |
| target-matches | The artifact(s) pasted in the report, read literally against the issue's stated failure (its error type, exit behavior, or stack signature — not just its title or area of the tool). | The artifact shows the same class of failure the issue reports (a panic stays a panic; a crash stays a crash), OR the report says plainly it could not reproduce that failure and shows what happened instead, including an honest guess at why not. Fails when the artifact shows a different failure than the one claimed and the report narrates it as a match anyway — confident and specific prose around the wrong artifact still fails this check. This is also where a constructed substitute input (a hand-written file standing in for one the issue links) earns its pass or fail: if the resulting artifact shows the same failure class the issue describes, the substitution worked; if it shows a different, milder, or absent failure, it didn't, whatever the report claims. | required |
| deviation-disclosed | The report's stated version and environment, held against the issue's stated target (its version, and any "confirmed on latest/main" line). | Any difference in version or environment that could plausibly change the result — an old pinned version tested against an issue confirmed on latest/main, a different OS or backend the issue calls out as relevant — is named in the report as a difference. This check is scoped to version and environment only; it does not re-litigate whether a substituted *input file* was a valid stand-in, which `target-matches` already judges by its result. Passes when there is no version/environment deviation, and passes an honest cannot-reproduce that names exactly what differed. Fails when a real version/environment deviation goes unmentioned, even if the artifact otherwise looks right. | required |
| comms-specific | The claim comment, and any claims the repro comment makes about itself. | Names the specific version and behavior at stake (not "this bug"), commits to a concrete next artifact or action rather than an outcome or timeline it doesn't control yet ("repro report on the way," not "fixed by Friday, guaranteed"), and reads like it was written by someone who actually looked at this issue rather than a template that would fit any of them. Fails on generic assign-me/hype comments, promised timelines or guarantees, and self-assigned claims with no stated next step. | required |
| ai-disclosure | The repo-facts block's stated contribution policy and any dedicated AI-policy line, held against the claim comment's and report's own text. | If the stated policy requires disclosing AI assistance, the comment(s) state that assistance explicitly. If the policy sets conditions short of a disclosure requirement (human review, understanding, testing), this check passes without a disclosure statement. If the repo states nothing about AI use, this check passes by default — silence passes. An outright ban on AI-generated contributions fails this check regardless of what the comments say. | required |
| control-tested | Whether the report includes a second, comparably-evidenced run against a deliberately different condition (an earlier version, a value just outside the trigger range, a different locale/config) that isolates the failure rather than just asserting it. | A control run is present and its actual result is shown, not merely claimed. | preferred |

## Verdict rule

Accept only if every `required` check passes. `unclear` counts as `fail` on
every check: a fact the package's own text doesn't let me decide is not
evidence for either side. `preferred` checks never change the verdict —
`control-tested` only ranks packages that already pass every required check.

An evidenced "could not reproduce" is graded through exactly the same six
required checks as an evidenced "reproduced," and scores the same when it
passes them. A report that tried, failed, and names precisely what it tried
and how that differed from the issue's conditions can accept outright; a
report that claims a match while its own artifact shows something else, or
that quietly tests different conditions than the issue targets, rejects on
`target-matches` or `deviation-disclosed` however complete and confident it
reads. Confidence is not a check; nothing in this rubric grades it.

## Calibration notes

Not graded by the harness; kept here so the reasoning behind each check
survives to the write-up. Built and checked against the four calibration
packages (`calib-01`–`calib-04`), which are never scored, plus specific
scored packages where they forced a real decision.

- **`steps-followable` and `deviation-disclosed` went through two revisions
  after the first full run (19/20, missing `pkg-05`), and the second
  revision is the one worth reading closely, because it fixed a mistake
  the first one made.** `pkg-05`'s report builds its own minimal `env.yml`
  with an unrecognized section instead of fetching the issue's linked
  conda-lock URL, and the first version of both checks rejected it as an
  undisclosed deviation — measuring "is the input literally the issue's"
  rather than "does a stand-in still trigger the same rule." First fix:
  I rewrote both checks to ask whether a constructed substitute "visibly
  satisfies the issue's specific rule," re-ran `pkg-05` alone, got `accept`,
  and ran `calib-03`/`pkg-16`/`pkg-06` as canaries — all three still
  correctly rejected, so I moved straight to a confirming full run. That
  run came back 19/20 again, but on a *different* miss: `pkg-09`, an honest
  cannot-reproduce report that runs a large, carefully-documented attempt to
  hit an argument-size race and states plainly it didn't land, naming
  exactly what might differ. My "visibly satisfies the rule" wording had
  put `steps-followable` in the business of judging whether the attempt
  *worked*, and an honest attempt that doesn't land can't "visibly satisfy"
  anything — so it failed a check that was never supposed to grade outcome.
  I hadn't re-run an honest-cannot-reproduce canary alongside the first fix,
  and that's exactly the gap it needed. Second fix, and the one that
  stands: `steps-followable` went back to being purely mechanical — same
  command and flags as the issue's trigger, followable in order, regardless
  of whether following them actually reaches the bug — and the question of
  whether a substitute input worked moved entirely into `target-matches`,
  which already reads the resulting artifact against the issue's claimed
  failure and has no trouble passing an honest "I tried, and here's what I
  got instead." `deviation-disclosed` narrowed to version/environment gaps
  only, since input-substitution quality is `target-matches`'s job.
- **`calib-01` anchors "looks rough, is fine."** Its report is terse — four
  steps, one artifact, a plain expected/actual pair — and passes every
  required check anyway, because none of them asks for length, structure, or
  polish. `env-recorded` and `steps-followable` ask only whether the needed
  facts are present, never how they're formatted.
- **`calib-02` anchors the floor.** An emphatic me-too with no environment,
  no steps, and a cause "verified" from nothing fails `env-recorded`,
  `steps-followable`, and `target-matches` simultaneously. That's expected,
  not double-counting: the verdict rule needs only one required fail to
  reject, and a package this empty was always going to fail several checks
  at once. `pkg-04` and `pkg-13` are the scored set's versions of the same
  shape.
- **`calib-03` is the check that shaped `target-matches`'s wording, and the
  one I'd flag as the eval's real trap.** The report is long, specific, and
  confident — it states expected vs. actual, claims ten identical runs, and
  claims a cross-checked second install. But its own "Preparation" step
  creates the input file with a colon instead of the issue's `=`, so the
  artifact it pastes is a graceful "Missing key/value separator" parse
  error, not the issue's Go panic with a goroutine trace. A pass condition
  that only checks "are expected and actual both stated" would accept this
  package, since both are stated — just wrongly. `target-matches` is written
  to require literally comparing the artifact's failure class against the
  issue's, which is the only thing that catches this. `pkg-02`, `pkg-08`,
  and `pkg-17` are scored packages with the same shape: a real command was
  run, and it produced a real but different failure than the one claimed.
- **`calib-04` is the genuinely defensible one, and the reason `env-recorded`
  stays required with no exception.** The repro itself is strong — exact
  command, the panic shown, a control run at a shifted value — and the only
  thing missing is any environment line at all. The repo-facts block for
  this issue states that debug and release builds fail differently (panic
  vs. silent wraparound), which makes "what kind of build was this" the one
  fact a stranger can't infer and can't check independently. A rubric that
  let a strong repro pass without an environment record would also pass a
  package on an issue where that omitted fact happens to be the one that
  matters, and there's no way to write a check that only applies when the
  omission is consequential. Reject.
- **`pkg-16` is why `deviation-disclosed` is its own check and not folded
  into `target-matches`.** The artifact here isn't adjacent-but-wrong the
  way `calib-03`'s is — it's a real `ValueError` from a real run — but the
  run was on pandas 1.5.3 against an issue the reporter confirmed on latest
  and main, and the report calls this "the crash the issue describes,
  confirmed" without ever saying the version differs. `target-matches` alone
  would plausibly pass this (the artifact does show a failure at the named
  call site); `deviation-disclosed` is what catches the silent substitution.
- **`pkg-19` is why `comms-specific` reads only the claim comment, separate
  from the report.** The repro report on that package is genuinely solid —
  env, steps, artifact, a confirming control. The claim comment is the
  problem: "I will fix it within 2 days guaranteed," addressed to "sir,"
  closer to a form letter than an investigation. If `comms-specific` graded
  the package as a whole, a strong report could carry a bad claim comment
  past this check; reading the claim comment on its own is what makes the
  rejection land on the actual defect.
- **`pkg-07` and `pkg-20` are the pair `ai-disclosure` is calibrated
  against, in opposite directions.** `pkg-07`'s claim comment states outright
  that AI helped organize the report and that the poster ran and understood
  every step, meeting p5.js's assistive-use-with-responsibility policy.
  `pkg-20`'s repro is arguably the strongest in the whole set — a two-config
  control run that isolates the exact code path — and still rejects, because
  ghostty's policy requires disclosing all AI use and nothing in either
  comment discloses it. Keeping this as its own required check, separate
  from `comms-specific`'s tone-based reading, is what lets a single missing
  sentence override an otherwise perfect package.
