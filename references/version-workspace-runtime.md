# Version, Workspace, Merge, and Runtime Integrity

## Version And Workspace Integrity

For code, config, prompt, runtime, or workflow changes, use one state model:

`isolated task worktree -> verified commit -> merged canonical branch -> clean canonical runtime checkout`

Trust advances only after verification and merge gates pass. Run services from the clean canonical source checkout at an explicitly selected merged commit.

### Isolated Task Worktree To Verified Commit

Before editing, inspect the base commit and pre-existing changes in the intended paths.

- Give every agent or independently owned task its own writable branch and worktree. Never let multiple agents share one mutable checkout; if isolation is unavailable, keep the conflicting path read-only or hold the task.
- Start each task branch from the current canonical commit and record that base revision.
- Do not overwrite, stash, reset, stage, or absorb changes that are not owned by the current task.
- If ownership of an existing change is unclear, hold the conflicting path until ownership is resolved.
- Write test outputs, generated state, caches, screenshots, and build artifacts only to a temporary directory or a project-owned artifact directory with a cleanup rule. Clean disposable outputs before handoff or completion, and never let tests mutate source or runtime configuration as an untracked side effect.
- Stage only explicit task-owned paths; do not use broad staging that can absorb unrelated work.
- Inspect the staged diff and run the smallest meaningful tests that exercise the changed behavior through its real entry path.
- A checkpoint commit may preserve incomplete work for continuation or handoff, but it is not verified and must not be merged or run.
- When a semantic unit is complete, preserve it in a scoped commit with its verification evidence. A local commit does not authorize push, merge, restart, or other external publication.

### Verified Commit To Canonical Merge

Merge is allowed only after all of these checks pass:

- Refresh the canonical branch and confirm the task's recorded base has not changed unexpectedly.
- Compare the task diff against all canonical changes since the recorded base.
- Confirm there are no textual conflicts, overlapping semantic owners, incompatible config/schema changes, missing tracked dependencies, or untracked files required by runtime code.
- If Git reports a clean merge but the same behavior or owner changed on both branches, treat it as a semantic conflict and reconcile it explicitly.
- Merge the verified task commit into the canonical branch using normal Git merge semantics. Do not copy files between shared checkouts as a substitute for merge.
- Inspect the resulting merge diff and rerun the relevant tests against the merged tree. A task-branch PASS does not prove the merged tree.
- If the baseline changes during review or merge, stop, update the task branch against the new canonical base, resolve ownership, and rerun verification.
- Keep the task branch/worktree until the merged canonical tree passes verification; then remove or archive it according to repository policy.

### Canonical Merge To Runtime

- Use the canonical source checkout as the runtime source.
- Restart services only from the canonical checkout after it is updated to the explicitly verified merged commit.
- Require the canonical runtime checkout to have no staged, unstaged, or untracked runtime dependencies before restart.
- Record the exact runtime commit and verify service working directory, loaded revision, health, and relevant behavior after restart.
- Never restart services from an agent task worktree, an unmerged branch, a shared dirty checkout, or uncommitted files.
- If the canonical checkout is dirty, hold restart, identify ownership, and either commit and merge the complete verified semantic unit or remove the unrelated runtime dependency through its owner.
- Roll back by selecting and checking out a previously verified canonical commit, then restart and verify.

Branches and worktrees isolate development; Git commits and merges define version history; the clean canonical source checkout defines runtime.
