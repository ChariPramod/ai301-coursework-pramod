# Unit 3 — Plan and Build

## Posted upstream

**GitHub username**

ChariPramod

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-6030150151

Exact posted text:

My reproduction: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-6030129402

The issue's snippet leaves `(555) 123-4567` visible and returns no detection; the dashed number redacts. The no-space control returns `([REDACTED]`. Those results point to two parts of `phone_us`: no literal space in the separators, and a leading word boundary that skips the opening parenthesis.

I plan to change only that pattern in `safety/pii_scrubber.py`, allowing spaces and matching the full number, including its parentheses or optional country prefix. I'll keep protection against matching within longer identifiers and avoid `\s` so the pattern doesn't join separate lines.

In `tests/unit/test_pii_scrubber.py`, I'll check exact redacted output and detected values/positions for parenthesized, dashed, dotted, plain and country-prefixed forms, plus longer-digit, identifier and newline controls. I'll remove the four phone tests' strict xfail markers once they pass. The unrelated address-pattern failure in `test_mixed_pii_and_text` stays outside this change.

I'll rerun the posted snippet: both numbers must become `[REDACTED]`, and detection must include the complete `(555) 123-4567` at positions 11–25. Then I'll run the focused tests, full unit suite, lint, formatting and type checks. Space-separated digit groups can still be false positives; this remains a heuristic, and I'll report check failures or environment limits. I haven't built the fix yet.

Codex helped reproduce the bug and draft this plan; Claude is checking it with my plan-check skill and will help implement it on my fork.

## Your branch

**Branch**

`fix/53-complete-phone-redaction`

https://github.com/ChariPramod/pathreview-ai301-fa26-s1/tree/fix/53-complete-phone-redaction

The plan was posted before code changes. Claude edited the two planned files, and Codex reviewed the full diff and ran verification. `plan.md` and `comment.md` are excluded from the fix branch's commits.

**Evidence**

Before: base commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`, as posted in [my reproduction](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-6030129402). After: the changed code on the branch above. Both runs import the real `safety.pii_scrubber.PIIScrubber`.

The exact script used for both runs (`reproduce53.py`, kept outside the source tree):

```python
import platform
from importlib.metadata import version
from safety.pii_scrubber import PIIScrubber

