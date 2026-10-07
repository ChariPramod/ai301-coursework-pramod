# Procedure

## Read order

1. Determine live or eval mode. In live mode read scope.md first, enforce its repository and house rules, then read voice-guide.md. In eval mode ignore both and use only the supplied package; never browse the source issue.
2. Read rubric.md and references/evidence-guide.md. List every check, its weight and the verdict rule. Stop and identify missing components if either contains no usable instructions.
3. Read issue title/body, thread highlights and repo facts BEFORE the candidate plan. In live mode fetch the issue thread and repository contribution/reporting policies. Record the reported trigger, expected behavior, relevant maintainer direction and explicit policy.
4. Read the accepted reproduction evidence next, including inputs, controls, actual output and limits. In live mode locate the contributor's posted reproduction and the reproduction quoted in the draft. Other contributors' reports do not substitute for it. Record observations separately from causal claims.
5. Only then read the whole candidate plan and draft plan comment. Do not let its confident diagnosis replace the observations recorded in step 4.

## Evidence gathering

1. For each check, use its evidence-guide section to pair the candidate claim with the issue-side fact or repro observation that supports or contradicts it. Record the location and a short quote or precise fact.
2. Trace trigger → observed failure → proposed cause → intended edit → expected post-fix observation. Specifically test the cause against every supplied control or discriminating observation; identify a contradiction even when the plan names a plausible file or repeats the issue title.
3. Map every planned edit to the supported cause. Identify unrelated changes, unresolved central choices and tests that bypass the failure. Inspect explicit thread requests and policy even if the technical approach looks sound.
4. Check that the outgoing comment conveys the proposed change and verification consistently with the plan, and compare assertions with actual evidence. In live mode read only issue-side sources plus what the drafts contain or quote; do not silently rescue an incomplete draft using unrelated local files. If a necessary source cannot be retrieved, record the precise evidence gap.

## Check execution

1. Execute every rubric row in order, without short-circuiting after a failure. Assign pass when its stated condition is supported, fail when evidence contradicts it, and unclear when necessary evidence is absent after following the map.
2. Give each grade one deciding quote or concrete fact with its location. For a non-pass state the smallest correction or evidence needed. Do not invent additional tests, headings, word counts or certainty requirements beyond the rubric.
3. In live mode compare both drafts against every voice-guide rule. Quote any violated rule and the offending phrase; report these separately unless they also fail a required rubric row.

## Verdict assembly

1. Accept only if every required row passes; otherwise reject. Preferred failures are advice only. Translate accept to ready and reject to hold in any readable summary.
2. Summarize blockers, evidence gaps and live voice notes without claiming the fix was implemented or tested merely because the plan says it will be.
3. End with the SKILL.md fenced JSON schema: item, all checks with name/grade/evidence, and verdict accept or reject. Emit nothing after the JSON. A revision gets a fresh full check, not automatic approval because the previous blocker disappeared.
