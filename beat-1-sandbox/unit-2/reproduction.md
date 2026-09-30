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

Pending: the Unit 1 selected issue or staff-assigned house issue has not been supplied. No claim has been posted.

**Reproduction comment**

Pending: claim first, then set up the assigned repository from its docs, reproduce the selected issue, check the complete package with the installed skill, and post the report. No reproduction result is asserted here.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Draft account for review; these are the runs performed with AI assistance during this session.

1. Initial full harness attempt with the first rubric: all 20 calls errored (`claude exited 1`), with no valid verdicts. It printed `agreement: 0/0 scored items` and refused to write the submission transcript. This was an execution failure, not a 0/20 calibration result.
2. Direct diagnostic using the harness prompt on pkg-20: accept, against gold reject. This was not a scored harness run. The model inferred that absent disclosure meant the policy was not triggered.
3. Official single-item harness retry on pkg-20 with the original components, one worker: `agreement: 1/1 scored items`; reject matched gold reject. Together with the diagnostic, this showed ambiguity in the disclosure rule. The rule and evidence map were tightened to require an explicit assistance statement under a strict all-AI-use disclosure policy.
4. Confirming full harness run with the revised installed components, two workers: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. The harness wrote the adjacent eval-run.txt without manual edits.

**Package analysis**

pkg-20: the final rubric decided **reject**, and the gold label is **reject**. The package's repo-facts policy says: "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". The report otherwise provides a concrete comparison: `^[[?997;2n` for a single theme versus `^[[?997;1n` for the conditional pair. Those observations support the target behavior, but neither outgoing comment includes an assistance statement. The revised conventions check therefore holds the package without pretending its technical reproduction is weak. The initial direct diagnostic had accepted the same package, which motivated making the treatment of silence explicit.

**Check rationale**

> | Repo conventions and communication | Both comments compared with repo-facts reporting requirements and contribution/AI policy, or live repository docs; Comms in the evidence guide | Meets applicable explicit reporting and disclosure requirements and communicates an independent, issue-specific account without blame, demands, or piggyback confirmation. When the repository explicitly requires disclosure of all AI assistance, the outgoing text must state the tool and extent of assistance, or explicitly state that none was used. Silence leaves compliance unclear and holds the package; do not infer no AI use from silence. No unstated policy is invented. Equivalent wording/organization is sufficient unless the repository explicitly mandates a form. | required |

Draft rationale for review: The first diagnostic on pkg-20 treated absent disclosure as evidence of no AI use and accepted the package. A separate official single-item run rejected it. The wording was tightened to make silence explicitly unclear under a strict all-AI-use policy, so the outcome does not depend on an inferred absence of assistance. This is a stricter proof-of-compliance rule; it does not claim that silence proves AI was used.

**Trade-offs**

Draft trade-off for review: Requiring an explicit assistance statement under a strict disclosure policy can hold a human-only report whose author stayed silent. That false hold is accepted to make policy compliance reviewable. Repositories without an explicit disclosure requirement do not acquire one from this check. An honest cannot-reproduce remains acceptable when the report evidences the actual trigger and observed nonfailure.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
