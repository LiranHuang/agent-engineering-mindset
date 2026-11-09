---
name: agent-engineering-mindset
description: Engineer an explicit native Goal or complex multi-stage technical task across integration, lifecycle, worktrees, runtime, or migration. Do not use for an ordinary bounded code edit.
---

# Agent Engineering Mindset

Use only for an explicit native Goal or work whose stages, adoption state,
artifacts, runtime, migration, or worktrees can make local progress misleading.
Applicable `AGENTS.md` remains authoritative; this skill adds conditional
mechanics, not another goal, ledger, permission, or acceptance system.

## Engineer the stable Goal and mutable card

Translate the user's original intent into one stable native Goal contract:

```text
Main goal: <observable end state>

Required results:
1. <independently closable result>

Execution constraints:
- <durable scope, authority, acceptance, or stop boundary>
```

Never put progress, current stage, transient blockers, child state, or tool
activity in the native objective. Keep those only in the parent thread's compact
sequential card:

```text
Goal Execution Card
Main goal: <same observable end state>
Progress: <completed>/<required>

1. [√|→|×|!] <result>
   Progress: <one short current-state sentence>
```

Count only adopted `[√]` results. Normal serial work has exactly one `[→]`:
the first unfinished unblocked result. `[!]` requires a real external decision,
authority, input, or state change. Update the card only after adoption,
completion, a real blocker, user scope change, or dependency change. Resume it
after compaction or handoff; reconstruct from the Goal and adopted evidence only
when missing. Complete the Goal only at `N/N` with the observable outcome true.

## Stay anchored and proportional

Advance the current card result through the highest-ROI safe action. Fold files,
commits, tests, audits, ledger work, and child assignments into that result; do
not turn them into subgoals. Reject speculative infrastructure, duplicate
evidence, unrelated cleanup, and convenient later-stage work.

Repair the canonical owner. Re-ground only after integration, handoff,
conflicting evidence, authoritative-input change, baseline mismatch, overlapping
ownership, or uncertain runtime identity.

For ordinary reversible Git work, use the current checkout, exact-path staging,
one focused check, and one final acceptance decision. Follow a project-declared
ledger without duplicating its protocol here.

For delegation or agent handoff, load `task-collaboration`; solo work skips it.

## Load detail only when needed

Read exactly the relevant reference:

- [state and revision integrity](references/state-and-revision-integrity.md):
  isolated worktrees, handoff, non-current targets, checkpoints, or drift;
- [artifact and resource lifecycle](references/artifact-and-resource-lifecycle.md):
  managed outputs, destructive lifecycle actions, ledger migration, or large I/O;
- [verification and runtime](references/verification-and-runtime.md):
  integration, runtime loading, migration, data integrity, or high-risk evidence;
- [heartbeat supervision](references/heartbeat-supervision.md):
  a deterministic background stage that outlives a normal turn.

Do not read every reference for ordinary work. Project-specific paths, commands,
schemas, and cleanup methods belong in project instructions.

## Close the adopted result

Use direct target by default; isolate only for a real conflict, overlapping
writer, handoff/merge boundary, active mutator, material integration risk, or
project rule. An isolated candidate is incomplete until integrated when
integration is required. Preserve unique work, then remove the worktree at every
terminal outcome unless a durable owner and removal condition justify retention.

Run one impact-scoped final gate on the adopted state. Reuse immutable evidence,
widen checks only for invalidated invariants, and prove the real loader when
runtime changed. Temporary outputs must already be cleaned at last use or remain
blocked under a live owner. The parent alone accepts the result and updates the
card.
