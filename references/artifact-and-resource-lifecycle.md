# Artifact and resource lifecycle

Use this reference for the strict lifecycle of every project file and directory,
including Git-tracked files, non-Git outputs, destructive or externally visible
operations, ledger migration, and large-resource work. Git owns tracked content
and revision history; the ledger owns file identity and lifecycle metadata.
Neither may substitute for the other.

## Scope the ledger correctly

Every file or directory the task will create, modify, rename, move, adopt,
retain, or delete is managed, including source, tests, configuration, schemas,
scripts, prompts, documentation, generated files, build/test outputs, logs,
caches, ignored files, datasets, databases, models, checkpoints, shared runtime
state, and task worktrees. The only exemption is an object that remains wholly
inside a process, has no project- or externally addressable path, and disappears
before the producing command returns. If uncertain, register it or stop before
further mutation.

A project has exactly one canonical writable ledger for
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

Git is the sole authority for tracked contents, index, blobs, commits, merges,
and adopted revisions. The ledger stores only stable file id, exact path and
history, owner, classification, references, lifecycle events, evidence, and
adopted revision or content identity metadata. It never stores or reconstructs
source contents. If no authority exists, one explicitly authorized genesis
bootstrap may atomically create the unique database, CLI/service, governed-root
declaration, and immutable genesis evidence. Close that exception after the
first successful transaction; an existing declared or writable authority
forbids another bootstrap.

For an externally visible, irreversible, or managed-artifact deletion, move,
publication, or other lifecycle action, record a bounded operation intent and
idempotency token, perform the separately authorized action, verify the
postcondition, then record terminal evidence. Routine removal of task-local
disposable scratch needs no operation record. Intent alone is not success.
During ledger migration, keep one writable authority, prove import and all real
reader/writer cutovers, then retire the old path; never dual-write.

## Register and close task scope

Before first mutation, read or create one entry for every exact touched file and
record its stable id, location, owner, producer, purpose, admitted Git/content
identity, intended action, timestamps, references, lifecycle state, and exactly
one classification. A closed manifest may batch the transaction, but each member
keeps an individual stable id, revision, path history, and terminal state. Never
use an open glob, whole repository, whole worktree, or whole drive as a file
entry.

- `temporary`: exactly one `deadline`, `event`, or `gate` cleanup trigger;
- `long_term`: retention basis, live consumer, review condition, and retirement
  condition.

After the authorized Git or filesystem action, verify the exact path,
Git/content identity, or absence postcondition and record the terminal adopted,
moved, deleted, or blocked transition. At task closure, reconcile every touched
entry with the exact adopted Git state and every task-created output root with
its bounded inventory. A new file must be committed or deliberately adopted,
cleaned under its temporary trigger, or closed with the exact blocker, next
owner, and next cleanup point. Remove only eligible owned inactive artifacts
within authority and record terminal evidence.

Do not turn inherited overdue entries or inventory gaps into unrelated task
scope. Keep them as maintenance debt unless they block the current task or the
user requests cleanup. If cleanup is blocked, record the blocker, next owner,
and next cleanup point. Never erase unrelated work or unresolved evidence.

A future deadline is real-time only when a live project-owned scheduler or
cleanup runner is bound with observable execution evidence. Policy text does
not wake itself.

For Git-backed work, every task-created tracked, untracked, generated, or ignored
file must be adopted in the exact scoped commit or ledger state, or removed by
its owner. Direct persistent output into a pre-registered exact path or closed
manifest. Do not add an ignore rule merely to hide unexplained dirt, and do not
use broad clean, reset, or stash as lifecycle closure.

## Bound large-resource work

Only when scale creates risk, define input/output bounds, streaming or chunking,
concurrency, spill location, reusable partial progress, and a stop threshold.
Prefer the smallest I/O that proves the requirement. Small local edits do not
need a formal resource envelope.

Keep generic policy project-neutral. Concrete repositories, drives, business
layers, storage roots, schedulers, and cleanup destinations belong in project
instructions or canonical configuration, not in this reference.
