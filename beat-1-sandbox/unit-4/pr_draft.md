fix(safety): redact complete parenthesized US phone numbers

## Summary

The phone scrubber left `(555) 123-4567` visible and returned no detection, while handling dashed numbers. This change allows literal-space separators and uses word-character lookarounds so `scrub()` and `detect()` cover the complete number, including the opening parenthesis or optional country prefix. It retains protection against matching inside longer identifiers and does not join numbers across newlines.

## Issue

Closes #53

Plan posted before implementation: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-6030150151
Original reproduction: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-6030129402

## Changes

- Update only `phone_us` in `safety/pii_scrubber.py`: add literal-space separators and replace leading/trailing word boundaries with word-character lookarounds.
- Strengthen exact-output and detection-span assertions in `tests/unit/test_pii_scrubber.py`; cover the mixed sentence, no-space parentheses, existing phone formats, country prefixes, longer digits, identifier neighbors and newline-split inputs.
- Remove the four phone-specific strict xfail markers. Retain the separate mixed-PII/address xfail as explained below.

## Testing

- [ ] **CI is green on this PR (all five jobs)** — pending after opening; no CI result claimed.
- [x] Unit tests pass (`make test-unit`)
- [ ] Integration tests pass (`make test-integration`) — no tests collected; expected empty suite, not a pass.
- [x] Linter passes (`make lint`)
- [x] Type checker passes (`make typecheck`)
- [x] New/updated tests cover the changes
- [ ] If this fixes a seeded bug: removed its `@pytest.mark.xfail` marker (and any matching suppression in `pyproject.toml`) — four phone markers removed; one mislabeled mixed-PII/address marker deliberately retained. No matching pyproject suppression exists.

Fresh verification on macOS 26.5 arm64, Python 3.12.14, structlog 26.1.0 at `33d15a49b10504339539e6adef463d5530dbff30`. Setup: a new venv and `.venv/bin/python -m pip install -e '.[dev]'`. No Docker services, migrations or live AI provider were used. Unit tests use the mock provider; default pytest plugins were enabled.

The same script was run against main `f89c06fc3ff292df2a04a39ac51319d32a76b779` and the fixed branch. To repeat, run the following from each checkout with its venv:

```sh
PYTHONPATH=. .venv/bin/python - <<'PYTHON'
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(s.scrub('Call me at (555) 123-4567 or 555-123-4567'))
print(s.detect('Call me at (555) 123-4567'))
print('No-space control:', s.scrub('(555)123-4567'))
PYTHON
```

Before (main source exported to a separate directory; same venv):

```text
OS: Darwin 26.5; arch: arm64
Python: 3.12.14; structlog: 26.1.0
Call me at (555) 123-4567 or [REDACTED]
2026-10-14 02:34:47 [info     ] pii_detected                   count=0 types=0
[]
No-space control: ([REDACTED]
```

After (branch source):

```text
OS: Darwin 26.5; arch: arm64
Python: 3.12.14; structlog: 26.1.0
Call me at [REDACTED] or [REDACTED]
2026-10-14 02:34:35 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]
No-space control: [REDACTED]
```

Check results:

```text
$ .venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -q
28 passed, 1 xfailed in 0.08s

$ LLM_PROVIDER=mock make test-unit
383 passed, 49 xfailed, 4 warnings in 12.06s
(exit 0)

$ make test-integration
collecting ... collected 0 items
============================ no tests ran in 0.04s =============================
make: *** [test-integration] Error 5
(pytest exit 5; make exit 2 — expected empty suite)

$ make lint
All checks passed!
(exit 0)

$ make typecheck
Success: no issues found in 76 source files
(exit 0)

$ .venv/bin/black --check .
110 files would be left unchanged.
(exit 0)

$ cd frontend && npm ci && npm test -- --run
Test Files  2 passed (2)
Tests  18 passed (18)
(exit 0; Node 22.16.0)
```

Unit warnings include Pydantic deprecation, a passlib `crypt` deprecation and unawaited AsyncMock warnings; the complete output is retained in the coursework evidence. Local runs do not establish CI’s Python 3.11/Node 18 results or Docker-backed application behavior.

## Screenshots / Demo

N/A — this change is in a Python text scrubber. The exact before/after output and detected span above demonstrate the behavior.

## Notes for Reviewers

The implementation follows the posted two-file plan. `test_mixed_pii_and_text` still carries an issue #53 xfail reason, but its remaining failure is the unrelated street-address pattern removing “Python” from ordinary prose. I ran it with `--runxfail`: 1 failed, 28 deselected; the phone is redacted, while the assertion preserving “Python” fails. I retained that marker rather than broaden this phone fix into an address change. This exception is recorded in the plan’s Deviations. All four tests named in #53 now pass without xfail.

This remains a heuristic matcher: space-separated digit groups can be false positives. The separate international-phone pattern and its overlapping detections for some `+1` inputs are unchanged. No API, dependency, frontend or other PII-pattern changes are included.

AI assistance: Codex helped reproduce and investigate the bug, draft the plan, review the diff, and run verification. Claude checked the Unit 2/3 drafts and implemented the two-file change. For Unit 4, Codex authored the pr-precheck components using the prior coursework and fixed contract, captured fresh test evidence, and drafted this description and the coursework write-up. Claude/Sonnet is being used to evaluate the skill on the supplied packages and run the pre-opening review.
