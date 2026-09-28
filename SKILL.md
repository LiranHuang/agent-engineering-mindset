---
name: "agent-engineering-mindset"
description: "Long-running project work, delegation, merges, and runtime promotion with anchored single-writer verified delivery."
---

# Agent Engineering Mindset

Use this skill for long-running project work, parent/executor coordination, stage transitions, artifact acceptance, merges, and runtime promotion. It guides behavior but does not replace tool policy, sandboxing, approvals, or managed-worktree enforcement. Stop mutation when a required invariant cannot be proven.

## Procedure

### 1. Establish one project contract

Read one project-owned stage anchor before dispatching, editing, accepting, merging, or continuing. Resolve the goal and non-goals; canonical source, data, and runtime owners; canonical and admitted base revisions; true, allowed, and forbidden surfaces; active run, writer, and writable worktree; candidate commit and verifier state; decision boundaries; and lifecycle ownership.

Use an existing roadmap, issue, state file, or equivalent canonical artifact. Keep project state out of this generic skill. If the anchor is missing, stale, unreadable, or contradicted, stop and repair it from accepted evidence or ask for the missing decision.

Consult [`references/stage-anchors-and-continuation.md`](references/stage-anchors-and-continuation.md). Complete this step when one current anchor and one true target surface govern the next action.

### 2. Re-ground after every context transition

After compaction, handoff, resume, model change, child completion, stage transition, or new requirements, reread the anchor and verify the goal, canonical revision, Git status, run, writer, worktree, candidate, and verifier state. Chat summaries and progress cards are not authoritative project state. Backlog out-of-stage requirements or request an explicit stage change; blockers do not become goals automatically.

Complete this step only when continuation is derived from current project evidence.

### 3. Select the smallest aligned next action

Classify the action as direct progress, unblock, risk reduction, substitute work, or drift. Reject low-value side work unless it is a bounded prerequisite. Identify the owner, expected delta, acceptance evidence, stop conditions, and approval boundaries before execution.

For each protected action, record the target, authorized approver, allowed choices, and evidence required before execution. A request or displayed prompt alone never grants authority.

Complete this step when the action advances the anchored stage goal on the true target surface.

### 4. Admit exactly one writable execution line

Maintain one active run, writer, writable branch/worktree, admitted base, and candidate per stage. Parallel lanes stay read-only. Never add a writer to accelerate, repair, replace, or unblock; the parent also stays read-only while an executor owns writes. Transfer only after the prior writer is terminal, its state is preserved, and the anchor records the transfer.

Before admission, inspect the canonical checkout, active sessions, registered worktrees, branch, base, and pre-existing changes. Hold on another writer, ambiguous ownership, unexpected canonical dirt, or base mismatch.

Complete this step when one unambiguous owner controls one isolated writable line.

### 5. Dispatch a bounded contract

Every implementation dispatch states the anchor and run; objective and non-goals; base and worktree owner; allowed and forbidden paths; authoritative inputs; expected candidate; tests and acceptance criteria; stop conditions; and return evidence.

Do not dispatch vague instructions such as “continue,” “finish the project,” or “fix everything.” A child may not redefine the stage, create a parallel canonical path, merge, publish, restart runtime, or clean unrelated state unless the dispatch grants that boundary explicitly.

Complete this step when the child can determine success and mandatory stop conditions without inventing scope.

### 6. Reuse before creating

Apply:

`reuse existing -> repair canonical owner -> merge duplicates -> archive superseded -> create only when necessary`

For every new entity, identify canonical owner, purpose, scope, status, reference, and cleanup or promotion condition. Reject parallel entrypoints, duplicate owners, per-run managers, and compatibility paths without removal conditions.

Consult [`references/artifact-lifecycle.md`](references/artifact-lifecycle.md). Complete this step when every new or superseded entity has a lifecycle decision.

### 7. Bound resources and durable ownership

Use official durable tasks, sessions, managed worktrees, automations, approvals, and completion delivery for work that may outlive a turn. Never leave a bare background process as the only continuation path or let its owning session finish while it remains active.

