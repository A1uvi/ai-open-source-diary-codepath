# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/213

**Branch**

fix/72-verify-password-malformed-hash

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 (full): 14/20. Run 2 (full): 20/20. The last score matches the agreement line in my eval-run.txt: agreement: 20/20 scored items (bar: 18/20: PASS).

Run 1 missed 6 clear-accepts (pkg-05, 08, 11, 13, 16, 19), so my rubric was too strict. It caught every reject, including both standards-wall packages. I loosened three checks (repo-checks-run, repro-rerun-before-after, description-matches-diff) and ran the full eval again. I did not run any --only re-runs in between.

**Package analysis**

Package: pkg-05 (nushell keybinding merge). Gold label: accept. In run 1 my rubric said reject, and it failed repro-rerun-before-after and repo-checks-run.
The test plan named two scenarios: same-name bindings on different keys, and a warning when the same key is redefined. The evidence showed a before and after for the first one and only stated the warning in prose, so my rubric read that as a missing failure mode. It also read "cargo test -p nu-protocol passes (312 tests); fmt and clippy clean" as a bare claim with no pasted output.
I changed the rubric so that a secondary scenario passes if the evidence states its observed result, and so that a named command with a pass count counts. In run 2 my rubric said accept, which agrees with gold. The issue's own repro has a clear before and after, so I think accept is right.

**Check rationale**

repo-checks-run | The test evidence, read against the commands the repo's PR template, contributing guide, or repo-facts block ask contributors to run (tests, lint, typecheck, build). | The evidence names the repo's own test command or suite and states its outcome (a named command with a pass count or result counts; pasted output is better), or a check that could not run is named with the reason (an honest failure or an unrunnable check disclosed counts). Fails when no repo check is mentioned at all, or the outcome is a bare unnamed assertion ("tests pass", "all green", "works locally") with no command. Not every command the repo lists must appear: the test suite the change touches is the one that matters, and a lint or format step the evidence does not show does not fail this check. | required |

My first version required output for every command the repo listed and treated a ticked box with no output as a failure. That rejected five clear-accepts in run 1, for example pkg-08 over go generate and the formatter. I changed it to require the test command with its outcome. I kept the failure for "tests pass" with no command, because pkg-04's "cargo test passes" evidence is the case this check should catch. It also still accepts an honestly disclosed check that couldn't run, which is what my own PR does for make test-integration.

**Trade-offs**

repo-checks-run now gives up strictness about lint and format. A PR that shows a passing test suite but never shows lint or format output passes this check, even if the template asks for lint. I accept that because the template-and-policy check covers the template's other asks, and a missed format step is cheap for a reviewer to catch.

Loosening it could flip a reject that depended on it. I did not run canaries with --only after the change. I ran the full set, and the not-tested rejects (pkg-04, 07, 10, 14) and the standards-wall rejects (pkg-01, 20) all still agreed. Those rejects don't depend on this check, because each fails on the repro or the standards check.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
