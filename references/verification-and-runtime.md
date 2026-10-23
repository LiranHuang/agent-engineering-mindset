# Verification and runtime integrity

Use this reference for integration, runtime-affecting work, or acceptance where a stale artifact, configuration, checkout, or process could invalidate a local result.

## Exact accepted-state evidence

Acceptance evidence must name or unambiguously identify the exact revision or bounded diff actually being adopted. It must also cover the admitted baseline when that baseline determines behavior. A previous result is not reusable after a relevant change to the candidate, target, baseline, inputs, or semantic owner.

For meaningful changes, exercise the smallest authoritative check that reaches the real entry path, not merely a helper or substitute path. Inspect the final delta for unauthorized scope, omitted required changes, and duplicate active implementations. Pair required positive behavior evidence with a negative or count check whenever stale or duplicate ownership is plausible.

## Integration determines the acceptance surface

For `adoption_mode=must_integrate`, the single final acceptance gate always evaluates the exact committed canonical target revision. With `workspace_mode=direct_target`, the scoped commit already is that target revision. With `workspace_mode=isolated_worktree`, checks on the isolated branch or workspace are implementation preflight; integrate the exact candidate into the declared canonical target before the gate. Do not require a branch PASS and a target PASS as two cumulative acceptance gates. Resolve semantic conflicts explicitly even if version-control tooling reports no textual conflict, and retain patch-equivalence evidence when squash, rebase, or cherry-pick breaks ancestry.

For a parent-executed persistent change, the independent audit child reviews the exact final adopted revision or bounded diff and returns `PASS`, `FAIL`, or `BLOCKED`. Include that verdict and its concrete findings in this same final evidence bundle. It is not a preceding or following gate. `BLOCKED` prevents acceptance; `FAIL` returns work to the same canonical implementation path. If the accepted state changes after review, discard the stale verdict and obtain one replacement audit of the new final state before rerunning the single gate and making the parent acceptance decision.

For `research_only` or `checkpoint_only`, do not merge merely to satisfy a generic closure pattern. Validate only the deliverable that the adoption contract actually defines, and keep an unverified checkpoint explicitly unaccepted.

## Prove the running system is loading the verified result

When a change affects runtime behavior, identify the real loader or entry path and verify it uses the intended integrated revision, configuration, or artifact. Then check the relevant health and behavior. Look for loaders, configuration precedence, generated copies, long-lived processes, and other paths that could retain stale behavior.

Do not claim runtime success from source-tree state alone. If the loaded revision or configuration cannot be determined, report that limit and do not promote a local result into runtime acceptance.