print(f'OS: {platform.system()} {platform.mac_ver()[0]}; arch: {platform.machine()}')
print(f'Python: {platform.python_version()}; structlog: {version("structlog")}')
s = PIIScrubber()
print(s.scrub('Call me at (555) 123-4567 or 555-123-4567'))
print(s.detect('Call me at (555) 123-4567'))
print('No-space control:', s.scrub('(555)123-4567'))
```

Original run from the coursework workspace:
```sh
work/pathreview/.venv/bin/python work/reproduce53.py
```
```text
OS: Darwin 26.6.2; arch: arm64
Python: 3.14.7; structlog: 26.1.0
Call me at (555) 123-4567 or [REDACTED]
2026-10-06 20:11:44 [info     ] pii_detected                   count=0 types=0
[]
No-space control: ([REDACTED]
```

After run from `/private/tmp/ai301-pathreview-pramod`:
```sh
PYTHONPATH=. .venv/bin/python /Users/pramodkrishnachari/Documents/Codex/2026-09-29/unit-2-reproduce-it-and-teach/work/reproduce53.py
```
```text
OS: Darwin 26.6.2; arch: arm64
Python: 3.14.7; structlog: 26.1.0
Call me at [REDACTED] or [REDACTED]
2026-10-06 20:16:43 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]
No-space control: [REDACTED]
```

For a stranger's rerun, save the script above as `reproduce53.py` outside either checkout, run `PYTHONPATH=. .venv/bin/python /path/to/reproduce53.py` from that checkout, and use the setup commands in the quoted reproduction in `plan.md`. The local temporary checkout uses the same installed venv; `PYTHONPATH=.` selects the code being verified.

Focused test commands (before in the original checkout; after in the temporary checkout):
```sh
PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 .venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -q
PYTHONPATH=. PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 .venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -q
```
Before:
```text
..xx.......x.....x....x..                                                [100%]
20 passed, 5 xfailed in 2.71s
```
After:
```text
..........................x..                                            [100%]
28 passed, 1 xfailed in 0.07s
```

The four phone expected failures now pass normally; the new exact-output, span and negative-control tests also pass. The remaining expected failure is the unrelated mixed-PII/address behavior.

Full verification commands:
```sh
PYTHONPATH=. PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 LLM_PROVIDER=mock .venv/bin/python -m pytest tests/unit -q -p pytest_asyncio.plugin
.venv/bin/ruff check .
.venv/bin/black --check .
.venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
```
Full-unit summary:
```text
383 passed, 49 xfailed, 2 warnings in 8.86s
```
Ruff:
```text
All checks passed!
```
Black:
```text
All done! ✨ 🍰 ✨
110 files would be left unchanged.
```
Mypy:
```text
Success: no issues found in 76 source files
```

The initial broad run had 6 failures from the omitted async plugin and 30 setup errors fetching tokenizer data with network disabled. The corrected run above passed. Cloud-evicted files also stalled the initial Git scan and default pytest startup; the working checkout was moved to a local temporary directory. These are environment adjustments, not hidden code changes. Full unit output is in `unit-tests.txt`; two warnings remain. No Docker, frontend or cross-Python-version result is claimed.

## Eval iterations

**Run history**

1. First full attempt: stopped after the four initial Claude calls stalled on Git reads of cloud-evicted files. No verdict table or agreement score was produced, so this is an interrupted execution, not a scored calibration result. No rubric changes followed.
2. Full official Sonnet run from the temporary local starter checkout, four workers: `agreement: 18/20 scored items  (bar: 18/20: PASS)`. Category agreement: clear-accept 5/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4. The harness wrote the submitted `eval-run.txt`; it was copied byte for byte. No scored partial runs were made. The two disagreements were pkg-09 and pkg-14, both false holds; the target and every category floor were met.

**Package analysis**

pkg-01: my rubric said **reject**, and the gold label said **reject**. The repro says: "the error is raised by argparse's `parse_args` while consuming positionals; the request items are never handed to HTTPie's item parser." The candidate instead says: "The `REQUEST_ITEM` tokenizer in `httpie/cli/requestitems.py` is the problem." That edit would happen after the observed failure point. The control also parses the same request items when the flag is removed. Reading those observations before the candidate diagnosis makes the contradiction visible; a concrete file name and a tidy test section do not repair it.

**Check rationale**

Exact current check:

> | Grounded diagnosis | Issue, thread, repro observations and candidate diagnosis; Diagnosis and grounding | The proposed cause fits the observed trigger, controls, and failure location. It explains the reported behavior without contradicting the repro or ignoring a discriminating observation. An explicitly tentative cause is enough when supported and paired with a concrete verification step before dependent edits. | required |

I chose this wording instead of asking only whether a plan names a cause and a file. That weaker check would accept pkg-01's plausible-sounding tokenizer change even though the repro never reaches the tokenizer. I also allowed a supported tentative cause with a concrete verification step, because a plan should not need a finished patch to be reviewable. These components were drafted from the supplied assignment and Unit 2 workflow, with AI assistance; no group worksheet contribution is claimed.

**Trade-offs**

The conservative evidence checks produced two false holds. In pkg-09, the grader flagged the plan's claim that a collaborator endorsed an option when the quoted endorsement came from a non-collaborator. In pkg-14, it wanted a stronger account of the cache control and a more specific starting point before accepting the handshake repair. Staff accepted both. This version therefore risks holding a workable plan whose technical inference or attribution is less explicit. I kept that limitation visible instead of reporting perfect agreement or changing rules solely to match two answers. Every rejection category was recognized, and five clear accepts still passed; the exact per-check decisions are saved in `eval-results.json`.
