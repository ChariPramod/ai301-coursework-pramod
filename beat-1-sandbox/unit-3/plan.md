# Plan: complete US phone-number redaction (#53)

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53
Reproduction: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-6030129402

## Diagnosis

At `f89c06fc3ff292df2a04a39ac51319d32a76b779`, the issue's exact input leaves `(555) 123-4567` visible while redacting `555-123-4567`; detection returns `[]`. Removing the space produces `([REDACTED]`. The shared `phone_us` pattern allows dash/dot separators but not spaces. Its initial word boundary also prevents a match from starting at the opening parenthesis after whitespace or at the start of text. Both public methods use this pattern, so the repair belongs there rather than in their callers.

## Scope and files

- `safety/pii_scrubber.py`: change only the `phone_us` pattern.
- `tests/unit/test_pii_scrubber.py`: strengthen exact-output and detection-span assertions, add focused positive/negative cases, and remove the four phone tests' strict xfail markers once they pass.

No change to the public API, other PII patterns, database, frontend, dependencies or global formatting. Keep `test_mixed_pii_and_text` marked: its unrelated address matching behavior is outside this fix, even though its existing reason also names #53.

## Approach

1. Allow literal spaces alongside the existing dash/dot separators, including after optional `1`/`+1`. Avoid `\s`, which also joins numbers across lines.
2. Replace the leading word boundary with a boundary that can consume an opening `(` or `+` without matching inside an alphanumeric identifier. Preserve the trailing protection against matching part of a longer identifier. Keep optional country codes and existing dashed, dotted and unseparated numbers working.
3. Verify full-match behavior through `scrub()` and the `phone_us` records returned by `detect()`, including the exact start/end slice. The international pattern can produce overlapping detections for `+1` inputs; this fix will not change that separate behavior.
4. Update only the affected tests and review the diff for unintended changes.

## Test plan

Re-run the exact Python snippet quoted below through the changed module. Expected outputs are `Call me at [REDACTED] or [REDACTED]`, a `phone_us` record with value `(555) 123-4567`, start 11 and end 25, and `No-space control: [REDACTED]`.

Run `.venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -q` (with `PYTEST_DISABLE_PLUGIN_AUTOLOAD=1` if the unrelated plugin startup problem persists). The four named phone tests should become ordinary passes, with the unrelated expected failure retained. Add exact assertions for phone at start/end, `(555)123-4567`, dashed/dotted/plain forms and `+1 555 123 4567`. Negative controls: a longer digit sequence, an alphanumeric identifier containing the digits, and a phone-shaped sequence split across newlines must remain unchanged and produce no `phone_us` detection.

Then run the complete unit suite with `LLM_PROVIDER=mock`, Ruff, Black's check mode and mypy using the repository configuration. Report actual outcomes and any setup limitation; do not call a skipped or blocked check green.

## Risks and unknowns

A wider separator class can classify unrelated 3-3-4 digit groups as phone numbers; this remains a heuristic scrubber, not a phone-number validator. Boundary changes risk partial matches and lost punctuation, which is why exact output and span assertions matter. Existing country-code detection may overlap `phone_intl`. Python 3.14.7 is the local runtime, while CI uses 3.11; local results do not claim cross-version coverage. This plan was checked and posted before implementation; the build results are recorded below.

## Quoted reproduction

Reproduced on my fork at `f89c06fc3ff292df2a04a39ac51319d32a76b779` (macOS 26.6.2, arm64; Python 3.14.7; structlog 26.1.0).

I followed the Python dependency steps from the Makefile used by `docs/SETUP.md`:
```sh
git clone https://github.com/ChariPramod/pathreview-ai301-fa26-s1.git
cd pathreview-ai301-fa26-s1
git checkout f89c06fc3ff292df2a04a39ac51319d32a76b779
python3 -m venv .venv
.venv/bin/python -m pip install -e '.[dev]'
```
This is a scoped setup: I did not start Docker, run migrations, install the frontend or configure an AI API key. This reproduction imports the Python scrubber directly and uses no backing services. I am not claiming the full application works in this environment.

Run this from that checkout:
```sh
.venv/bin/python - <<'PYTHON'
import platform
from importlib.metadata import version
from safety.pii_scrubber import PIIScrubber

print(f'OS: {platform.system()} {platform.mac_ver()[0]}; arch: {platform.machine()}')
print(f'Python: {platform.python_version()}; structlog: {version("structlog")}')
s = PIIScrubber()
print(s.scrub('Call me at (555) 123-4567 or 555-123-4567'))
print(s.detect('Call me at (555) 123-4567'))
print('No-space control:', s.scrub('(555)123-4567'))
PYTHON
```
Actual output:
```text
OS: Darwin 26.6.2; arch: arm64
Python: 3.14.7; structlog: 26.1.0
Call me at (555) 123-4567 or [REDACTED]
2026-10-06 20:11:44 [info     ] pii_detected                   count=0 types=0
[]
No-space control: ([REDACTED]
```
Expected: `Call me at [REDACTED] or [REDACTED]`, and a `phone_us` detection whose value is `(555) 123-4567` (start 11, end 25).

The dashed number is a working control; the parenthesized number is left visible and detection returns `[]`. The no-space control also leaves the opening `(` behind. Reading `phone_us`, the separator accepts dash/dot but no space, and the leading word boundary cannot start at `(` following whitespace or at the beginning of the string. These are the mechanisms I plan to check in the fix, not a claim about all phone-number formats.

Related tests:
```sh
PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 .venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -q
```
```text
..xx.......x.....x....x..                                                [100%]
20 passed, 5 xfailed in 2.71s
```
I disabled automatic third-party pytest plugins for this focused run after the default invocation stalled at startup. The actual repository tests and scrubber are unchanged. The four phone tests named by the issue are expected failures; the fifth is `test_mixed_pii_and_text`, which also exercises the separate address matcher. I will keep that unrelated behavior outside this phone fix.

Codex helped inspect the code, execute the reproduction and draft this report. Claude checked the outgoing package with my repro-check skill.

## Deviations

The code stayed within the planned scope: one phone-pattern change and the matching tests. Claude made the edits; Codex reviewed the full diff and ran the checks. The four phone xfail markers are gone, and the separate mixed-PII/address failure is unchanged.

The test environment needed two adjustments. Cloud-evicted files in the Documents checkout stalled Git, so the same base commit was cloned into a temporary local directory. The existing venv was reused with `PYTHONPATH=.` to load the temporary checkout's real code. Automatic pytest plugins remained disabled; the complete unit run explicitly loaded `pytest_asyncio.plugin` and allowed the tokenizer fixture download. The first broad attempt lacked that plugin and network access (6 failures and 30 errors); after those setup corrections, the complete run passed.

Final results: the original snippet redacts both numbers and detects the full parenthesized value at 11–25; focused tests 28 passed / 1 expected failure; complete unit tests 383 passed / 49 expected failures; Ruff passed; Black reported 110 files unchanged; mypy found no issues in 76 source files. The full test run reports two warnings. Docker-backed application behavior and frontend tests were not exercised by this isolated Python repair. No implementation departure requires changing the posted plan.
