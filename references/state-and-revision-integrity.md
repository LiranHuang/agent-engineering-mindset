# State and revision integrity

Use this reference when a change crosses a context boundary, has a mutable baseline, is handed between workers, or must be integrated into a versioned canonical tree.

## Clean-worktree invariant

For a Git-backed code, configuration, test, build, or repository-documentation change, select one execution workspace before mutation and use its state model:

- `direct_target`: `clean admitted target baseline -> bounded task-owned change -> exact staged candidate -> scoped target commit -> one final gate -> clean target checkout`;
- `isolated_worktree`: `clean admitted baseline -> bounded task-owned change -> exact staged candidate -> scoped candidate commit -> integrated target -> one final gate -> clean closed worktree lifecycle`.

During an active edit the worktree may be temporarily dirty. That dirt must not survive completion, a stage transition, a pause, or a handoff. The worktree that receives and adopts the change is clean only when `git status --porcelain=v1 --untracked-files=all` returns no records, including staged, unstaged, untracked, and submodule state.

## Choose the execution workspace

Inspect the canonical target checkout before creating any task worktree: exact ref and baseline, full porcelain status, registered worktrees, assigned write owners, and active processes or locks capable of mutating that checkout or an overlapping semantic surface.

Use `direct_target` by default. When the canonical target checkout is clean, attached to the intended target branch at the admitted baseline, has no other active writer, mutating process, or lock on the checkout or overlapping semantic surface, and project policy does not require isolation, do not create a task worktree. Make the bounded change, stage exact paths, commit, validate, and close in that checkout. An inactive registered worktree, idle chat, or clean unrelated checkout does not by itself occupy the target.

Use or reuse an `isolated_worktree` only when direct target mutation is blocked by unrelated dirt, a real concurrent or overlapping writer, an active mutating process or lock, explicit handoff or merge isolation, material merge risk, or repository policy. Record that concrete reason before creation; convenience, habit, or a generic retry is not sufficient.

## Adoption contract

Before mutating the selected checkout, bind both its execution workspace and one adoption mode:

- `must_integrate`: the requested repository change must reach an exact canonical `target_ref`;
- `research_only`: the branch or artifact is the intended bounded deliverable and must not alter the canonical target; or
- `checkpoint_only`: the state is preserved for continuation but is explicitly unverified and unaccepted.

Record or otherwise make inspectable the exact `target_ref` when applicable, `baseline_sha`, stable task or owner identity, integration owner, candidate reference, terminal action, and next-use or removal condition. A title, the word `current`, or an idle/not-loaded session state is not a durable owner identity.

For `must_integrate`, use the workspace-specific terminal state machine:

- `direct_target`: `created -> active -> committed_on_target -> final_gate_passed -> target_clean`;
- `isolated_worktree`: `created -> active -> candidate_committed -> integrated_target -> final_gate_passed -> worktree_removed_or_deliberately_adopted`.

Do not skip from either committed state to task or Goal completion. In `direct_target`, the committed target still requires the single final gate and clean closure. In `isolated_worktree`, if integration is outside current authority, preserve the clean candidate and report `integration_pending` or the applicable blocked state to the owning parent or user. Do not silently reinterpret it as `research_only`.

Before the first edit:

- identify the repository root, branch or detached revision, selected execution workspace, and exact baseline;
- run the full porcelain status check and classify every existing record by owner;
- inspect registered worktrees, assigned writers, and active mutating processes or locks before deciding that the canonical target is occupied;
- do not edit a dirty adoption checkout or mix with pre-existing state; use a clean isolated branch or worktree when existing dirt is outside the task;
- if cleaning or recovering the dirty state is itself the authorized task, inventory it first and change only proven task-owned paths.

For candidate or target closure:

- keep generated output outside the source worktree or in an intentional ignored location unless it is an adopted deliverable;
- stage explicit task-owned paths and inspect the staged diff; do not use broad staging that could absorb unrelated state;
- run whatever bounded implementation checks are needed to produce a credible candidate, then create a scoped local commit;
- verify the commit contains only the candidate and require empty full porcelain status; and
- in `direct_target`, treat the resulting scoped commit as the integrated target revision and run the single final gate there; and
- in an isolated `must_integrate` workspace, do not call development checks the final acceptance gate.

For an isolated `must_integrate` workspace, the designated integration owner integrates the exact candidate into `target_ref`, inspects the resulting canonical delta, and performs the one final acceptance gate there. Confirm both target and task worktrees are clean as part of the same terminal decision. Candidate checks are preflight evidence, not a second acceptance layer. For `direct_target`, confirm the committed target checkout is clean and validate that exact revision; there is no separate integration step or temporary worktree to close.