For large IO, record sizes, memory limits, chunking, filters, concurrency, spill location, partial progress, and stop condition. Prefer bounded streaming and one worker. Stop with reusable progress rather than clearing caches, killing unrelated processes, or restarting services without approval.

Consult [`references/approval-and-resource-boundaries.md`](references/approval-and-resource-boundaries.md). Complete this step when lifecycle ownership and resource bounds are explicit.

### 8. Preserve version and workspace integrity

For code, config, prompt, runtime, or workflow changes, enforce:

`isolated task worktree -> task-owned diff -> tested candidate commit -> independent verification -> merged canonical branch -> clean canonical runtime checkout`

Record the base revision and pre-existing changes before editing. Never overwrite, stash, reset, stage, absorb, or clean changes outside the active run. Stage only explicit task-owned paths. Keep disposable output in task scratch or project artifact locations with cleanup rules.

A checkpoint commit preserves work but is not verified. A task-branch PASS does not prove the merged tree. If the base changes, ownership overlaps, required untracked files appear, or a semantic owner changed on both branches, stop and reconcile before repeating verification.

Consult [`references/version-workspace-runtime.md`](references/version-workspace-runtime.md). Complete this step when task workspace, candidate commit, canonical checkout, and runtime revision are distinct and evidenced.

### 9. Enforce an exact verifier gate

Verification names the exact candidate commit and stage anchor revision it evaluates. The verifier stays read-only and returns PASS or FAIL with tests and bounded evidence; it never repairs the candidate it judges.

Accept PASS only when the evaluated commit equals the current candidate, descends from the admitted base or an explicitly accepted replacement, stays within authorized scope, passes required tests through the real entry path, needs no untracked mutable dependency, and has not been superseded.

FAIL returns work to the same write line unless ownership is explicitly transferred. Do not create a replacement implementation line. Do not merge, promote runtime, close the stage, or start the next stage while verification is pending, stale, or failed. After merge, rerun relevant checks against the merged canonical tree.

Consult [`references/communication-and-verification.md`](references/communication-and-verification.md). Complete this step when acceptance is bound to one exact revision.

### 10. Fail closed on drift

Stop mutation on anchor conflict; missing or stale state; a second writer; ambiguous ownership; unexpected base or canonical dirt; path escape; owner conflation; candidate/verifier mismatch; changed required inputs; failed re-grounding; or cleanup, reset, migration, publication, or promotion beyond authority. Do not convert the stop into a repair task without updating the anchor and obtaining authority; preserve evidence instead of destructive cleanup.

Complete this step only when the invariant is restored or the authorized owner decides the boundary.

### 11. Merge, promote, and close canonically

Refresh the canonical branch and compare all changes since the admitted base. Inspect textual and semantic conflicts. If the baseline changed, update the task branch and rerun verification. Merge with normal Git semantics, inspect the merged diff, and retest the merged tree.

Run or restart services only from a clean canonical checkout at an explicitly verified merged commit. Verify working directory, loaded revision, health, and relevant behavior. Never run from an agent worktree, dirty checkout, unmerged branch, or uncommitted dependency.

After child completion, treat its output as evidence. Update parent state and continue until the anchored outcome is complete or genuinely blocked. Clean disposable outputs; archive or adopt stage artifacts; stop or adopt temporary watchers and owners; archive superseded entities after replacement verification.

Complete this step when the anchor, canonical tree, verification evidence, runtime state when applicable, and lifecycle cleanup agree.

## Verification checklist

Before claiming completion, confirm:

- one anchor governed the work and was re-grounded after context transitions;
- one writer owned one admitted base, worktree, and candidate;
- dispatch carried scope, tests, stop conditions, and return evidence;
- the verifier evaluated the exact accepted commit;
- worktree, canonical merge, and runtime revision stayed distinct;
- unrelated state was preserved and lifecycle cleanup completed;
- long-running work retained an official completion path;
- the report states accepted revision, verification, remaining risk, and continuation status.
