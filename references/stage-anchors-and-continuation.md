# Main Goal, Stage Alignment, and Continuation

## Goal hierarchy

Use one authoritative chain:

`main goal -> current stage goal -> next action -> acceptance evidence`

- **Main goal:** the user's observable end state. It remains authoritative until the user changes, pauses, cancels, or replaces it.
- **Stage goal:** the smallest measurable outcome currently needed to advance the main goal.
- **Next action:** the highest-ROI safe action that advances the stage goal on the true target surface.
- **Acceptance evidence:** the evidence used by the single final validation gate.

A stage may narrow execution but may not redefine, weaken, silently broaden, or substitute for the main goal. A blocker, child task, artifact, metric, local PASS, or available tool does not become the goal automatically. Stage completion closes only that stage; compare the result with the remaining main-goal gap before continuing or claiming completion.

## When a durable stage anchor is required

Create or update one project-owned anchor when work spans turns, agents, mutable revisions, a handoff, or a material stage boundary. Do not require a durable anchor for a short, single-owner task whose goal and state remain visible in the active context.

A durable anchor contains only the state needed for continuation:

- main goal and observable completion evidence;
- current stage goal and its explicit contribution to the main goal;
- true target surface and forbidden substitutes;
- canonical semantic and runtime owners;
- admitted baseline, active writer, intended final accepted state, and allowed scope;
- active decision boundaries;
- retained or superseded artifacts that affect continuation.

Use an existing roadmap, issue, state file, or equivalent canonical artifact. Do not create a parallel ledger. Repair a required missing or contradictory anchor from accepted evidence; ask the user only when the unresolved choice would materially change the desired outcome, authority, cost, or risk.

## Re-grounding

After compaction, handoff, resume, ownership transfer, stage transition, or a requirement that can change the target, reread the active goal and applicable anchor. Recheck the current goal hierarchy, baseline, ownership, intended final state, and unresolved decision boundaries. Summaries and progress cards are supporting context, not replacements for authoritative goal or project state.

Hold affected mutation when the main goal, stage relationship, target surface, ownership, or baseline cannot be re-established. Do not turn the hold into a new repair program unless that repair is the highest-ROI necessary unblocker.

## ROI continuation rule

For each safe and authorized candidate action, compare:

`expected main-goal progress / (time + compute + coordination + operational risk + likely rework)`

A numeric score is optional. The parent must be able to explain why the selected action has the highest reasonable total-goal ROI among available in-scope options.

Prefer direct progress, then necessary unblockers, then risk reduction, then acceptance-critical evidence or cleanup. Reject drift, substitute surfaces, duplicated proof, speculative infrastructure, and low-ROI side work. Required safety, authorization, and final validation are not optional merely because their immediate progress is indirect.

Before continuing after a stage or child result, confirm:

- the result changes the real main-goal gap rather than only a local metric;
- the proposed next stage is necessary for the main goal;
- the action targets the canonical surface;
- its expected benefit justifies cost, risk, and coordination;
- it does not require an ungranted decision or authority boundary.

If these checks fail, select a better aligned action, backlog the side work, request the material decision, or report the blocker.