If work must pause before acceptance, create an explicitly unverified task-only checkpoint commit so the worktree is clean. It may be continued on the same owned line but must not be merged, deployed, or treated as accepted. A local commit never authorizes push, publication, deployment, restart, or an otherwise unauthorized merge. When the user's requested repository change and project workflow already authorize local integration, the named integration owner must perform it rather than treating the candidate commit as the terminal deliverable. If commits are explicitly prohibited, stop before mutation and surface the conflict instead of deliberately leaving a persistent dirty change.

## Isolated worktree lifecycle

A task-created isolated worktree is exceptional temporary execution infrastructure, not the default workspace, unless the project deliberately adopts it with a live owner, reason, reference, and removal condition. A clean status does not by itself close that lifecycle.

Before creating one:

- record the exact evidence that made `direct_target` unavailable or made isolation mandatory;
- enumerate current registrations with `git worktree list --porcelain` and inspect the intended branch, revision, and exact path;
- reuse a compatible existing task worktree; do not create renamed retry, `verify2`, or `v2` siblings merely because setup, checkout, or validation failed;
- define its adoption mode, target ref when applicable, baseline revision, stable owner, integration owner, purpose, and terminal removal or adoption condition;
- for `must_integrate`, start from the declared target baseline; do not stack on an unintegrated task branch unless the dependency, integration order, and common integration owner are explicit; and
- if `git worktree add` partially fails, reconcile the intended path, registration, branch or detached revision, and files before retrying. Resume the same instance or roll back only proven task-created partial state after preserving any unique work.

After a `must_integrate` task change is integrated into the intended canonical target and that exact integrated state passes the single final validation, close the worktree lifecycle within the same acceptance gate:

- resolve the exact registered path from `git worktree list --porcelain`; never infer a deletion target from a name pattern;
- confirm it is not the canonical adoption worktree, has empty full porcelain status, contains no unresolved task evidence or unique unintegrated work, and has no active writer, process, or lock;
- remove the registered worktree with `git worktree remove <exact-path>` without `--force`; do not delete a registered worktree as an ordinary filesystem directory;
- use `git worktree prune` only for verified stale registrations whose directories are already absent, then confirm both the registration and exact path are gone; and
- treat branch deletion as a separate lifecycle decision governed by explicit authorization or repository policy.

At any other terminal outcome, including failure, cancellation, supersession, or an indefinite blocker, preserve unique task work on a named branch or explicit checkpoint when needed, then remove the worktree or deliberately adopt it with a stable owner identity and concrete next-use or removal condition. A bounded pause or handoff may retain the worktree only when those fields are explicit and durable.

Squash, rebase, or cherry-pick integration may not make the worktree HEAD an ancestor of the target branch, so require evidence tying its task-owned change to the accepted integrated revision rather than relying on ancestry alone. If safe removal cannot be proven, do not force it: keep or adopt the worktree with an explicit owner and removal condition and report the blocker. Completion must not leave an ownerless task-created worktree.

Before a task or Goal is marked complete, make one terminal decision over the adoption contract: the exact target revision contains the required change, the final target state passed the single gate, all selected checkouts are clean, lifecycle records agree, and any task-created worktree is removed or deliberately adopted. This terminal decision bundles acceptance and lifecycle closure; it is not another validation pass.

## Re-ground from authoritative state

Before continuing after compaction, resume, handoff, model change, child result, integration, or a material new requirement, inspect the current state that governs the next action. Reconfirm the objective, target surface, canonical semantic owner, authoritative inputs, baseline, current candidate or task-owned diff, and acceptance evidence.

Use the repository's or project's existing records where they are authoritative. A conversation summary, progress note, branch name, or worktree path can help locate evidence but does not replace it. Do not create a separate registry, tracker, or control plane for ordinary work.

If the re-grounded evidence conflicts with the assumed state, stop the affected mutation. Resolve the discrepancy through the canonical project mechanism or report the missing decision; do not silently reinterpret the goal, reuse stale acceptance, or turn a blocker into unrelated work.

## Keep revision evidence precise

When version control is relevant, record or otherwise make inspectable:

- the admitted baseline revision and pre-existing changes in the affected surface;
- the exact candidate revision or bounded task-owned diff; and
- which required checks evaluated that candidate.

Candidate evidence is stale when the candidate, baseline, authoritative inputs, or affected semantic owner changed after the evidence was collected. Re-run the relevant validation rather than applying an earlier PASS to a new candidate.

An isolated workspace can protect a change while it is developed, but it is not proof of integration. Before accepting an integrated change, inspect the resulting canonical diff and validate relevant behavior against the integrated tree. Treat clean textual merges as insufficient when both sides changed the same semantic owner.

## Preserve ownership boundaries

Do not overwrite, reset, stage, absorb, or clean changes whose ownership is unclear or outside the approved change. Keep generated output out of source surfaces unless the project explicitly owns it. If mutable work has an assigned writer, do not create a competing write line to accelerate, replace, or repair it; use the host and applicable project rules to resolve ownership.
