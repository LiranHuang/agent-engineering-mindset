# Communication and Single Validation

## Communication minimality

Lead with the result and include only decision-relevant evidence. Do not expand a narrow answer merely to satisfy a report template, and do not dump hidden reasoning, command logs, registries, or full anchors unless the user requests them or they materially change the decision.

For a small completed change, report:

- the changed canonical surface;
- the result of the single final validation;
- any remaining risk or main-goal gap.

Use a fuller stage report only for a genuine stage close, handoff, approval boundary, or project-round summary.

## Single validation gate

Every accepted change gets one final validation gate bound to the exact state being accepted. One gate may contain several checks, but it produces one evidence bundle and one PASS or FAIL decision.

Select checks proportionate to the change:

- exercise the real positive entry path;
- run the smallest targeted tests that cover changed behavior;
- check that a retired or duplicate path is absent when relevant;
- verify schema, artifact, configuration, or revision identity when relevant;
- verify loaded state and health when runtime changed.

Avoid ceremonial checks that do not change confidence. Do not require separate implementation, independent-verifier, merge, and runtime PASS layers. When independence is genuinely required, the independent verifier performs this one gate.

The validation result must name or otherwise unambiguously identify the accepted final state. If that state changes, discard the stale result and run one replacement gate on the new final state.

## Outcome handling

- **PASS:** the exact final state satisfies the main-goal-relevant acceptance criteria. Continue or close according to the remaining main-goal gap.
- **FAIL:** return to the same owned implementation path, repair the defect, and rerun the one final gate.
- **BLOCKED:** preserve state and report the missing authority, input, owner, or capability. Do not claim partial local success as main-goal completion.

Before claiming completion, confirm that the stage advanced the main goal, the chosen action was ROI-aligned, the final accepted state passed the single gate, unrelated state was preserved, and retained artifacts or runtime owners have a clear lifecycle.
