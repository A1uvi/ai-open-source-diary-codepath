# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

A1uvi

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5804875898

Picking this up as my first contribution here. I'll reproduce `verify_password` letting `passlib.exc.UnknownHashError` escape on a malformed stored hash instead of failing closed, then post a repro report with my environment, exact steps, and the log. Once that's up I'll look at removing the `xfail` marker on `test_verify_with_wrong_hash_format` (manifest H-05) as part of the fix.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5804907465

Reproduced on my own fork, from a clean venv (no Docker/`.env` needed — every `Settings()` field in `core/config.py` has a default, so `core/security.py` runs in isolation).

**Environment:** macOS 27.0 (arm64), Python 3.12.7, `A1uvi/pathreview-ai301-fa26-s1` at commit `f89c06f`. Installed: `passlib[bcrypt]>=1.7.4`, `bcrypt>=4.0.1,<5.0.0`, `python-jose[cryptography]>=3.3.0`, `pydantic[email]>=2.5.0`, `pydantic-settings>=2.1.0` — resolved to passlib 1.7.4, bcrypt 4.3.0, pydantic 2.13.5.

**Baseline, to confirm the function works at all before breaking it:**

```
$ python3 -c "
from core.security import verify_password, hash_password
h = hash_password('password')
print(verify_password('password', h))
"
True
```

**The reported trigger**, the exact malformed-hash string `test_verify_with_wrong_hash_format` uses:

```
$ python3 -c "from core.security import verify_password; verify_password('password', 'not_a_valid_bcrypt_hash')"
Traceback (most recent call last):
  ...
passlib.exc.UnknownHashError: hash could not be identified
```

**Expected:** `False`, fail-closed, same as a wrong-but-well-formed password. **Actual:** the `UnknownHashError` above propagates straight out of `verify_password`.

**One thing worth adding beyond the issue's own repro:** the failure isn't specific to that one string. I tried a couple of other malformed hashes to see whether the fix needs to handle more than a single exception type:

```
$ python3 -c "
from core.security import verify_password
for bad in ['', 'plaintext', '\$2b\$notarealhash']:
    try:
        print(repr(bad), '->', verify_password('password', bad))
    except Exception as e:
        print(repr(bad), '-> raised', type(e).__module__ + '.' + type(e).__name__, str(e))
"
'' -> raised passlib.exc.UnknownHashError hash could not be identified
'plaintext' -> raised passlib.exc.UnknownHashError hash could not be identified
'$2b$notarealhash' -> raised builtins.ValueError not enough values to unpack (expected 2, got 1)
```

So an empty string and plain text both raise the same `UnknownHashError` the issue names, but a string that merely *looks* bcrypt-prefixed and is malformed raises a bare `ValueError` from lower in passlib's parsing instead. A fix that only catches `UnknownHashError` would still let this second case through — worth keeping in mind once I get to the fix in Unit 3.

Also ran the repo's own covering test directly, to confirm this matches what the codebase already expects:

```
$ python3 -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL
```

That's the `@pytest.mark.xfail(strict=True, reason="issue #72 (manifest H-05): ...")` marker already sitting on the test, confirming this is the documented, expected-to-fail case, not an environment fluke on my end.

