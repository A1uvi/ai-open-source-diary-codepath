# Evidence guide: where proof lives in a reproduction package

This is the rubric's map: for every check in `rubric.md`, where to find its
evidence in a package, and what a sufficient answer looks like once you're
looking at the right place.

In an eval bundle, a package is always four parts in this order: the issue
context (title, body, labels, thread highlights), a repo-facts block
(stars, latest release, the repo's own stated bug-report template, and its
contribution/AI policy), a candidate claim comment, and a candidate repro
report. Live mode has the same four parts, just not frozen in one file: the
issue lives on GitHub, the repo-facts equivalents live in the repo's own
docs, and the claim/report are the student's drafts.

## Environment

**Where it lives.** The repro report's own stated environment line(s) —
usually a labelled "Environment" section near the top of the report. What
the environment needs to cover is set by the repo-facts block's "bug
reports:" line, which states exactly what that repo's own template asks
reporters for (version, OS, browser, driver, installation method, build
profile). In live mode, the equivalent is the repo's issue template
(`.github/ISSUE_TEMPLATE/`) or its bug-report section in `CONTRIBUTING.md`.

**What good looks like.** At minimum: the version of the software under
test and the operating system. Beyond that, whatever the repo's own
template names specifically — if the template asks for a driver and the
report doesn't give one, that is a gap the template itself flags, not an
optional extra. A version number with nothing else ("running the latest")
is not an environment record; neither is silence.

## Steps

**Where it lives.** The report's own steps section, read next to the
issue's stated reproduction command or linked example. Some issues give an
exact command (`hyperfine --parameter-scan x 2147483646 2147483647 'true'`);
others give a numbered list (`minikube start --driver vmware`, then
`minikube tunnel --cleanup --alsologtostderr`); others link an external
reproduction (an SFC playground link). The comparison is always: what did
the issue say to run, and what did the report actually run.

**What good looks like.** A stated starting point (a fresh clone/install,
or an explicitly named existing state) followed by commands or actions in
order, ending in the same triggering action the issue names — same flags,
same command shape, same kind of input. When the report changes something
from the issue's exact trigger (drops a flag, hand-types an input instead
of using a linked one), the change has to be named; a step list that just
quietly omits part of the issue's own reproduction (a `--driver` flag on a
driver-specific bug, a `--cleanup` flag the issue includes) is not
followable, whatever its output looks like. A narrated summary of what
happened ("I ran it and it crashed") is not steps.

## Behavior shown

**Where it lives.** The artifacts the report actually pastes — a terminal
transcript, a stack trace, a rendered CSS block, a console log — read
against the issue's own stated failure: not its title or its subject area,
but the specific mechanism (a panic with a stack trace vs. a validation
error; a silently-dropped flag vs. an explicit error message; a crash vs. a
merely-wrong value).

**What good looks like.** The pasted artifact has to be the same *class* of
failure as the issue's, not merely present. A confident, detailed writeup
built around the wrong artifact is worse evidence than a short one built
around the right one — read the artifact's own text first, and only then
read the prose that surrounds it. An artifact that is honestly a
cannot-reproduce (a clean run where the issue expects a failure) is
sufficient here as long as the report says so plainly and shows what it
got instead; it does not need to show the failure to count as evidence.

## Honesty

**Where it lives.** The gap between what the report says it tested and
what the issue actually targets: version numbers, "confirmed on
latest/main" checkboxes in the issue, linked reproductions versus
hand-recreated ones, the reporter's stated environment versus the report's.

**What good looks like.** Any real difference between the tested
conditions and the issue's target is named in the report, in the report's
own words — not left for a reader to notice by comparing dates or version
strings themselves. A report that tests an old pinned version against an
issue confirmed on latest, or substitutes a hand-typed reproduction for a
linked one, and says nothing about it, is claiming more than its evidence
shows even if every individual sentence in it is true. The line that
separates a passing cannot-reproduce from a failing one is exactly this:
did the report name what was different, or did it let "I tried and it
didn't happen" stand in silently for "I tried something different and it
didn't happen."

## Comms

**Where it lives.** Two separate things, read separately: the claim
comment's own words, and the repo-facts block's stated contribution policy
and AI-policy line, held against whatever the claim comment or report says
about how it was produced.

**What specific-and-honest looks like, for the claim comment itself.** It
names the version and the specific behavior at stake — something that
would not also fit a different issue in the same repo. It commits to a
next artifact or a next action ("repro report on the way," "next I want to
check X"), not to an outcome or a deadline the poster doesn't control yet.
It reads like someone who looked at this issue, not a phrase reusable
anywhere: "please assign me," "amazing project," a guaranteed timeline, and
a self-assignment with no stated next step are all the same failure —
enthusiasm standing in for investigation.

**What it means for AI-use policy specifically.** Read the repo-facts
block's contribution-policy line for any AI-specific language:

- **A requirement to disclose** ("all AI usage must be disclosed," stating
  the tool and the extent of assistance) is met only when the claim comment
  or report actually says AI was used and that the poster understands and
  verified the work. Nothing else in the package satisfies this — a
  technically excellent report with no such sentence still fails.
- **A condition short of a disclosure requirement** (assistive use allowed
  if the contributor understands and takes responsibility; comments
  expected in the contributor's own words; AI help with grammar/proofreading
  explicitly fine) is a term to meet through the quality of what's posted,
  not a sentence to produce — no disclosure statement is needed to pass.
- **Silence** (no stated AI policy at all, the common case) passes without
  comment.
- **An outright ban** on AI-generated contributions fails regardless of
  what the comments say, however good the repro is.
