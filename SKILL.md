---
name: "agent-engineering-mindset"
description: "Use for any repository code or configuration change and for long-running project work, stage transitions, delegation, merges, and runtime promotion; enforce clean-worktree closure, main-goal alignment, ROI priority, and one final validation gate."
---

# Agent Engineering Mindset

Use this skill for any Git-backed code, configuration, test, build, or repository-documentation change, and when project work can drift across stages, agents, revisions, artifacts, or runtime state. It adds goal continuity, ROI discipline, ownership, state integrity, and clean-worktree closure. It does not replace tool policy, approvals, permissions, or project-specific rules.

## 1. Anchor the main goal and current stage

Treat the user's observable end state as the authoritative main goal. Use a native active goal when one exists; otherwise use the current user request and authoritative project evidence. Do not create a second goal store merely to satisfy this skill.

Define the current stage goal as the smallest measurable outcome that is necessary for the main goal. Record or resolve, only as needed:

- the main goal and completion evidence;
- the current stage goal and how it advances the main goal;
- non-goals and forbidden substitute surfaces;
- the canonical source, data, configuration, artifact, and runtime owners;
- the admitted baseline, active writer, intended final state, and decision boundaries.

A stage goal may narrow execution but must not replace, weaken, or silently broaden the main goal. A blocker, child deliverable, local PASS, new request, or convenient substitute does not become the main goal automatically. Put out-of-stage work in backlog or request an explicit goal or stage change.

Use a durable project-owned stage anchor only when work spans turns, agents, revisions, or a material stage boundary. For short work, keep the anchor in the active task context instead of creating a ledger. Read [stage anchors and continuation](references/stage-anchors-and-continuation.md) when durable continuation state is required.

## 2. Choose the highest-ROI aligned next action

Consider only actions that are safe, authorized, and consistent with the main goal. Compare candidate actions by their expected main-goal progress against total time, compute, coordination cost, operational risk, and future rework. Exact numeric scoring is optional; the ranking and reason must be clear.

Prefer, in order of actual expected value rather than ceremony:

1. direct progress on the true target surface;
2. a bounded unblocker required for direct progress;
3. risk reduction that protects likely progress;
4. evidence or cleanup required for acceptance or safe continuation.

Reject drift, substitute work, speculative infrastructure, duplicated evidence, and low-ROI side work unless they are necessary prerequisites. Do not optimize a stage metric when doing so lowers total main-goal ROI. For the selected action, know its owner, expected delta, cost or risk, acceptance evidence, stop condition, and approval boundary.

## 3. Keep one canonical implementation path

Change the canonical semantic owner instead of adding a parallel owner, entry point, or fallback. Reuse, repair, consolidate, or retire before creating. Give retained artifacts an owner and cleanup, archival, adoption, or removal condition; keep tiny scratch output task-local and remove it when done.

One writer owns each overlapping semantic surface at a time. Independent writers may work concurrently only on clearly disjoint surfaces with explicit ownership. Preserve unrelated state and stop when ownership, baseline, target surface, or authority is ambiguous.

When delegating, provide the objective, authoritative context, exact writable scope, required deliverable, acceptance evidence, and stop conditions. Child output is evidence; it does not redefine or complete the parent goal.

Read [artifact lifecycle](references/artifact-lifecycle.md) when creating or superseding durable artifacts. Read [approval and resource boundaries](references/approval-and-resource-boundaries.md) only for protected actions or genuinely large-resource work.

## 4. Preserve version and runtime integrity

For versioned or runtime-affecting changes, distinguish the admitted baseline, task-owned change, intended final integrated state, and loaded runtime state. Use an isolated worktree when concurrent work, merge risk, or repository policy justifies it; do not impose one on a small uncontested edit.

For every Git-backed change, treat a clean adoption worktree as a hard terminal invariant. Before the first edit, require an empty `git status --porcelain=v1 --untracked-files=all`. If the intended worktree is dirty, do not add another change: reconcile a clearly owned recovery task or use a clean isolated worktree. Never build on unrelated dirt.

When one task spans a Git-backed owner and an unversioned live file, inventory every writable surface before mutating any of them. A non-Git path does not waive repository preflight: check every enclosing repository first, repair the canonical tracked owner in a clean worktree, and synchronize the live file only from the accepted tracked state. If the canonical repository is dirty or ownership is unresolved, do not edit the live file and create a split state.