(Separately: passlib 1.7.4 paired with bcrypt 4.3.0 prints a `(trapped) error reading bcrypt version` warning on every single call here, control run included — a known compatibility quirk between those two library versions, unrelated to this bug. Noting it so it doesn't get mistaken for part of the failure if someone else sees it in their own run.)

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

4/4 (calibration warm-up, `--include-calibration --only calib-01,calib-02,calib-03,calib-04`,
unscored), then 19/20 (first full run, missed `pkg-05`), then 3/3 (`--only
pkg-05,pkg-06,pkg-16` plus unscored `calib-03` as canaries, after revising `steps-followable`
and `deviation-disclosed` for the first time), then 19/20 (confirming full run — `pkg-05` now
correct, but a new miss appeared on `pkg-09`), then 5/5 (`--only
pkg-05,pkg-09,pkg-06,pkg-02,pkg-16` plus unscored `calib-03` as canaries, after a second,
narrower revision), then **20/20** (confirming full run, saved as `eval-run.txt`).

**Package analysis**

`pkg-05` (conda/conda#16543). Gold label: `accept`. My rubric's first version decided `reject`,
failing it on `steps-followable` and `deviation-disclosed` with the evidence line "report
silently substitutes a hand-written minimal env.yml with no statement that this differs from
the linked file." The issue links a specific conda-lock-generated environment file; the
candidate report instead builds its own three-line `env.yml` containing an unrecognized
`category:` section. My first-pass checks treated any input that wasn't literally the issue's
own linked file as an undisclosed deviation. That's the wrong test: the report shows exactly
what it built, and what it built triggers the identical rule the issue is about (an environment
section conda doesn't recognize, producing `EnvironmentSectionNotValid` on stdout ahead of the
JSON, which breaks `--json` parsing) — a textbook minimal reproduction, not a substitution that
changes what's being tested. After rewriting the two checks to ask whether a constructed input
still satisfies the issue's actual rule (later narrowed further, see Check rationale), my
rubric now reads this as `accept`, matching gold: `env-recorded`, `steps-followable`,
`target-matches`, `deviation-disclosed`, `comms-specific`, and `ai-disclosure` all pass; only
the preferred `control-tested` fails, since no second run at a different condition is shown, and
a preferred check never moves the verdict.

**Check rationale**

The check as currently written in `tools/repro-check/rubric.md`:

> | deviation-disclosed | The report's stated version and environment, held against the issue's
> stated target (its version, and any "confirmed on latest/main" line). | Any difference in
> version or environment that could plausibly change the result — an old pinned version tested
> against an issue confirmed on latest/main, a different OS or backend the issue calls out as
> relevant — is named in the report as a difference. This check is scoped to version and
> environment only; it does not re-litigate whether a substituted *input file* was a valid
> stand-in, which `target-matches` already judges by its result. Passes when there is no
> version/environment deviation, and passes an honest cannot-reproduce that names exactly what
> differed. Fails when a real version/environment deviation goes unmentioned, even if the
> artifact otherwise looks right. | required |

It reads this way because its first two forms both measured the wrong thing, in two different
directions. Form one failed `pkg-05` (above) by treating any recreated input as a deviation.
Form two fixed that by asking whether a constructed input "visibly satisfies the issue's
specific rule" — which fixed `pkg-05`, but then wrongly failed `pkg-09` (sharkdp/fd#2033), an
honest cannot-reproduce report that documents a careful, real attempt at a race condition and
says plainly it didn't land, naming exactly what might differ (uniform file-name lengths, a 2
MiB `ARG_MAX`). An attempt that is honestly reported as unsuccessful can't "visibly satisfy" a
rule by definition, so form two was accidentally grading outcome, not honesty. The check now
reads for version/environment gaps only; whether a substituted *input's content* actually
worked is left entirely to `target-matches`, which already compares the resulting artifact
against the issue's claimed failure and has no trouble passing an honest "I tried, and here's
what I got instead."

**Trade-offs**

`control-tested` is `preferred`, not `required`, and it costs the rubric a second layer of
proof on packages that would otherwise sail through: `calib-01` passes every required check —
terse but complete, environment stated, four followable steps, one matching artifact, expected
vs. actual named — with no control run at all, and my rubric accepts it exactly as written,
same as gold does. A rubric that required a control run for every accept would also reject
`calib-01`, so I keep the isolation step as a ranking signal rather than a gate: it rewards
`pkg-01`, `pkg-07`, `pkg-20`, and my own repro (which adds one) for doing more than the floor
asks, without punishing a short report that already proves what it needs to. What this gives up
is real: a terse report that happens to be right can look identical, on the required checks
alone, to one that got lucky on a single run with no comparison point, and this rubric cannot
tell those two apart.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
