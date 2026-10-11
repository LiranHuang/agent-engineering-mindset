# Version, Workspace, and Runtime Integrity

## State model

For a Git-backed code, configuration, test, build, or repository-documentation change, use:

`clean admitted baseline -> bounded task-owned change -> exact staged candidate -> scoped local commit -> clean adopted worktree -> loaded runtime state`

During an active edit the worktree may be temporarily dirty. That dirt must not survive completion, a stage transition, a pause, or a handoff. The worktree that receives and adopts the change is clean only when `git status --porcelain=v1 --untracked-files=all` returns no records, including staged, unstaged, untracked, and submodule state.

## Workspace and write ownership

- Identify every intended writable surface before editing. Classify each as a versioned canonical owner, an unversioned live mirror, generated output, or runtime state, and identify the repository root, adoption worktree, branch or detached revision, baseline, and relevant pre-existing changes for every enclosing repository.
- Run the full porcelain status check for every enclosing repository before the first mutation anywhere in the task. A target outside Git is not an exception: when it mirrors a Git-backed owner, update and accept the tracked owner first, then synchronize the live mirror and verify semantic parity. If the canonical repository is dirty or ownership is unresolved, leave the live mirror unchanged.
- Do not edit a dirty adoption worktree or mix with pre-existing state. Use a clean isolated branch or worktree when the existing dirt is outside the task.
- If cleaning or recovering existing dirt is itself the authorized task, inventory it first and change only paths with proven ownership.
- Keep one writer for each overlapping semantic surface. Concurrent writers are allowed only on clearly disjoint surfaces with explicit ownership.
- Use an isolated branch or worktree when concurrency, merge risk, handoff, or repository policy justifies it. Do not require one for a small uncontested edit.
- Never overwrite, stash, reset, stage, absorb, or clean unrelated state.
- Keep generated output outside the source worktree or in an intentional project-owned ignored location unless it is an adopted deliverable. Do not add an ignore rule merely to hide unexplained dirt.
- Stage explicit task-owned paths and inspect the staged diff. Do not use broad staging that could absorb unrelated state.
- A commit preserves a version; it does not by itself prove correctness or authorize push, merge, restart, deployment, or publication.

If the baseline changes, required untracked dependencies appear, or semantic ownership overlaps, stop affected mutation and reconcile before acceptance.

## Clean closure

Perform the one final validation gate on the exact staged candidate. When it passes, create a scoped local commit without changing the validated tree, confirm the commit contains only the accepted candidate, and run the porcelain status check once as part of the same acceptance gate. Do not start another semantic task or report completion until the status is empty.

If work must pause before acceptance, create an explicitly unverified task-only checkpoint commit so the worktree is clean. It may be continued on the same owned line but must not be merged, deployed, run, or treated as accepted. If commits are explicitly prohibited, stop before mutation and surface the conflict instead of deliberately leaving a persistent dirty change.

## Isolated worktree lifecycle

A task-created isolated worktree is temporary execution infrastructure unless the project deliberately adopts it with a live owner, reason, reference, and removal condition. Empty porcelain is necessary but does not by itself close the worktree lifecycle.

Before creating one:

- enumerate current registrations with `git worktree list --porcelain` and inspect the intended branch, revision, and exact path;
- reuse a compatible existing task worktree; do not create renamed retry, `verify2`, or `v2` siblings merely because setup, checkout, or validation failed;
- define its live owner, purpose, starting revision, and terminal removal or adoption condition; and
- if `git worktree add` partially fails, reconcile the intended path, registration, branch or detached revision, and files before retrying. Resume the same instance or roll back only proven task-created partial state after preserving any unique work.

After the task change is integrated into the intended canonical target and that exact integrated state passes the single final validation, close the worktree lifecycle within the same acceptance gate:

- resolve the exact registered path from `git worktree list --porcelain`; never infer a deletion target from a name pattern;
- confirm it is not the canonical adoption worktree, has empty full porcelain status, contains no unresolved task evidence or unique unintegrated work, and has no active writer, process, or lock;
- remove the registered worktree with `git worktree remove <exact-path>` without `--force`; do not delete a registered worktree as an ordinary filesystem directory;
- use `git worktree prune` only for verified stale registrations whose directories are already absent, then confirm both the registration and exact path are gone; and
- treat branch deletion as a separate lifecycle decision governed by explicit authorization or repository policy.

At any other terminal outcome, including failure, cancellation, supersession, or an indefinite blocker, preserve unique task work on a named branch or explicit checkpoint when needed, then remove the worktree or deliberately adopt it with a live owner and concrete removal condition. A bounded pause or handoff may retain the worktree only when the continuing owner and next-use or removal condition are explicit.

Squash, rebase, or cherry-pick integration may not make the worktree HEAD an ancestor of the target branch, so require evidence tying its task-owned change to the accepted integrated revision rather than relying on ancestry alone. If safe removal cannot be proven, do not force it: keep or adopt the worktree with an explicit owner and removal condition and report the blocker. Completion must not leave an ownerless task-created worktree.

## One final validation

Use one acceptance gate after the change reaches the exact final state intended for adoption.

The single validation pass may include multiple proportionate checks, such as targeted tests, the real positive entry path, an absence or negative check for retired behavior, configuration loading, and runtime health. These checks form one evidence bundle and one PASS or FAIL decision.

Do not stack branch validation, independent verifier validation, merged-tree validation, and runtime validation as separate mandatory gates. Choose the actual acceptance target:

- for a local change, the exact staged candidate and the scoped local commit with the same tree;
- for a versioned integration, the exact final integrated revision;
- for a generated artifact, the exact adopted artifact;
- for a runtime change, the exact loaded configuration or revision and its relevant behavior.

An independent verifier may execute the single gate when the risk or project policy requires independence. It replaces the ordinary acceptance pass; it does not add another layer.

If integration or later edits change the target after validation, the earlier result is stale. Perform one replacement validation on the new final state. A failed pass returns to the same owned implementation path; after repair, run the single final validation again.

## Integration and runtime

Integrate using normal repository semantics and inspect ownership or conflicts before acceptance. A clean textual merge does not settle a semantic conflict.

For runtime-affecting work:

- identify the real loader, working directory, configuration source, artifact, and revision;
- load the final intended state rather than an agent scratch path or accidental dirty dependency;
- include loaded-state identity, health, and relevant behavior in the single final validation pass;
- keep a known recoverable prior state when rollback is proportionate to the risk.

Completion requires agreement between the accepted final state and the runtime state when runtime is in scope.
