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

## Lifecycle Gate

Every parent-owned entity or artifact must either be clearly disposable or have lifecycle metadata.

Minimum lifecycle metadata:

- `owner`: who or what maintains it;
- `purpose`: what problem it solves;
- `scope`: task, stage, project, or runtime boundary;
- `status`: one of the lifecycle statuses below;
- `cleanup`: deletion, merge, archive, expiry, stop, or promotion condition;
- `reference`: stage anchor, task id, report, config, registry, or parent artifact that cites it.

### Lifecycle Status Rules

- `temporary`: delete when the current validation or work round ends. If it must be kept as evidence, move it into a project artifact location and change status to `stage_artifact` or `archived`.
- `scratch`: clean before final user-visible wrap-up unless the user explicitly asks to keep it or it is promoted with lifecycle metadata.
- `stage_artifact`: keep through the current stage. At stage close, archive it or have the next stage anchor explicitly adopt it as continuation base.
- `active`: keep only if it has a live owner and live reference/entrypoint. Active entities must not be orphaned.
- `superseded`: archive/trash promptly after the replacement is verified and no active reference depends on it. If retained, record why and when to revisit.
- `compatibility`: must have owner, purpose, and removal condition. Delete when the condition is met.
- `registry/state`: archive when the task completes, expires, is cancelled, or is adopted by a new canonical registry/state owner.
- `watcher/cron/daemon`: stop and clean when the task completes, times out, is cancelled, ownership changes, or the canonical service replaces it.
- `archived`: retain only as evidence/history. It must not remain active as a runtime, prompt, config, or dispatch path.
- `disposable`: no retention value. Delete before final wrap-up.

If lifecycle metadata would be excessive for a tiny local scratch artifact, keep it under task-local `tmp/` or project-owned scratch space and clean it before final wrap-up unless intentionally retained.

If an entity supersedes another, archive or clean up the superseded entity promptly when safe. Prefer recoverable archive/trash over destructive deletion.
