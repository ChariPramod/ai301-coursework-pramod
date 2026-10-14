# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Eval: compare Plan context (repro, scope, boundary, test plan and any deviations) with Candidate PR → Diff, Description and Commits. Live: compare plan.md including Deviations and the posted issue plan with git diff main...HEAD and pr_draft.md. Map behavior as well as filenames: an in-scope file can still contain out-of-scope changes.

Good: each delivered hunk traces to the repair and each planned promise is implemented or explicitly narrowed. A justified deviation or honest omission can reconcile the plan and diff when the outgoing description agrees. “Exactly follows the plan” is contradicted when material additions or omissions remain hidden. The title and description must not claim behavior the diff does not deliver.

## Test evidence (harness category: not-tested)

Eval: pair the Plan context reproduction/test plan and Repo facts check asks with Candidate PR → Test evidence and testing claims in Description. Live: read test_evidence.md, the reproduction quoted in plan.md and issue thread, pr_draft.md testing boxes, and the actual PR template/CONTRIBUTING.md commands.

Good: the original trigger or equivalent regression is exercised and decisive before/after results are observable; repository checks have commands and actual results. Concrete manual actions and observed outputs can count; “works,” test names alone, future promises or unrelated suite success cannot. Distinguish a recorded failure, blocked attempt, skipped check and pass. Honest failures/limits may be acceptable under the stated standards; do not silently mark them green or demand complete raw logs when decisive evidence is present.

## Diff quality (harness category: unreviewable)

Eval: inspect Candidate PR → Diff and Commits against Plan context and the issue. Live: inspect git diff main...HEAD (all hunks), its --stat and git log main..HEAD; use git status --short to distinguish local drafts from committed files.

Good: the diff contains the bounded repair and useful regression tests without debug prints, dead code, unrelated formatting, opportunistic cleanup or generated/local artifacts. Necessary documented scope changes are reviewable; merely mentioning unrelated cleanup does not make it necessary. Neither many commits nor a large diff is an automatic failure.

## Standards and comms (harness category: standards-wall)

Eval: Repo facts names template asks and contribution/AI policy; Issue and Thread highlights contain explicit direction. Compare them with Candidate PR → Title and Description. Live: read the target .github/PULL_REQUEST_TEMPLATE.md, docs/CONTRIBUTING.md and the whole issue thread, then compare pr_draft.md. Read scope.md and voice-guide.md only in live mode as the frame directs.

Good: actual mandatory sections contain substantive answers, the correct issue is linked, required AI disclosure identifies tools and extent (or explicitly none), and claims/checkboxes agree with evidence. Do not import Path Review’s template into an eval repo with no template. Do not invent requirements from silence. Pending post-opening CI is honestly unchecked in a draft; recorded red CI must never be described as green. Voice feedback quotes the user’s own rule separately unless the rubric also makes it consequential.

**Reading incomplete excerpts**

Plan context explicitly identifies the accepted posted plan; selected thread highlights need not repeat it. A concrete test-evidence transcript is a recorded observation, while a diff excerpt may omit callers, helpers, imports or output decoration. Do not invent their behavior to manufacture a contradiction. A visible contradiction still fails: an unrelated exercised path, a disabled assertion, missing promised repair, or a green claim paired with a failure cannot be rescued by imagined omitted code. Routine checklist attestations such as formatting can be evidence when logs are not explicitly required; they never replace observable before/after proof of the repair.
