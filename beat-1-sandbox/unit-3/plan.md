# Plan: issue #72, `verify_password` raises on malformed stored hashes

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72
Repro comment: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5804907465

## Diagnosis

`verify_password` in `core/security.py` calls `pwd_context.verify(...)` with no error handling, so any exception passlib raises while identifying or parsing the stored hash escapes to the caller. My repro shows two different exceptions from that one line:

```
'' -> raised passlib.exc.UnknownHashError hash could not be identified
'plaintext' -> raised passlib.exc.UnknownHashError hash could not be identified
'$2b$notarealhash' -> raised builtins.ValueError not enough values to unpack (expected 2, got 1)
```

The valid-hash control (`hash_password('password')` then `verify_password`) returns `True`, so verification itself works and the break is only on the malformed-hash path. The only caller is `api/routes/auth.py:80` (`login`), where an escaping exception would surface as an error instead of the 401 the route documents.

I checked passlib's class hierarchy locally: `UnknownHashError.__mro__` is `(UnknownHashError, ValueError, Exception, ...)`, so it is a `ValueError` subclass. Both exceptions in the repro are therefore `ValueError`s.

## Scope

In: make `verify_password` return `False` when passlib cannot parse the stored hash (unknown format, empty string, bcrypt-looking but malformed). Remove the `xfail(strict=True)` marker on `test_verify_with_wrong_hash_format`. Add regression cases for the other two malformed shapes from my repro.

Not in: changing `hash_password`, the `CryptContext` schemes, the bcrypt/passlib pin or the "(trapped) error reading bcrypt version" warning, logging or metrics for bad hashes, any change to `api/routes/auth.py`, and any migration of stored hashes.

## Files

- `core/security.py`: `verify_password` only.
- `tests/unit/test_security.py`: remove the xfail marker; add a parametrized test.

## Approach

1. In `verify_password`, wrap the `pwd_context.verify(...)` call in `try/except ValueError` and return `False`. `ValueError` covers `UnknownHashError` and the bare `ValueError` from my third repro case. Catching only `UnknownHashError` would still let `'$2b$notarealhash'` through.
2. Update the docstring: returns False for a wrong password or an unreadable stored hash.
3. Remove the `@pytest.mark.xfail(strict=True, ...)` decorator from `test_verify_with_wrong_hash_format`.
4. Add a parametrized test over `""`, `"plaintext"`, `"$2b$notarealhash"` asserting `verify_password("password", bad) is False`.
5. Run `make check` and `make test-unit` locally.

## Test plan

Re-run my unit 2 repro steps and compare with what I recorded then.

Before (recorded in my repro comment):
- `verify_password('password', 'not_a_valid_bcrypt_hash')` raises `passlib.exc.UnknownHashError: hash could not be identified`.
- `''` and `'plaintext'` raise `UnknownHashError`; `'$2b$notarealhash'` raises `ValueError: not enough values to unpack (expected 2, got 1)`.
- `pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v` reports `XFAIL`.

After the fix I expect:
- All four inputs above return `False` with no traceback.
- The valid-hash control `verify_password('password', hash_password('password'))` still returns `True`.
- `pytest tests/unit/test_security.py -v` shows `test_verify_with_wrong_hash_format` as `PASSED` (not XFAIL, and not XPASS-strict failure), plus the new parametrized cases passing.
- `make test-unit` has no new failures.

## Risks and unknowns

- Catching `ValueError` is broader than `UnknownHashError`. I have not checked whether passlib can raise a `ValueError` for a reason other than a malformed hash (for example a password-side problem). If it can, the function would return `False` instead of surfacing it. I will check passlib's bcrypt handler while building and narrow the clause if needed.
- Failing closed hides data corruption: a bad stored hash now looks like a wrong password at login. I am leaving logging out of scope and will note it in the PR rather than add it.
- I have not run the full integration suite for the login route; no test I found covers a malformed `hashed_password` end to end.
- Several classmates have claimed this issue and PRs #75, #84 and #85 are already open, #75 with the same `except ValueError` approach. I am building from my own reproduction and will re-check those PRs before opening mine in unit 4.

## Deviations

Nothing changed from the posted plan: same two files, same `try/except ValueError` approach, and the three-case parametrized test I planned. The one unknown I flagged (whether passlib can raise `ValueError` for a non-hash reason) I did not chase further; I kept the `ValueError` clause as planned. I also could not run `make test-unit`, `make typecheck` or the integration suite locally because my venv lacks the dev dependencies (17 collection errors on unrelated modules); I ran `ruff check` and `black --check` on the two changed files and the whole of `tests/unit/test_security.py` (28 passed) instead.
