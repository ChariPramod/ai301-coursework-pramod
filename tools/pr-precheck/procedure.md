# Procedure: how this tool grades a PR package

## Read order

1. Determine mode. In live mode enforce scope.md first, then read voice-guide.md. In eval use only the bundle and the four supplied components, ignoring scope and voice.
2. Read the rubric and evidence guide. Refuse empty rubric/procedure content and report missing components. Record check order, weights and verdict rule.
3. Read issue, thread and repo facts before the candidate. Record the reported trigger, observed failure, explicit maintainer constraints, template asks and contribution/AI policies. Live sources are the issue thread, PR template and CONTRIBUTING.md.
4. Read plan and reproduction next, before the diff. List promised behavior, file boundaries, non-goals, tests and explicit deviations. Distinguish original intent from disclosed changes.
5. Read candidate diff and commits, then title/description and test evidence in full. Live: run git diff main...HEAD and git log main..HEAD; inspect git status --short so uncommitted edits are not mistaken for committed evidence.

## Evidence gathering

1. Plan fidelity: map every diff hunk to a planned change or deviation and every promised change to delivered work or a disclosed omission. Compare description claims with that map in both directions. Record the precise mismatch if any.
2. Test evidence: pair each decisive reproduction/test-plan requirement with command or action, input/scenario, before/after observation and claimed outcome. Inventory the repository’s required checks and their actual results or disclosed failed attempts/limitations. Separate unrun, blocked, failed and passed; do not infer coverage from a green status alone.
3. Diff quality: inspect all hunks for debug statements, abandoned code, accidental files, unrelated refactoring and formatting. Record why each questionable hunk matters to the issue. Commit count alone is not evidence of poor reviewability.
4. Standards and comms: compare each explicit template or policy ask to the outgoing text. Record the applicable ask and satisfying content or absence. Separate requirements possible before opening from post-opening CI. Check attribution and authorization against the thread. In live mode identify duplicate student PRs if available; classmates’ PRs do not block this one.
5. Accurate claims: pair title/description assertions and checked boxes with the diff and gathered outputs. Use the whole package for evidence, but do not silently repair misleading outgoing claims with private notes. Locate evidence-navigation problems for the preferred row.

## Check execution

1. Execute every row in rubric order without stopping after a failure. Assign pass from supporting evidence, fail from contradiction, and unclear for a necessary missing fact. Cite one deciding quote or concrete observation and its location for every grade.
2. Before holding for incomplete scope or a failed check, read deviations and disclosure. Apply the actual pass condition: an honest bounded shortfall is not silent drift, but it does not manufacture test evidence or excuse unrelated changes or mandatory policy violations.
3. Use each supplied evidence source for what it establishes: accepted-plan context can establish a posted plan even when thread highlights omit it; a routine formatting checklist can attest completion without a separate log. Do not infer an exact error message or unshown caller behavior from an excerpt and use that inference to discredit a concrete recorded run. Require an explicit contradiction in the supplied record, while still rejecting missing decisive behavioral evidence. Do not require a particular length, full transcript, commit count or heading when the repository does not. Re-read only the paired source needed to resolve uncertainty; use already gathered facts for other checks. If a source is inaccessible, name it and grade the affected required evidence unclear rather than guessing.
4. In live mode report voice-rule violations with quotes separately; change a grade only if they also fail a rubric condition. Do not edit the package while grading it.

## Verdict assembly

1. Apply the rubric: all required pass means accept; any required fail or unclear means reject. Preferred results are advisory.
2. In the readable summary, quote the deciding evidence for the first non-passing required row in rubric order, then list every remaining blocker and smallest correction. For accept, summarize the evidence supporting fidelity and verification. Include live voice notes separately.
3. End with the fixed SKILL.md JSON block: item, all checks in order with name/grade/evidence, then verdict. Emit nothing after it. A revised package receives a fresh evaluation of every check.
