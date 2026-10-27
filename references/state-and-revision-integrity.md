# State and revision integrity

Use this reference for isolated worktrees, handoff, non-current targets,
checkpoint-only work, merge boundaries, or material baseline drift. Ordinary
direct-target edits use the shorter path in `SKILL.md`.

## Direct target

The implicit ordinary contract is `direct_target + must_integrate + current
branch + current HEAD baseline + parent integration owner`. Inspect the current
ref, baseline, affected owner, and full porcelain status. A checkout may contain
disjoint unrelated dirt when the task's paths and semantic surface are clean,
there is no overlapping writer, only exact task paths are staged, and unrelated
state remains unchanged. Never reset, stash, clean, broadly stage, or absorb
unrelated work.

Require a fully clean checkout when project policy requires it or when release,
migration, runtime, recovery, or another high-risk task makes whole-tree state
part of acceptance. Inspect process/lock state and all worktree registrations
only when a shared checkout, concurrent writer, active runtime mutator, handoff,
drift signal, or repository rule makes them relevant.

For durable adoption, create a scoped commit unless the user forbids commits.
An explicitly review-only or experimental task may end with a bounded
uncommitted diff, but it is not integrated completion. A pause that must retain
mutable work may use an explicitly unaccepted checkpoint commit.

## Exceptional adoption contract

Record `workspace_mode`, `adoption_mode`, target ref, baseline, stable owner,
integration owner, candidate reference, and terminal action only for an
exception: isolated worktree, non-current target, `research_only`,
`checkpoint_only`, handoff, retained workspace, or high-risk integration.

For `must_integrate`, an isolated candidate is implementation evidence, not
completion. Integrate the exact candidate into the declared target, inspect the
resulting canonical diff, and run the one final gate there. If integration is
outside authority, preserve the candidate and report `integration_pending`.
Do not reinterpret it as research merely to close the task.

## Isolated worktree lifecycle

Use or reuse an isolated worktree only for conflicting dirt, a real overlapping
writer, handoff or merge isolation, material merge risk, an active mutator, or
repository policy. Before creation, enumerate registered worktrees, declare the
baseline and owner, and reuse a compatible instance. Do not create retry,
`verify2`, or `v2` siblings after a setup or validation failure; reconcile the
original path, registration, ref, and files.

At completion, failure, cancellation, supersession, or indefinite block:

1. Preserve unique work on a named ref or explicit checkpoint when needed.
2. Resolve the exact registered worktree path; never infer it from a name glob.
3. Confirm it is not the canonical target, contains no unintegrated work, and
   has no active writer.
4. Remove it with `git worktree remove <exact-path>` without `--force`, then
   verify its registration and directory are gone.

If safe removal cannot be proven, deliberately retain it with a durable owner,
reason, reference, and concrete next-use or removal condition. Branch deletion
is a separate decision. Empty porcelain alone is not lifecycle closure.

## Re-ground only on material change

Reconfirm objective, owner, baseline, candidate, and evidence after integration,
handoff, a material new requirement, conflicting evidence, authoritative-input
change, baseline mismatch, overlapping ownership, or uncertain runtime identity.
Do not repeat broad discovery merely because of compaction, a model change, or a
child result when authoritative state did not change.

Evidence is stale only when its candidate, baseline, authoritative input,
semantic owner, validation implementation, or covered invariant changed. Treat
a clean textual merge as insufficient when both sides changed the same semantic
owner. Preserve unrelated ownership at all times.
