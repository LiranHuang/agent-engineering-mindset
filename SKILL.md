---
name: agent-engineering-mindset
description: Keep engineering work anchored to the main goal, canonical owner, highest-ROI next action, one impact-scoped final gate, and proportionate version, artifact, worktree, runtime, and audit controls.
---

# Agent Engineering Mindset

Use for Git-backed implementation and other technical work where state,
ownership, generated artifacts, integration, or runtime loading can make a
plausible result misleading. Follow applicable `AGENTS.md` authority and risk
rules. This skill adds repository mechanics; it does not create a second goal,
permission, worktree, ledger, or acceptance system.

## Anchor the work

Identify the user's observable main goal, the smallest necessary current stage,
the canonical semantic owner, and the evidence that will prove the adopted
result. Choose the highest-ROI safe action that advances the main goal. Reject
substitute surfaces, speculative infrastructure, duplicated evidence, and
cleanup or refactors unrelated to the current outcome.

Repair or extend the canonical owner. Reuse, consolidate, or retire before
creating another implementation, entry point, fallback, configuration owner,
or tracking store. Preserve unrelated state.

Re-ground only after a material state transition or drift signal: integration,
handoff, conflicting evidence, changed authoritative input, baseline mismatch,
overlapping ownership, or uncertain runtime identity. Compaction, model change,
or a child result alone does not justify repeating broad discovery when the
authoritative state is unchanged.

## Use proportionate governance

For an ordinary reversible Git edit with bounded consequences:

1. Inspect the current ref, baseline, affected owner, and full status.
2. Confirm the affected paths and semantic surface have no overlapping writer,
   then resolve each exact touched file in the project's unique lifecycle ledger.
3. Record the admitted Git/content identity and intended action before writing;
   edit the canonical owner directly, stage only task-owned paths, inspect the
   exact candidate, and run one focused check.
4. Commit when durable adoption is requested or implied, update every touched
   file entry to the adopted revision/content identity or terminal cleanup state,
   then make one final acceptance decision on that revision and ledger state.

Ordinary work requires per-file lifecycle records but does not require an audit
child, project-wide cleanup sweep, process/lock scan, worktree inventory,
explicit adoption state machine, or checkpoint solely because a tracked file
changes. Git remains the content authority; the ledger stores identity and
lifecycle metadata only. Unrelated dirt may remain when it is disjoint, recorded
in the baseline, never staged or modified, and not part of acceptance.

Escalate before mutation when consequences are high-risk: production or loaded
runtime behavior; deployment, restart, publication, or external side effects;
security, credentials, authorization, or privacy; destructive or irreversible
state; financial, eligibility, data-integrity, selection, or other value
semantics; shared concurrency or transaction integrity; backward-incompatible
schema or public contracts; real migration, cutover, or uncertain-state
recovery; shared mutable or large managed non-Git artifacts; overlapping write
ownership; handoff or merge isolation; or failure impact that is not locally
bounded. Risk follows consequences, not file count, module count, a database
call, or the mere presence of schema or data files.

For high-risk work, apply only the relevant controls required by `AGENTS.md`:
one independent read-only audit, exact baseline and adoption identity,
transactional per-file lifecycle tracking, operation intents for destructive
actions, runtime loader evidence when runtime changes, and an isolated worktree
only when actual conflict or policy requires it. Read:

- [state and revision integrity](references/state-and-revision-integrity.md) for
  isolated worktrees, handoff, non-current targets, checkpoints, or merge
  boundaries;
- [artifact and resource lifecycle](references/artifact-and-resource-lifecycle.md)
  for exact file entries, destructive lifecycle operations, or large I/O;
- [verification and runtime](references/verification-and-runtime.md) for
  integration, runtime loading, data-integrity changes, or high-risk acceptance;
- [heartbeat supervision](references/heartbeat-supervision.md) for an eligible
  mechanical background stage that outlives a normal turn.

For migration, the audit first resolves the accepted source receipt or report
and proves it still binds the current source identity, manifest, ordered members,
recorded content hashes, and covered semantics. Persistence or path existence is
not acceptance. Reuse valid immutable source evidence and never revalidate an
unchanged payload merely because it will move or be copied. Audit the new
transfer, write-exclusion, recovery, cutover, consumer, and cleanup risks; widen
payload checks only when source evidence is stale or unbound, or the migration
changes a covered invariant. Use the migration section of
[verification and runtime](references/verification-and-runtime.md).

Every project file or directory touched by the task uses the same lifecycle
authority, including Git-tracked source/test/config/docs and non-Git outputs.
Only wholly process-internal objects that have no addressable project path and
disappear before the producing command returns are exempt. If no authority
exists, an authorized project change may perform one bounded genesis bootstrap
for the unique SQLite database, CLI/service, governed-root declaration, and
genesis evidence; any existing authority forbids bootstrap. Read
[artifact and resource lifecycle](references/artifact-and-resource-lifecycle.md)
for the strict per-file protocol and cleanup closure.

## Keep versions and worktrees bounded

`direct_target` is the default. Use the current branch and HEAD as the implicit
`must_integrate` contract unless the task declares another target. Inspect
registered worktrees, processes, or locks only when concurrency, a shared
checkout, a runtime mutator, a drift signal, or project policy makes them
relevant.

Use or reuse an isolated worktree only for conflicting dirt, a real overlapping
writer, handoff or merge isolation, material integration risk, an active
mutator, or repository policy. Never create retry, `verify2`, or `v2` siblings
as a substitute for reconciling the original instance. If a task worktree is
created, integrate or preserve unique work, then remove it at every terminal
outcome unless it is deliberately retained with a durable owner and removal
condition.

An uncommitted bounded diff is valid when the user forbids commits or the task
is explicitly review-only or experimental, but it is not an integrated
completion. A pause that must preserve mutable work may use an explicitly
unaccepted checkpoint. A local commit never authorizes push, deployment,
restart, publication, or another external action.

## Validate once, by impact

Run exactly one final acceptance gate on the adopted state. It may contain
several focused checks, but produces one evidence bundle and one decision.
Validate only invariants the delta can invalidate:

- changed payload, membership, ordering, decoding, selection, eligibility, or
  value-producing semantics: check affected values, hashes, coverage, and real
  consumers;
- changed identity, receipt, manifest, or reference with unchanged payload:
  check identity, recorded hashes, and reference closure without rescanning
  unchanged payload;
- value-free binding, locator, catalog, index, or metadata: check schema,
  bindings, resolution, and one real consumer without reopening immutable data;
- changed consumer or adapter: exercise the real lookup and boundary behavior;
- runtime change: prove the real loader uses the adopted revision or config.

Full-data, full-history, or whole-dependency validation is forbidden unless the
changed invariant spans that scope and no narrower authoritative proof is
sufficient. Record that reason and a bounded resource envelope. When high-risk
audit is required, its verdict belongs inside this same final bundle and must
flag both missing evidence and redundant validation.

At closure, confirm the requested outcome, exact adopted diff or revision,
focused evidence, preservation of unrelated state, every touched file's ledger
transition, every task-created output's adoption or cleanup, runtime state when
changed, and exceptional worktrees. Do not turn inherited maintenance debt into
unrelated task scope.
