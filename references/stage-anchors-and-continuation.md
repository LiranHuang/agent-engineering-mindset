# Stage Anchors and Parent Continuation

## Stage Anchor Contract

Before parent continuation can pass, resolve a `stage_anchor`.

A stage anchor is a project-owned artifact or document that records:

- current stage goal;
- continuation base artifacts and metrics;
- true target surface;
- forbidden substitute surfaces;
- allowed support surfaces;
- superseded routes, if any;
- active decision boundaries, if any;
- protected boundaries not authorized by the current stage;
- lifecycle ownership for stage-level artifacts when applicable;
- canonical source, data, and runtime owners;
- admitted base revision, active run, active writer, and writable worktree;
- allowed and forbidden paths;
- candidate commit and verifier state.

Before choosing, accepting, dispatching, merging, or continuing work, the parent must:

1. Locate the current project stage anchor from dispatch, registry, project policy, roadmap, or known project anchor path.
2. Read the anchor.
3. Populate the parent gates:
   - `continuation_base_check`
   - `target_surface_check`
   - `progress_delta_check`
   - `supersede_check`
   - `project_vocabulary_check`
   - `artifact_minimality_check`
   - `lifecycle_check`
4. Cite the anchor in dispatch, parent acceptance, parent merge, decision registry, or final report when relevant.

Hard fail if a stage anchor is expected but missing, unreadable, stale, or contradicted by the proposed next action.

If no reliable current stage anchor exists, or if the existing anchor is stale or contradicted by accepted evidence, stop normal continuation and do one of:

- create a project-owned stage anchor from accepted evidence;
- update the project-owned stage anchor to cite latest accepted parent-state evidence;
- mark stale routes as superseded and update the anchor;
- ask the user for missing stage goal or target surface if it cannot be inferred safely.

Stage anchors must be written only inside the project directory or other project-owned state location, never into generic skills.

## Context Re-grounding Gate

Treat compaction, handoff, resume, model change, child completion, stage transition, and new requirements as re-grounding events. Reread the anchor and verify the current goal, canonical revision, Git status, active run, active writer, writable worktree, candidate commit, and verifier state. Chat summaries and progress cards are not authoritative project state.

Place requirements outside the current stage into backlog or request an explicit stage change. A blocker may justify bounded analysis but does not become the new stage goal automatically. Hold mutation when current state cannot be re-proven.

## Parent Continuation Gate

Before continuing after child completion, partial success, discovery of a new blocker layer, or a stage transition, the parent must decide:

- Does the action advance the current stage goal?
- Does it operate on the true target surface rather than a substitute surface?
- Does it produce direct progress, unblock progress, reduce risk, or merely create local substitute work?
- Does it require a user decision boundary?
- Does it require a new entity, and if so, did it pass artifact minimality?
- Does every new or superseded entity have a lifecycle decision?

A child local PASS does not become parent stage progress unless it cites or updates the stage anchor and passes these checks.


## Gate Outcomes

Allowed/positive outcomes:

- `pass_stage_anchor_repair`: anchor was created or updated from accepted evidence, with no direct target delta claimed.
- `pass_reuse_existing_entity`: work reused, repaired, or extended the canonical owner instead of creating a duplicate.
- `pass_new_entity_with_lifecycle`: a new entity was necessary and lifecycle metadata was recorded.
- `pass_superseded_entity_archived`: old entity was safely archived/cleaned after replacement.
- `pass_communication_minimality`: user-visible output used the shortest accurate form for the task type.
- `pass_resource_envelope`: task has explicit bounded RSS/IO/cache strategy.

Hold/fail outcomes:

- `fail_missing_stage_anchor`: no reliable anchor exists for a project that requires one.
- `fail_anchor_conflict`: proposed next action contradicts the current anchor.
- `fail_unnecessary_entity`: proposed or created entity is redundant or lacks necessity.
- `fail_unmanaged_lifecycle`: new entity lacks owner, scope, status, cleanup/archive condition, or reference.
- `fail_overreported`: output was expanded only to satisfy a template, exposed unnecessary internal process, or repeated mechanical report sections for a narrow answer.
- `hold_cleanup_required`: work is otherwise complete, but temporary/scratch/superseded/runtime entities need cleanup before final wrap-up.
- `hold_resource_envelope_required`: task may touch large resources but lacks bounds.
- `blocked_memory_boundary`: task stopped at the configured memory/IO stop condition with reusable partial progress.
