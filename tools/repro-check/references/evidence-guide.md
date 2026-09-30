# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives:** In eval, compare the issue body and repo-facts bug-report requirements with the candidate repro report's version, platform, runtime, and configuration statements. In live mode, compare the draft environment record with the issue thread, the repository README/setup docs, contribution guide, and applicable issue template at the tested revision.

**What good looks like:** The record identifies the tested code and the environment variables that could change the outcome: for example browser and viewport for a UI bug, or runtime and dependency versions for a CLI bug. A different version or platform is disclosed and conclusions remain bounded to it; a machine inventory is unnecessary when unrelated to the trigger.

## Steps

**Where it lives:** In eval, read the candidate report's setup commands, numbered or prose actions, embedded input files, fixture content, and stated starting state against the issue trigger. In live mode, read the same material in the draft, consult the repo's setup docs, and verify any linked fixture is accessible to a reader.

**What good looks like:** A reader can establish the starting state, supply the same inputs, perform the trigger, and inspect the result without inventing a significant missing step. Short prose or a self-contained test may suffice; many numbered steps do not compensate for a missing input. Local files outside the outgoing draft are not evidence unless included or accessibly linked.

## Behavior shown

**Where it lives:** In eval, compare the issue description and thread clarification with output excerpts, logs, test assertions/results, or screenshot descriptions included in the candidate report. In live mode, inspect the artifacts actually embedded in or linked from the outgoing draft and compare them with the issue's expected and reported behavior.

**What good looks like:** The artifact connects the relevant trigger to an observable result that discriminates this bug from nearby symptoms. A visual bug can use an annotated screenshot; a CLI bug can use input, output, and exit status. A bare assertion that it happens, an inaccessible image, or an error before the relevant trigger does not establish reproduction. For cannot-reproduce, show the attempted trigger and the resulting normal behavior rather than requiring evidence of a failure that did not occur.

## Honesty

**Where it lives:** In eval, compare assertions in both candidate comments with their included artifacts and environment; distinguish the issue author's theory from verified behavior. In live mode, make the same comparison within the outgoing package, using the issue for context rather than treating another contributor's reproduction as the student's own evidence.

**What good looks like:** The conclusion says what the recorded attempt demonstrated and acknowledges material limits. A prospective claim promises investigation and a report; a completed report can state reproduced or cannot reproduce if backed by observations. Suspected causes are labeled hypotheses, and one tested configuration does not become a claim about every platform.

## Comms

**Where it lives:** In eval, compare the candidate claim with the issue's particulars, then compare both comments with the repo-facts bug-report template asks and contribution/AI policy. In live mode, read the issue thread, applicable README/CONTRIBUTING and issue templates, scope.md's house rules, voice-guide.md, and the exact outgoing drafts.

**What good looks like:** The claim identifies the concrete behavior and next investigation; the report contains independent proof and satisfies stated reporting requirements. Apply an explicit AI-disclosure requirement to the comments themselves, not an unseen local note; under a strict all-AI-use disclosure policy, silence cannot establish compliance: require tool and extent of assistance or an explicit statement of no assistance. Absent a stated policy, do not invent one. Shared classroom claims are allowed by scope.md. Comment tone reports observations without blame or demands, and promises no guaranteed fix or date. Voice rules are checked only in live mode and violations are reported by quoting the rule.
