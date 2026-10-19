# Agent Engineering Mindset

A deployable OpenClaw skill for keeping repository changes in clean committed worktrees while long-running engineering work remains anchored to one main goal, aligned through necessary stages, prioritized by total-goal ROI, and accepted through one final validation gate.

## Core questions

1. What observable user outcome is the authoritative main goal?
2. How does the current stage measurably advance that goal?
3. Which safe in-scope next action has the highest expected main-goal ROI?
4. What is the adoption mode, target ref, baseline, and integration owner?
5. What exact adopted state will receive the single acceptance validation?
6. What remains between a candidate commit and canonical completion?

## Operating model

```text
main goal
    ↓
necessary current stage
    ↓
highest-ROI aligned next action
    ↓
canonical owned implementation
    ↓
candidate commit
    ↓
integrated target when required
    ↓
one proportionate validation gate
    ↓
worktree and artifact closure
```

## What it governs

- main-goal continuity and measurable stage alignment;
- ROI-based continuation and rejection of drift or substitute work;
- low-overhead heartbeat supervision for eligible mechanical long-running stages;
- canonical ownership and bounded delegation;
- version, workspace, artifact, and runtime state integrity;
- proportionate lifecycle and large-resource handling;
- one final validation gate bound to the actual accepted state.

It does not replace OpenClaw tool policy, permissions, approvals, sandboxing, native goal state, managed task or worktree facilities, or project-specific rules.

## Installation

The repository root is the installable skill package because it contains `SKILL.md`.

```bash
openclaw skills install . --global
```

If replacing an older reviewed copy, inspect the diff first and replace it intentionally:

```bash
openclaw skills install . --global --force
```

For one agent:

```bash
openclaw skills install . --agent <agent-id>
```

## Verification

```bash
openclaw skills info agent-engineering-mindset --json
openclaw skills check --agent <agent-id> --json
```

For agents with an explicit skill allowlist, add `agent-engineering-mindset` without replacing unrelated entries.

## Safety defaults

- A stage goal cannot replace, weaken, or silently broaden the main goal.
- Stage completion and child PASS do not complete the parent goal.
- Low-ROI side work and substitute surfaces are rejected.
- A mechanical long-running stage may use one task-attached heartbeat only when its canonical run, progress markers, and terminal evidence are observable; unchanged checks stay brief, material progress refreshes the heartbeat binding, and terminal state reclaims it.
- One writer owns each overlapping semantic surface; disjoint ownership may proceed independently.
- Every task branch declares `must_integrate`, `research_only`, or `checkpoint_only`, plus its target, baseline, owner, integration owner, and terminal action.
- A candidate commit is not completion. For `must_integrate`, the designated integration owner must place the exact change on the target before the one final acceptance gate.
- A local commit does not authorize publication, deployment, restart, or an otherwise unauthorized merge.
- Every repository change leaves its task worktree with an empty `git status --porcelain=v1 --untracked-files=all`; a pause uses an explicitly unverified checkpoint commit.
- Before creating a task worktree, inventory and reuse compatible registrations; reconcile a partial creation before retrying instead of creating renamed siblings.
- At completion, failure, cancellation, supersession, or indefinite block, preserve unique work and remove the task worktree or deliberately adopt it with a live owner and removal condition.
- Acceptance uses one final validation gate on the exact adopted state; branch checks for `must_integrate` are implementation preflight, not another acceptance gate.
- If that state changes, the prior result is stale and is replaced rather than layered with another gate.
- Runtime must load the accepted configuration, artifact, or revision rather than an accidental scratch or dirty dependency.

## Repository layout

```text
SKILL.md
references/heartbeat-supervision.md
references/state-and-revision-integrity.md
references/artifact-and-resource-lifecycle.md
references/verification-and-runtime.md
```

## License

MIT — see `LICENSE`.
