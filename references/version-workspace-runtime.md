# Version, Workspace, and Runtime Integrity

## State model

For a Git-backed code, configuration, test, build, or repository-documentation change, use:

`clean admitted baseline -> bounded task-owned change -> exact staged candidate -> scoped local commit -> clean adopted worktree -> loaded runtime state`

During an active edit the worktree may be temporarily dirty. That dirt must not survive completion, a stage transition, a pause, or a handoff. The worktree that receives and adopts the change is clean only when `git status --porcelain=v1 --untracked-files=all` returns no records, including staged, unstaged, untracked, and submodule state.

## Workspace and write ownership

- Identify the repository root, adoption worktree, branch or detached revision, baseline, and relevant pre-existing changes before editing.
- Run the full porcelain status check before the first edit. Do not edit a dirty adoption worktree or mix with pre-existing state. Use a clean isolated branch or worktree when the existing dirt is outside the task.
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
