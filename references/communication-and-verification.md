# Communication and Verification

## Communication Minimality Gate

Default communication principle: shortest accurate answer that preserves decision-relevant facts.

Do not create long explanations just to fit a report template. The report format serves the work; the work does not serve the format.

Hard rules:

- Do not repeat full “goal” and “implementation” sections mechanically when the user asked a simple status, yes/no, confirmation, or narrow factual question.
- Do not expose internal checks, command logs, trace details, hidden reasoning, or full diagnostic process unless the user asks for evidence or those details change the decision.
- Do not narrate obvious process steps when the action or result is enough.
- Do not include broad background, postmortems, or redundant context in short user-visible updates.
- For status or judgment questions, answer with the shortest accurate result first; add only the minimum evidence needed to make it trustworthy.
- For small completed repairs, report changed surface, verification, and remaining risk in a compressed form.
- For project-round wrap-up, acceptance, handoff, manager completion, or decision-boundary reporting, use the required report order, but keep each section terse.
- If the user explicitly asks for depth, evidence, auditability, or a full report, expand only to the requested depth.

Use the full project-round report order only when the message is actually a project-round completion, parent-stage summary, manual completion summary, handoff, acceptance report, or decision/approval boundary.

For ordinary direct answers, status checks, command results, or narrow diagnostics, use a compact answer instead of the full five-section report.

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

## User-Visible Reporting

Keep reports concise.

When reporting parent-stage work, include only decision-relevant lifecycle facts:

- what was reused, repaired, merged, archived, or created;
- why a new entity was necessary, if one was created;
- lifecycle/cleanup path for new, temporary, runtime, or superseded entities;
- any intentionally retained superseded entity and reason;
- whether continuation is allowed, held, or waiting for approval.

Do not dump full stage anchors, registries, lifecycle metadata, internal checks, logs, or reasoning chains unless asked.

Use the required project-round report order only for real project-round wrap-up or manual completion summaries. Do not force the format onto simple status answers.

## Exact Revision Gate

Bind verification and acceptance to one exact candidate commit. A branch name, worktree path, progress report, or earlier PASS is insufficient when the candidate changed. The verifier stays read-only; remediation returns to the current writer unless ownership is explicitly transferred. Rerun relevant checks after canonical merge.

## Verification

Before claiming completion:

- confirm stage anchor was read or repaired when required;
- confirm re-grounding occurred after compaction, handoff, resume, model change, child completion, stage transition, or new requirements;
- confirm only one active writer and writable line existed;
- confirm the verifier evaluated the exact accepted candidate commit;
- confirm proposed next action advances the current stage goal;
- confirm no project-specific vocabulary was added to this generic skill;
- for code/config/prompt/runtime changes, confirm the task workspace, verified commit, and runtime version were not conflated;
- confirm unrelated pre-existing changes were not overwritten, staged, stashed, reset, or absorbed;
- confirm runtime promotion, if any, used an explicitly verified revision rather than a mutable development checkout;
- confirm routing or collaboration layers did not redefine these semantics;
- confirm new entities passed canonical-owner and necessity checks;
- confirm temporary/scratch entities were deleted or promoted;
- confirm superseded entities were archived/trash-cleaned or intentionally retained with reason;
- confirm watchers/crons/daemons created for the task were stopped or adopted by a canonical owner;
- confirm user-visible reporting used the shortest accurate form for the task type;
- confirm protected actions executed only after the response identity, choice, target, and current decision record were validated;
- confirm request delivery, a displayed prompt, silence, or an unrelated affirmative response was never treated as authorization;
- confirm large memory/IO tasks had a resource envelope before dispatch or execution;
- confirm no unbounded full-table loads or broad mounted-drive scans were used without explicit approval;
- confirm memory-pressure recovery did not use cache clearing, unrelated process killing, environment reboot, or service restart without explicit approval;
- for concrete repair diffs, confirm duplicate active implementations were searched, the real positive path passed, the retired path was absent by negative/count check, and relevant health or tests passed.
