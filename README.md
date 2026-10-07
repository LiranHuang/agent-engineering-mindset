# Agent Engineering Mindset

A deployable OpenClaw skill for keeping repository changes in clean committed worktrees while long-running engineering work remains anchored to one main goal, aligned through necessary stages, prioritized by total-goal ROI, and accepted through one final validation gate.

## Core questions

1. What observable user outcome is the authoritative main goal?
2. How does the current stage measurably advance that goal?
3. Which safe in-scope next action has the highest expected main-goal ROI?
4. Are we changing the canonical target with clear ownership?
5. What exact final state will receive the single acceptance validation?
6. What remains between a stage-local result and main-goal completion?

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
final intended state
    ↓
one proportionate validation gate
    ↓
continue against remaining main-goal gap or close
```

## What it governs

- main-goal continuity and measurable stage alignment;
- ROI-based continuation and rejection of drift or substitute work;
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
- One writer owns each overlapping semantic surface; disjoint ownership may proceed independently.
- A local commit does not authorize publication, merge, deployment, or restart.
- Every accepted repository change ends in a scoped local commit and an empty `git status --porcelain=v1 --untracked-files=all`; a pause uses an explicitly unverified checkpoint commit.
- Acceptance uses one final validation gate on the exact state intended for adoption.
- If that state changes, the prior result is stale and is replaced rather than layered with another gate.
- Runtime must load the accepted configuration, artifact, or revision rather than an accidental scratch or dirty dependency.

## Repository layout

```text
SKILL.md
references/stage-anchors-and-continuation.md
references/version-workspace-runtime.md
references/artifact-lifecycle.md
references/approval-and-resource-boundaries.md
references/communication-and-verification.md
```

## License

MIT — see `LICENSE`.
