# Artifact and resource lifecycle

Use this reference for managed non-Git artifacts, destructive or externally
visible artifact operations, lifecycle-ledger migration, or large-resource work.
Git is the lifecycle owner for ordinary tracked source, tests, configuration,
prompts, and documentation; do not ledger them merely because they change.

## Scope the ledger correctly

A non-Git object is managed when it survives the producing task or process, is
shared or externally stored or published, is costly and intended for reuse, can
be discovered by a runtime or consumer, or needs a deliberate retention or
cleanup decision. Typical examples are retained outputs, datasets, databases,
models, checkpoints, shared caches or logs, shared mutable runtime state, and
exceptional worktrees needing durable tracking.

Process-local or task-local scratch, ordinary build/test temporaries, and
ignored disposable files are not managed when they are unshared, have no
retention or reuse value, cannot be discovered by runtime, and are automatically
removed before task closure. If any condition is uncertain, treat the object as
managed. Do not bootstrap a ledger for a source-only Git edit.

A project with managed artifacts has exactly one canonical writable ledger for
its declared governed roots. Use one SQLite transactional database through one
project-owned local service or CLI. Authorized agents may concurrently call the
interface for their assigned stable ids, but no agent opens the database
directly, writes raw SQL, keeps a private copy, or adds an external lock file.
Cross-environment callers invoke the same host-owned interface.

Each mutation is a short transaction: validate authority, compare expected
global and entry revisions, apply only the owned delta, append immutable actor
and evidence history, increment revisions, and commit. Conflicts fail closed for
re-read and bounded retry or owner reconciliation. Last-write-wins, blind
upsert, and writable JSON exports are forbidden. Ledger access never grants
authority to mutate the artifact it describes.

For an externally visible, irreversible, or managed-artifact deletion, move,
publication, or other lifecycle action, record a bounded operation intent and
idempotency token, perform the separately authorized action, verify the
postcondition, then record terminal evidence. Routine removal of task-local
disposable scratch needs no operation record. Intent alone is not success.
During ledger migration, keep one writable authority, prove import and all real
reader/writer cutovers, then retire the old path; never dual-write.

## Register and close task scope

Register a managed artifact when created or adopted. Bound a set by an exact
directory or manifest, never an open glob or whole drive. Record stable id,
location, owner, producer, purpose, timestamps, references, lifecycle state,
terminal evidence, and exactly one classification:

- `temporary`: exactly one `deadline`, `event`, or `gate` cleanup trigger;
- `long_term`: retention basis, live consumer, review condition, and retirement
  condition.

Cleanup is active for task-created or task-adopted managed artifacts and for
artifacts that block the task's ownership, safety, acceptance, or continuation.
Resolve and update the ledger only when the task performs a lifecycle action on
such an artifact, after a relevant trigger, and at the task's terminal boundary.
Merely entering a project or editing a Git-tracked file inside a governed root
does not trigger a ledger read. Remove an eligible owned inactive artifact
within authority and record terminal evidence.

Do not turn inherited overdue entries or inventory gaps into unrelated task
scope. Keep them as maintenance debt unless they block the current task or the
user requests cleanup. If cleanup is blocked, record the blocker, next owner,
and next cleanup point. Never erase unrelated work or unresolved evidence.

A future deadline is real-time only when a live project-owned scheduler or
cleanup runner is bound with observable execution evidence. Policy text does
not wake itself.

For Git-backed work, any task-created file visible in porcelain status must be
adopted in the exact scoped commit or removed/relocated by its owner. Do not add
an ignore rule merely to hide unexplained dirt.

## Bound large-resource work

Only when scale creates risk, define input/output bounds, streaming or chunking,
concurrency, spill location, reusable partial progress, and a stop threshold.
Prefer the smallest I/O that proves the requirement. Small local edits do not
need a formal resource envelope.

Keep generic policy project-neutral. Concrete repositories, drives, business
layers, storage roots, schedulers, and cleanup destinations belong in project
instructions or canonical configuration, not in this reference.
