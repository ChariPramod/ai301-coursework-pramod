# Unit 4 — Test and Submit

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/206

Opened by ChariPramod against `codepath/pathreview-ai301-fa26-s1:main` on October 14, 2026. It fixes issue #53 and contains only `safety/pii_scrubber.py` and `tests/unit/test_pii_scrubber.py` (one commit, +67/-23). All template sections are filled, including AI assistance, before/after reproduction, the four required Make targets, and the deliberately retained unrelated address xfail. CI was honestly unchecked when opened.

**Branch**

`fix/53-complete-phone-redaction`

https://github.com/ChariPramod/pathreview-ai301-fa26-s1/tree/fix/53-complete-phone-redaction

## Eval iterations

**Run history**

All runs below used the official, unmodified Unit 4 harness and the installed `~/.claude/skills/pr-precheck/` components. The harness pins Sonnet. Codex helped author and revise the components from the Unit 3 materials and fixed Unit 4 contract; no activity worksheet or group participation is claimed.

1. The first full attempt, with two workers, was interrupted when the chat turn was stopped. It printed 16 package verdicts but never reached an agreement score or wrote an evaluation transcript. This is an incomplete attempt, not a scored full run.
2. The first completed full run, with five workers, scored `agreement: 18/20 scored items  (bar: 18/20: PASS)`. Categories: clear-accept 5/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. The disagreements were false rejections of pkg-08 and pkg-16.
3. After clarifying how evidence from incomplete excerpts is interpreted, a targeted run on pkg-08 and pkg-16 plus canaries pkg-01, pkg-03, pkg-04, pkg-12 and pkg-20 scored `agreement: 7/7 scored items`. Both standards-wall cases and one case from each other rejection family remained correct. This partial run did not replace the saved full transcript.
4. The confirming full run scored `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: clear-accept 7/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. The adjacent `eval-run.txt` is the exact file generated with `--save-run`, copied without editing. Its component fingerprints match the uploaded skill. Full per-check results are in `eval-results.json`.

Separately, an initial live precheck was interrupted without output. A completed live run accepted the draft, and a fresh live run with the revised final components also returned `accept`, with every required check passing. The final output is in `pr-precheck.txt`; live checks are not calibration scores.

**Package analysis**

pkg-08: my first completed full run returned **reject**, while the staff gold label is **accept**. After revision, both the targeted and final full runs returned **accept**.

The failed Accurate claims row said the description’s “plan posted on the thread” was unsupported because the two thread highlights did not include that plan. It also said “code formatted” had no recorded result. But Plan context explicitly introduces “The accepted plan this PR claims to implement, excerpted from the posted plan”; those supplied facts establish the plan independently of the selected highlights. The description’s requirements checklist explicitly attests formatting, and this repository does not demand a separate formatter transcript.

The actual behavioral evidence is concrete: “after: s -> toast” followed by “no prompt; untracked.txt still listed,” plus the tracked-change control and passing integration/go-test results. The diff adds the pre-prompt guard, internationalized message and integration test requested by the plan. The revision accepts that evidence without requiring the highlights to repeat the plan or inventing a formatter-log requirement. It still requires observable proof of the repair itself.

pkg-16 exposed the related inference problem: the first run reconstructed exact error text and unshown start-path caller behavior from a partial diff, then used that speculation to dispute a recorded corrupt-tar/valid-tar transcript. The clarification requires an actual contradiction in supplied evidence, rather than assuming omitted implementation context behaves one particular way.

**Check rationale**

Exact current rubric row:

> | Accurate claims | PR title, description and checked boxes paired with diff, observed test results and stated limitations; Plan fidelity and Test evidence | Material claims about behavior, coverage, completed checks and readiness are supported by the package as a whole. Accepted-plan context and concrete recorded outcomes are evidence; missing details in thread highlights or excerpted code are not contradictions. Require a demonstrated conflict, not speculation about omitted callers/helpers, before declaring a claim false. An explicit routine formatting/checklist attestation counts without a separate log unless the repository asks for one; core behavioral verification still requires observable results. Failures, omissions and environment limits are disclosed without being labelled passes. No invented approval or test result. A truthful bounded shortfall is eligible to pass; disclosure does not replace all behavioral evidence or override a mandatory repository standard. | required |

I revised this row because the first grader treated omitted context as evidence against the submission. The distinction I want is between a contradiction and an excerpt that does not show every supporting detail. A supplied accepted-plan context can establish that the plan was posted; routine checklist attestations can establish formatting when logs are not required. Neither excuses an unrelated behavioral test, a failing result described as green, missing promised behavior or a real policy violation. The procedure and evidence guide now explain how to use those evidence sources without guessing what omitted code does.

**Trade-offs**

This rule trusts concrete supplied observations and routine attestations when the record does not contradict them. A dishonest author could exploit that limit; this precheck cannot independently rerun every frozen package. Requiring an independent transcript for every routine claim would instead hold complete submissions for missing optional paperwork. The required Observable verification check still asks for the decisive trigger and observed outcome, and the standards check still enforces explicit repository asks.

The targeted canaries stayed correct at 7/7, including both disclosure/template cases, silent drift, missing verification and debris. The final full run matched all 20, which supports the change on this supplied set, not a claim of perfect judgment on unseen PRs.

## Verification and scope notes

Fresh results: 28 focused tests passed with 1 expected failure; 383 unit tests passed with 49 expected failures and 4 warnings; lint, Black, mypy and 18 frontend tests passed. `make test-integration` reported no tests ran (pytest exit 5, Make exit 2), exactly as the assignment predicts. It is disclosed rather than labelled a pass. Full commands/output are in `test_evidence.md`, and the submitted title/description are in `pr_draft.md`.

The four phone xfails are removed. A diagnostic `--runxfail` run confirms the remaining mixed-PII test still fails on unrelated address overmatching. The exception is explained in the PR and the Unit 3 plan’s Deviations; no unrelated address change was introduced. The plan and draft/evidence files were not committed to the application fix branch.