Never overwrite, stash, reset, stage, absorb, or clean unrelated changes. If the baseline changes or semantic ownership overlaps, reconcile before acceptance. Runtime-affecting work must identify the real loader or entry path and must not depend on an unintended dirty checkout or untracked mutable file.

Keep generated outputs outside the source worktree or in project-owned ignored locations unless they are deliberate task-owned deliverables. For a completed change, use one final acceptance gate: stage only explicit task-owned paths, inspect and check that exact candidate, create a scoped local commit without changing its tree, and require the porcelain status to be empty before the gate returns PASS. A code-changing task, stage, pause, or handoff is not closed while its worktree contains staged, unstaged, or untracked task output. If work must pause before acceptance, preserve the bounded state in an explicitly unverified checkpoint commit and still leave the worktree clean; never merge or run that checkpoint as accepted. A local commit does not authorize push, merge, deployment, restart, or publication. If an explicit instruction forbids commits, stop before editing because a persistent repository change would conflict with this invariant.

Treat a task-created isolated worktree as temporary execution infrastructure. Before creating one, inventory registered worktrees and the intended branch and path, then reuse a compatible existing worktree instead of creating retry, `verify2`, or `v2` siblings. If creation partially fails, reconcile the exact path, registration, branch or detached revision, and files before retrying; resume the same instance or roll back only proven task-created partial state with no unique work.

At every terminal outcome—integrated completion, failure, cancellation, supersession, or indefinite block—preserve unique work on a named ref or explicit checkpoint when needed, then remove the worktree registration and directory, or deliberately adopt it with a live owner, reason, reference, and removal condition. A bounded pause or handoff may retain it only with a live owner and explicit next-use or removal condition. Integrated acceptance closes the worktree lifecycle within the same final gate. Empty porcelain alone is not worktree closure. Never force cleanup when ownership, unintegrated work, or active use is unresolved.

Read [version, workspace, and runtime integrity](references/version-workspace-runtime.md) before every Git-backed change and whenever handoff, merge, or runtime promotion is involved.

## 5. Use one final validation gate

Every change has exactly one acceptance gate, performed after the change reaches the final state intended for adoption. The gate may contain several proportionate checks, but they form one validation pass and one acceptance decision.

Bind that validation to the exact final state: the task-owned diff, candidate revision, merged revision, generated artifact, configuration, or loaded runtime actually being accepted. Choose the smallest meaningful evidence set, such as the real positive entry path, targeted tests, an absence or negative check for retired behavior, and runtime health when runtime changed.

Do not require a task-branch PASS, a separate independent verifier PASS, and a second merged-tree PASS as cumulative gates. When integration changes the accepted state, validate the final integrated state once. An independent verifier may perform this single gate when risk or project policy warrants it, but it is not an additional mandatory layer.

If the accepted state changes after validation, the result becomes stale. Replace it with one new final validation; do not accumulate validation layers. On failure, return to the same owned implementation path, repair it, and run the single final gate again.

Read [communication and single validation](references/communication-and-verification.md) when defining or reporting acceptance evidence.

## 6. Continue or close against the main goal

After a stage completes, reassess the remaining main-goal gap and choose the next highest-ROI aligned action. Stage completion, artifact creation, a commit, or child completion is not main-goal completion.

Claim completion only when the main observable outcome is satisfied, the final accepted state passed the single validation gate, unrelated state was preserved, and temporary or superseded entities were cleaned, archived, or adopted. Otherwise continue, hold for a real decision boundary, or report the evidenced blocker.

## Completion check

- the main goal remained authoritative and the stage goal demonstrably advanced it;
- the chosen action had the highest reasonable main-goal ROI among safe in-scope options;
- one canonical implementation path and unambiguous write ownership were preserved;
- the exact final accepted state passed one proportionate validation gate;
- every task worktree creation began with an exact registration, branch, and path inventory; compatible worktrees were reused and partial creation failures were reconciled before retry;
- every Git worktree that received or adopted the task change has empty porcelain status;
- every task-created isolated worktree was removed at its terminal outcome or deliberately adopted with a live owner, reason, reference, and removal condition;
- runtime, artifacts, and lifecycle state agree with the accepted outcome;
- remaining main-goal gaps, risks, or blockers are stated without promoting stage-local success to completion.
