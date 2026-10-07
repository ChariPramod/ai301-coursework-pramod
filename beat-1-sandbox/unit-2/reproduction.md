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

ChariPramod

---

## Posted upstream

**Claim comment**

Selected issue: [#53 — PII scrubber fails to redact parenthesized US phone numbers](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53), the same issue recorded in Unit 1.

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-6030114975

I'd like to work on #53 for AI301. I'll check why `(555) 123-4567` passes through `scrub()` and `detect()` while the dashed form is handled, then post the commands and output from my local reproduction before proposing a fix.

I'm using Codex to help inspect the code, run checks, and draft the report, and Claude to check the drafts against my course rubric.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/53#issuecomment-6030129402

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

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

These are the runs performed with AI assistance.

1. Initial full harness attempt with the first rubric: all 20 calls errored (`claude exited 1`), with no valid verdicts. It printed `agreement: 0/0 scored items` and refused to write the submission transcript. This was an execution failure, not a 0/20 calibration result.
2. Direct diagnostic using the harness prompt on pkg-20: accept, against gold reject. This was not a scored harness run. The model inferred that absent disclosure meant the policy was not triggered.
3. Official single-item harness retry on pkg-20 with the original components, one worker: `agreement: 1/1 scored items`; reject matched gold reject. Together with the diagnostic, this showed ambiguity in the disclosure rule. The rule and evidence map were tightened to require an explicit assistance statement under a strict all-AI-use disclosure policy.
4. Confirming full harness run with the revised installed components, two workers: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. The harness wrote the adjacent eval-run.txt without manual edits.

**Package analysis**

pkg-20: the final rubric decided **reject**, and the gold label is **reject**. The package's repo-facts policy says: "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". The report otherwise provides a concrete comparison: `^[[?997;2n` for a single theme versus `^[[?997;1n` for the conditional pair. Those observations support the target behavior, but neither outgoing comment includes an assistance statement. The revised conventions check therefore holds the package without pretending its technical reproduction is weak. The initial direct diagnostic had accepted the same package, which motivated making the treatment of silence explicit.

**Check rationale**

> | Repo conventions and communication | Both comments compared with repo-facts reporting requirements and contribution/AI policy, or live repository docs; Comms in the evidence guide | Meets applicable explicit reporting and disclosure requirements and communicates an independent, issue-specific account without blame, demands, or piggyback confirmation. When the repository explicitly requires disclosure of all AI assistance, the outgoing text must state the tool and extent of assistance, or explicitly state that none was used. Silence leaves compliance unclear and holds the package; do not infer no AI use from silence. No unstated policy is invented. Equivalent wording/organization is sufficient unless the repository explicitly mandates a form. | required |

The first diagnostic on pkg-20 treated absent disclosure as evidence of no AI use and accepted the package. A separate official single-item run rejected it. The wording was tightened to make silence explicitly unclear under a strict all-AI-use policy, so the outcome does not depend on an inferred absence of assistance. This is a stricter proof-of-compliance rule; it does not claim that silence proves AI was used.

**Trade-offs**

Requiring an explicit assistance statement under a strict disclosure policy can hold a human-only report whose author stayed silent. That false hold is accepted to make policy compliance reviewable. Repositories without an explicit disclosure requirement do not acquire one from this check. An honest cannot-reproduce remains acceptable when the report evidences the actual trigger and observed nonfailure.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
