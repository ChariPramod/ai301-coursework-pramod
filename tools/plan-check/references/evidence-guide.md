# Evidence guide

## Diagnosis and grounding

Eval: read Issue context, Thread highlights and Repro evidence before Candidate plan. Live: issue body and thread, the contributor's posted reproduction, and the report quoted in plan.md. Good evidence names the failing input, the observed output and any control that distinguishes competing causes. A cause may be tentative but must fit those facts and name a way to check it before building on it. A file name or plausible story alone is not support; a control that contradicts the story matters more than confident prose.

## Scope

Eval: candidate plan's edits, affected files and limits against issue/repro and thread requests. Live: those same fields in plan.md and comment.md plus issue-side direction. Good evidence links every changed behavior to the reproduced defect. Precise edits can define scope without a non-goals section. A new architecture, dependency migration or adjacent cleanup needs a demonstrated role in this fix; otherwise defer it.

## Executability

Eval: candidate approach and targets against supplied repository/source facts. Live: the draft's named paths or components and relevant public repository source. Good evidence locates the starting point and explains the operation: what condition, data flow or behavior changes and how. Exact code, line numbers, exhaustive implementation details and a patch are unnecessary. “Fix parsing” with no locator or mechanism leaves the next contributor making the core design decision.

## Test plan

Eval: compare proposed validation with Repro evidence, especially the trigger and expected/actual difference. Live: compare draft commands, inputs and expected results with the posted reproduction. Good evidence re-exercises that trigger through the affected path and says what assertion or visible output changes. A short statement such as rerunning the named repro with an exact expected result can suffice. A green build alone, omitted failing input or a mocked-away failure cannot distinguish broken from fixed. Check directly threatened compatibility without demanding unrelated infrastructure.

## Honesty

Eval: compare plan/comment assertions with repro artifacts and thread facts. Live: compare outgoing text with posted evidence and what it quotes. Good evidence distinguishes observed behavior from a diagnosis, planned test or unresolved risk. Do not require an uncertainty disclaimer for every ordinary implementation detail, but do not accept unsupported universal claims or completed-test claims. Missing evidence is not proof of success or proof the contributor used no AI.

## Comms

Eval: read repo-facts contribution/AI policy and thread highlights, then candidate plan comment and plan. Live: read repository contribution docs, applicable templates and maintainer replies, then drafts and personal voice guide. Good evidence follows explicit requests, includes the required disclosure where applicable, and describes an independent plan in respectful concrete language. Equivalent wording is fine unless a format is expressly required. Other students' comments are context, not the candidate's own proof. No policy in the evidence means do not invent one. Voice rules apply only live; rubric communication rules apply in both modes.
