# Agent Engineering Mindset

A reusable agent skill for anchoring engineering work to one main goal,
necessary stages, total-goal ROI, canonical ownership, and one impact-scoped
validation gate. Ordinary reversible Git edits use a lightweight direct-target
path; audit, ledger, worktree, runtime, and destructive-operation controls are
added only when their risk boundary applies.

Use it with Codex, DeepSeek Harness (DSH), OpenClaw, or another agent runtime
that can discover and load `SKILL.md` packages. Each runtime keeps its native
tools, permissions, task tracking, and supported model controls.

## Core questions

1. What observable user outcome is the authoritative main goal?
2. How does the current stage measurably advance that goal?
3. Which safe in-scope next action has the highest expected main-goal ROI?
4. Can the bounded change safely use the direct target, or is an exceptional
   workspace or adoption contract actually required?
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
- canonical ownership; delegation is owned by the companion `task-collaboration` skill;
- version, workspace, artifact, and runtime state integrity;
- proportionate lifecycle and large-resource handling;
- one final validation gate bound to the actual accepted state.

It does not replace the host runtime's tool policy, permissions, approvals,
sandboxing, native goal state, task or worktree facilities, or project rules.

## Installation

The repository root is the installable skill package because it contains `SKILL.md`.
The companion package is `skills/task-collaboration/`. Install its contents as
the sibling skill `task-collaboration` in the runtime's skill directory; keep it
discoverable, but load it only for delegation or handoff. Installing this root
alone does not install the companion.

Place each package in the host's configured skill discovery directory:

| Runtime | Package destination |
| --- | --- |
| Codex | `~/.codex/skills/<skill-name>/` |
| DeepSeek Harness | `~/.dsh/skills/<skill-name>/` |
| OpenClaw | Its configured skill directory or skill installer |
| Other runtimes | Their supported `SKILL.md` discovery directory |

Copy the root `SKILL.md` and `references/` into `agent-engineering-mindset/`;
copy `skills/task-collaboration/` into the sibling `task-collaboration/`.
Preserve relative reference paths and runtime-specific adaptations. In DSH,
use native Goal/task tracking rather than introducing a second execution card.

### OpenClaw installation example

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

Refresh the runtime's skill catalog using its supported mechanism and confirm
both packages are visible and enabled. Check that solo work skips collaboration,
delegation loads `task-collaboration`, and engineering references resolve.
Existing sessions may require a catalog refresh or a new session.

For OpenClaw, use its runtime-specific checks:

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
- Ordinary reversible Git edits do not require an audit child, ledger entry,
  process scan, project-wide cleanup sweep, worktree, or explicit adoption state
  machine solely because a tracked file changes.
- b23 runs once only immediately before a fully prepared final action with no
  reliable authorized rollback or recovery; ordinary review stays with the parent.
- Inspect worktree registrations, writers, and processes only when concurrency,
  runtime mutation, handoff, drift, or project policy makes them relevant.
- The ordinary contract is direct target, current branch/HEAD, must-integrate,
  and parent-owned integration. Record an explicit contract only for exceptions.
- A candidate commit is not completion. For `must_integrate`, the designated integration owner must place the exact change on the target before the one final acceptance gate.
- A local commit does not authorize publication, deployment, restart, or an otherwise unauthorized merge.
- Stage only task-owned paths and preserve unrelated state. A bounded review or
  experiment may remain uncommitted but is not integrated completion.
- Create or reuse a task worktree only for evidenced isolation needs; inventory and reuse compatible registrations, and reconcile a partial creation before retrying instead of creating renamed siblings.
- At completion, failure, cancellation, supersession, or indefinite block, preserve unique work and remove the task worktree or deliberately adopt it with a live owner and removal condition.
- Acceptance uses one final validation gate on the exact adopted state; branch checks for `must_integrate` are implementation preflight, not another acceptance gate.
- If that state changes, the prior result is stale and is replaced rather than layered with another gate.
- Runtime must load the accepted configuration, artifact, or revision rather than an accidental scratch or dirty dependency.

## Repository layout

```text
SKILL.md
skills/task-collaboration/SKILL.md
references/heartbeat-supervision.md
references/state-and-revision-integrity.md
references/artifact-and-resource-lifecycle.md
references/verification-and-runtime.md
```

## License

MIT — see `LICENSE`.
