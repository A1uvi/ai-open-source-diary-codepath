# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

A1uvi

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5987944245

Plan for #72, from my own repro above. Before any fix, `verify_password('password', 'not_a_valid_bcrypt_hash')` raised `UnknownHashError`, and `'$2b$notarealhash'` raised a bare `ValueError` instead. The valid-hash control returned `True`, so only the malformed-hash path is broken.

The cause is in `core/security.py`: `verify_password` calls `pwd_context.verify(...)` with nothing around it, so whatever passlib raises while parsing the stored hash gets out. I checked locally that `UnknownHashError` subclasses `ValueError`, so one `except ValueError: return False` covers both exceptions I saw. Catching only `UnknownHashError` would miss the `$2b$` case.

I'll change `core/security.py` and `tests/unit/test_security.py` and nothing else. In the tests I'll drop the `xfail` marker on `test_verify_with_wrong_hash_format`, as the issue asks, and add a parametrized case for `""`, `"plaintext"` and `"$2b$notarealhash"`. I'm leaving `hash_password`, the passlib/bcrypt pins, `api/routes/auth.py` and any logging of bad hashes alone.

To check it, I'll re-run the same repro commands. Expect all four bad inputs to return `False` with no traceback, the control to still return `True`, and the test to show PASSED instead of XFAIL. I'll post the output.

I saw #75, #84 and #85 already open with the same `ValueError` approach. I'm building my branch on my fork from my own repro, and I'll look at those PRs again before opening mine in unit 4. One thing I haven't confirmed: whether passlib can raise `ValueError` for a reason other than a bad hash. If it can, I'll narrow the clause.

I used Claude Code to help draft this plan. I ran the repro and read the code myself.

---

## Your branch

**Branch**

fix/72-verify-password-malformed-hash

**Evidence**

Repro commands from my unit 2 repro comment, extended with the `''` / `'plaintext'` / `'$2b$notarealhash'` cases and run via `python3 -W ignore -c ...` in the repo's venv (the passlib `(trapped) error reading bcrypt version` warning is filtered out; it is unrelated and appears in both runs).

Before (`main` at `f89c06f`):

```
control (valid hash): True
'not_a_valid_bcrypt_hash' -> raised passlib.exc.UnknownHashError hash could not be identified
'' -> raised passlib.exc.UnknownHashError hash could not be identified
'plaintext' -> raised passlib.exc.UnknownHashError hash could not be identified
'$2b$notarealhash' -> raised builtins.ValueError not enough values to unpack (expected 2, got 1)
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL [100%]
================ 24 deselected, 1 xfailed, 2 warnings in 0.34s =================
```

After (branch `fix/72-verify-password-malformed-hash`):

```
control (valid hash): True
'not_a_valid_bcrypt_hash' -> False
'' -> False
'plaintext' -> False
'$2b$notarealhash' -> False
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [ 25%]
tests/unit/test_security.py::TestSecurity::test_verify_with_malformed_stored_hash_returns_false[] PASSED [ 50%]
tests/unit/test_security.py::TestSecurity::test_verify_with_malformed_stored_hash_returns_false[plaintext] PASSED [ 75%]
tests/unit/test_security.py::TestSecurity::test_verify_with_malformed_stored_hash_returns_false[$2b$notarealhash] PASSED [100%]
================= 4 passed, 24 deselected, 2 warnings in 0.17s =================
```

Full `tests/unit/test_security.py` on the branch: `28 passed`. `ruff check` and `black --check` clean on the two changed files. `make test-unit` and `make typecheck` could not run in my venv (dev dependencies not installed).

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

19/20 (one full run, saved as `eval-run.txt`; the only miss was `pkg-14`; categories line: clear-accept 6/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4). I did not re-run: 19/20 clears the 18/20 bar with every category matched.

**Package analysis**

`pkg-14` (zellij-org/zellij#5174). Gold label: `accept` (an honestly scoped-down plan, arguable on the deferral). My rubric decided `reject`, failing `diagnosis-grounded` and `executable`. On `executable`, the plan says "exact functions to be pinned in the PR after tracing the query issuance with debug logs", and my rubric says a plan fails when "the real decisions are deferred to build time"; that phrase reads as exactly that. On `diagnosis-grounded`, the plan's cause (the reattach path wires stdin before the OSC query responses are consumed) is a mechanism the repro never directly shows; the repro shows a regression window (0.44.1 clean, 0.44.2 on leaks) and the cache control, which is consistent with the cause but doesn't pin it, so my grader leaned toward not-confirmed. The plan does name the layer (the client attach path in `zellij-server` / `zellij-client`), does bound the scope, and does state its deferral of the Windows variant with a reason, which is why gold accepts it. My rubric is stricter than gold about naming functions up front, and I left it that way on purpose; this is one of the four packages the course calls arguable.

**Check rationale**

The check as it reads in `tools/plan-check/rubric.md`:

> | bounded-scope | The plan's in-scope statement, its not-in line, and everything its approach actually does. | One change the issue and repro justify, with a stated not-in line, and the approach stays inside it. A bounded core fix that explicitly defers a harder or untestable part, and says so with a reason, passes. Fails when the approach adds work the issue never asked for (a refactor, migration, new option, new framework, CI or test-harness rework, "while I'm here" cleanup), even when the core fix inside it is right. Also fails when the plan changes something other than what the repro shows is broken (a docs-only workaround for a code defect the thread is already fixing). | required |

It reads this way because the scope-creep packages in the set (`pkg-06`, `pkg-12`, `pkg-15`, `pkg-19`) all share one shape: a small, correct fix surrounded by extra work the issue never asked for (a redesign, a new option, a migration). So the check grades what the approach steps actually do against the not-in line, not whether the core fix is right, and it says outright that a correct core fix doesn't rescue a bundle. I added the sentence "A bounded core fix that explicitly defers a harder or untestable part, and says so with a reason, passes" because the scoped-down accepts (`pkg-09`, `pkg-14`) defer part of the issue on purpose, and a check that counted every deferral as a gap would reject them. The last sentence (a docs-only workaround for a code defect) was written for `pkg-04`, where the plan changes something other than what the repro shows is broken.

**Trade-offs**

`bounded-scope` gives up the benefit of the doubt on a plan whose extra work is arguably useful. `pkg-15` is the case: its core one-constant fix is right, and gold itself calls it arguable, but my check rejects it because the undici migration and settings panel are bundled in. The cost is that a maintainer who actually wanted the larger change gets a rubric that holds the plan anyway. I accept that, because a reviewer can hold a diff to a not-in line only if the plan stays inside it. A second thing it gives up: it reads only stated scope against stated approach, so a plan that quietly leaves the not-in line empty and then touches an extra file would be graded by what the approach lists, not by what the diff does later. That belongs to unit 4's diff-against-plan check, not this one. I did not re-run any canaries with `--only` since there was only one full run and I changed no check after it.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
