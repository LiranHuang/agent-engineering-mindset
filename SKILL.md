---
name: "agent-engineering-mindset"
description: "Project continuation, stage gates, isolated worktrees, verified merges, artifact lifecycle, resource bounds, and clean canonical runtime."
---

# Agent Engineering Mindset

Use this skill for parent-owned project work, autonomous continuation, manager/executor coordination, decision gates, stage alignment, and tasks that create or accept project artifacts, state, watchers, agents, scripts, configs, commits, merges, or runtime entities.

This skill owns parent engineering gates and includes the minimum standalone checks needed to verify concrete code, config, prompt, runtime, or workflow changes.

## Procedure

### 1. Resolve the stage anchor

Locate and read the current project-owned stage anchor before choosing, accepting, dispatching, merging, or continuing work. Resolve the stage goal, total goal, continuation base, true target surface, forbidden substitutes, allowed support surfaces, superseded routes, decision boundaries, protected boundaries, and lifecycle ownership.

Populate `continuation_base_check`, `target_surface_check`, `progress_delta_check`, `supersede_check`, `project_vocabulary_check`, `artifact_minimality_check`, and `lifecycle_check`.

If an expected anchor is missing, stale, unreadable, or contradicted, stop normal continuation and repair the project-owned anchor from accepted evidence or ask for the missing goal/target decision. Never write project state into this generic skill.

Consult [`references/stage-anchors-and-continuation.md`](references/stage-anchors-and-continuation.md). Complete this step when one current anchor and one true target surface are explicit.

### 2. Select the smallest aligned next action

Check that the action advances the current stage goal on the true target surface. Classify it as direct progress, unblock, risk reduction, substitute work, or drift. Reject low-ROI side work unless it is a bounded prerequisite.

Identify approval boundaries before execution. For every protected action, record the decision, authorized approver, allowed choices, expiry or nonce when applicable, and evidence required before execution. A request or delivered prompt alone never grants authorization; validate the response against the active decision record.

Complete this step when the next action, owner, expected delta, acceptance evidence, and approval state are explicit.

### 3. Reuse before creating

Apply:

`reuse existing -> repair existing -> merge duplicates -> archive stale -> create only when necessary`

Before creating any entity, identify its canonical owner, purpose, scope, lifecycle status, cleanup/archive condition, and reference. Reject parallel paths, duplicate semantic owners, and per-run managers/watchers/state owners.

Consult [`references/artifact-lifecycle.md`](references/artifact-lifecycle.md) when files, scripts, config keys, prompts, skills, agents, watchers, services, registries, reports, branches, or runtime entrypoints may be added or superseded. Complete this step when every new or superseded entity has a lifecycle decision.

### 4. Bound resources before high-throughput work

For large datasets, models, logs, media, browser traces, or mounted and network-backed paths, record expected input/output size, RSS soft/hard limits, chunking/streaming strategy, required-column filters, concurrency, spill location, cache/dirty-page risk, partial-progress artifacts, and stop condition.

Prefer bounded streaming and one worker. Stop at memory/IO boundaries with reusable progress; do not normalize cache clearing, process killing, environment reboot, or service restart as recovery.

Consult [`references/approval-and-resource-boundaries.md`](references/approval-and-resource-boundaries.md). Complete this step when the resource envelope is explicit or the task is proven small.

### 5. Isolate mutable implementation

For code, config, prompt, runtime, or workflow changes, enforce:

`isolated task worktree -> verified commit -> merged canonical branch -> clean canonical runtime checkout`

Record the canonical base revision. Give every independent writer its own branch/worktree. Never overwrite, stash, reset, stage, or absorb unowned changes. Keep conflicting paths read-only until ownership is resolved. Put disposable output in task scratch or a project artifact location with cleanup rules.

Consult [`references/version-workspace-runtime.md`](references/version-workspace-runtime.md). Complete this step when write ownership, base revision, allowed paths, and artifact locations are explicit.

### 6. Verify the task-owned semantic unit

Stage only explicit task-owned paths. Inspect staged changes and run the smallest meaningful tests through the real entry path. A checkpoint commit preserves work but is not mergeable or runnable evidence.

For concrete repairs, search for duplicate active implementations, run a positive path through the real entrypoint, run a negative or count-based check for the retired path, and verify relevant health or tests. Complete this step when the task commit and evidence are reproducible.

### 7. Merge and promote canonically

Refresh the canonical branch, compare all changes since the recorded base, and inspect textual plus semantic conflicts. Update and retest if the baseline changed. Merge with normal Git semantics, inspect the merged diff, and rerun tests on the merged tree.

Run or restart services only from a clean canonical checkout at an explicitly verified merged commit. Verify service working directory, loaded revision, health, and relevant behavior. Never run from an agent worktree, dirty checkout, unmerged branch, or uncommitted files.

Complete this step when the canonical tree passes or promotion is explicitly held.

### 8. Continue parent ownership after child completion

Treat child output as evidence, not parent completion. Compare it with the stage anchor and acceptance contract. Decide success, retry, repair, approval, return to mainline, or blocker. Do not pause merely because a child produced a report, commit, or local PASS.

Complete this step when parent state is updated and the original requested outcome is achieved or genuinely blocked.

### 9. Clean lifecycle and report minimally

Delete disposable/scratch outputs; archive or adopt stage artifacts; stop/adopt watchers, cron jobs, daemons, and temporary owners; archive superseded entities once replacements are verified.

Use the shortest accurate user-visible report. For a narrow result, report only changed surface, verification, and remaining risk. Use project-round reporting only for actual stage completion, handoff, acceptance, or decision boundaries.

Consult [`references/communication-and-verification.md`](references/communication-and-verification.md). Complete this step when no unmanaged temporary or active entity remains and the report preserves only decision-relevant facts.

## Verification checklist

Before claiming completion, confirm:

- the stage anchor and true target surface were resolved;
- the action advanced the current stage goal;
- task worktree, verified commit, canonical merge, and runtime revision were not conflated;
- no unrelated changes were overwritten, staged, stashed, reset, or absorbed;
- new entities passed necessity and lifecycle gates;
- temporary, scratch, and superseded entities were cleaned or intentionally retained;
- watchers, cron jobs, and daemons were stopped or adopted;
- resource-intensive work had a bounded envelope;
- protected actions were executed only after an authorized response matched the active decision record;
- the final report is the shortest accurate form for the task.
