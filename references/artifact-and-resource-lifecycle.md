# Artifact and resource lifecycle

Use this reference when creating, replacing, retaining, or cleaning artifacts, or when a task may consume substantial memory, disk, network, or mounted-storage bandwidth.

## Minimize entities and make their lifecycle visible

Before creating a file, helper, configuration surface, generated artifact, temporary runner, compatibility path, cache, or runtime entry point, first ask whether an existing canonical owner can be repaired, extended, or consolidated. A new entity is justified only when it has a distinct necessary responsibility, reduces a real risk, or is a bounded disposable validation artifact.

## Maintain one canonical project artifact ledger

Each project must declare exactly one canonical artifact ledger and the bounded output roots it governs. Reuse an existing project registry when it can satisfy this contract; never create a second ledger for a task, agent, storage device, output class, or run. Cross-project artifacts belong to one project's ledger and are referenced elsewhere. Resolve the ledger before creating or retaining any file or data artifact, including scratch, generated output, caches, logs, reports, archives, datasets, databases, models, checkpoints, media, worktrees, and output outside the repository.

This generic skill must not contain concrete project names, repository paths, drive or mount identifiers, business-layer names, branch names, scheduler names, or project-specific cleanup destinations. Those values belong only in the active project's instructions, canonical configuration, ledger, or user request. Examples in this reference remain abstract so that one project's storage policy cannot leak into another project.

Register each artifact at creation. One entry may cover a coherent artifact set only when it records an exact directory or manifest; an open-ended glob or entire drive is not a valid entry. Each entry contains `artifact_id`, exact `location`, `owner`, `producer`, `purpose`, `classification`, lifecycle `state`, `created_at`, `updated_at`, `references`, and `terminal_evidence`. `classification` is exactly `temporary` or `long_term`.

A `temporary` entry additionally records exactly one `cleanup_trigger` object. Its `type` is `deadline`, `event`, or `gate`, and its `value` is the exact time, lifecycle transition, or evidence threshold to evaluate. Choose the most reliable type for that artifact and do not populate unused types. Normal deletion safety and authority checks still apply but are not additional cleanup triggers. A `long_term` entry records `retention_basis`, `live_consumer`, `review_condition`, and `retirement_condition`; long-term does not mean permanent by default.

Use one closed loop: `enter project -> resolve ledger -> execute due scoped cleanup -> register new output -> classify -> observe trigger -> execute cleanup -> reconcile -> record terminal evidence`. Cleanup is an active parent duty. Load and evaluate the ledger on first project entry before creating output, immediately after a relevant event or gate transition, at task completion, stage transition, failure, cancellation, replacement acceptance, and before the final answer. Evaluate task-owned entries plus other entries whose triggers are now due and safely actionable within the current scope. When the selected trigger is satisfied and the exact artifact is owned, inactive, safe to remove, and within current authority, delete it immediately and close the entry with time and evidence; do not merely inventory or recommend deletion.

If a `deadline` lies beyond the active task window, exact-time cleanup requires a live project-owned scheduler or cleanup runner bound in the ledger with observable execution evidence. Without it, evaluate the deadline on the next agent entry and never claim that policy text alone provides real-time cleanup.

An unregistered artifact created or adopted by the task under a declared output root is an orphan. Prove and register its ownership and retention, or safely remove it. A task-owned ledger target that is missing must be repaired or closed with evidence. If cleanup is blocked by ownership, active use, permissions, or safety, set the entry to blocked, record the exact blocker, next owner, and next cleanup point, and report incomplete cleanup. Inherited project-wide inventory gaps remain visible lifecycle debt with a responsible owner and next review point; they block an otherwise unrelated task only when they make that task's ownership, safety, final state, or acceptance evidence ambiguous. Do not claim completion while an eligible task-owned temporary entry, an accepted due cleanup item, or an orphan created by the task remains. Do not use cleanup to erase evidence, unrelated work, or an unresolved ownership conflict.

For Git-backed work, task-created output that appears in `git status --porcelain=v1 --untracked-files=all` must be adopted in the scoped task commit or removed or relocated by its owner before completion, pause, or handoff. An ignored location is acceptable only when the project deliberately owns that output class; do not add an ignore rule merely to hide unexplained dirt.

## Use a resource envelope only when scale creates risk

For work on large datasets, logs, media, model artifacts, browser traces, mounted drives, or similarly high-throughput resources, choose bounds before execution:

- expected input and output scale, plus a memory or storage stop threshold;
- filtering, required-column reads, streaming, chunks, or shards instead of broad materialization;
- bounded concurrency and a deliberate spill/output location; and
- reusable partial-progress information and a safe stop condition.

Prefer the smallest resource use that can answer the question. If pressure appears, stop with reusable progress rather than clearing caches, terminating unrelated processes, or treating disruptive recovery as routine. Small local edits do not need a formal envelope.
