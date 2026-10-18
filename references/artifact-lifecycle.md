# Artifact Minimality and Lifecycle

## Artifact Minimality Gate

Principle: 如无必要，勿增实体.

Default action order:

`reuse existing -> repair existing -> merge duplicates -> archive stale entity -> create new entity only when necessary`

An entity includes, but is not limited to:

- files and directories;
- scripts and helper tools;
- config keys and config files;
- prompt blocks and policy blocks;
- skills and skill proposals;
- agents, managers, watchers, cron jobs, daemons, or services;
- state files, registries, task cards, reports, generated artifacts, caches, tables, branches, or runtime entrypoints.

Before creating any new entity, answer:

1. Is there an existing canonical owner that should be updated instead?
2. Can the existing entity be repaired, extended, or merged rather than duplicated?
3. Would the new entity create duplicate semantics, duplicate ownership, duplicate entrypoints, or old/new logic coexisting?
4. Is the entity temporary, scratch, stage artifact, active asset, compatibility shim, registry/state, watcher/cron/daemon, or disposable validation output?
5. What is its owner, purpose, scope, lifecycle status, cleanup/archive condition, and reference path?

Creating a new entity is allowed only when at least one is true:

- no existing owner can responsibly hold the behavior or artifact;
- the new entity materially reduces complexity or isolates risk;
- it is a bounded temporary validation artifact with an explicit cleanup path;
- it is a required project-owned stage artifact;
- it replaces an obsolete entity, and the old entity is removed, merged, archived, or explicitly retained with a reason.

Reject or hold when a proposed entity:

- bypasses bad logic by adding a parallel path;
- copies a system because the existing system was not understood;
- duplicates a report, registry, state file, script, prompt block, service, or semantic owner;
- lacks owner, purpose, scope, status, cleanup/archive condition, or reference;
- leaves old and new prompt/config/runtime logic active for the same behavior;
- creates a new manager/watcher/state owner per run when a task-level owner should be reused.

## One Canonical Project Artifact Ledger

Each project must maintain exactly one canonical artifact ledger. Reuse an existing project registry when it can own the full contract; never create a second ledger for a task, agent, drive, output class, or run. The project must declare the ledger's exact path and the output roots it governs in its project instructions or canonical configuration. Cross-project artifacts belong to one project's ledger; other projects reference that entry instead of duplicating ownership.

Resolve the ledger before creating or retaining any file or data artifact. This includes scratch files, generated validation output, caches, logs, reports, archives, downloaded or transformed data, databases, models, checkpoints, media, worktrees, and output stored outside the repository. An entry may cover a coherent bounded artifact set only when it records an exact directory or manifest and ownership is uniform; an open-ended glob or entire drive is not a valid entry.

Each entry must contain:

- `artifact_id`: stable project-unique identifier;
- `location`: exact path, URI, directory, or manifest;
- `owner`: live maintainer or producing component;
- `producer`: task, run, command, or workflow that created it;
- `purpose`: why the artifact exists and what consumes it;
- `classification`: exactly `temporary` or `long_term`;
- `state`: current lifecycle state such as active, cleanup-ready, blocked, removed, archived, or promoted;
- `created_at` and `updated_at`: auditable timestamps;
- `references`: source inputs, dependencies, stage anchor, run id, or parent artifact;
- `terminal_evidence`: initially empty, then the deletion, archival, promotion, or adoption evidence that closes the entry.

A `temporary` entry must additionally record all three cleanup dimensions:

- `cleanup_deadline`: a time or bounded latest time, including `task_end` when cleanup is immediate;
- `cleanup_event`: the lifecycle node that makes it eligible, such as validation completion, stage close, replacement acceptance, run failure, cancellation, or timeout;
- `cleanup_gate`: the evidence and safety threshold that must be true before deletion, such as no active reader, replacement hash accepted, or unique work preserved.

A `long_term` entry must record `retention_basis`, `live_consumer`, `review_condition`, and `retirement_condition`. `long_term` is not permanent by default; it means the project has deliberately adopted the artifact and knows why and when retention will be reconsidered.

## Closed-Loop Cleanup

Use one lifecycle loop:

`resolve ledger -> register at creation -> classify -> update state -> reconcile -> clean or retain -> record terminal evidence`

Reconcile the ledger with the project's declared output roots at task completion, stage transition, failed or cancelled runs, replacement acceptance, and before reporting cleanup or project completion. The reconciliation must establish:

1. every discovered artifact is represented by one live ledger entry or is deleted as owned task-local output;
2. every live ledger target exists and matches its recorded scope;
3. every temporary entry whose deadline and event have arrived is deleted once its gate passes;
4. every deletion, archive, promotion, or adoption is recorded with time and evidence;
5. no long-term entry lacks a live owner, consumer, review condition, or retirement condition.

An artifact found under a declared output root without a ledger entry is an orphan. Register and adopt it only when ownership, purpose, and retention are proven; otherwise delete it when authority and safety permit. A ledger entry whose target is missing is also a defect: determine whether the artifact was removed, moved, or lost, then close or repair the entry with evidence.

Task-owned temporary output may be removed without a second cleanup approval when its recorded gate passes and no broader destructive authority is required. If deletion is blocked by ownership, active use, permissions, or safety, set the entry to `blocked`, record the exact blocker, next owner, and next cleanup attempt or decision point, and report the incomplete cleanup. Never mark it removed or claim a clean terminal state.

For Git-backed work, task-created output that appears in `git status --porcelain=v1 --untracked-files=all` must be adopted in the scoped task commit or removed or relocated by its owner before completion, pause, or handoff. An ignored location is acceptable only when the project deliberately owns that output class and the ledger covers it; never add an ignore rule merely to conceal unexplained dirt.

If an artifact supersedes another, archive or clean up the superseded artifact promptly when safe and close its ledger entry. Prefer recoverable archive or trash over destructive deletion when either satisfies the terminal condition.
