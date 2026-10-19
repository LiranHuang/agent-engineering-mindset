# Heartbeat Supervision for Mechanical Long-Running Tasks

Use this reference when a parent agent is responsible for a background task that runs longer than one normal turn and is divided into deterministic chunks, dates, partitions, or similarly mechanical stages.

## Eligibility

A heartbeat is appropriate only when all of the following are true:

- an official managed process, task, or other canonical execution owner already performs the business work;
- the main goal and current stage remain unchanged between checks;
- progress and terminal state are observable from stable evidence such as a run identity, checkpoint, manifest, chunk state, process tree, or output counters;
- the next action after ordinary progress is mechanical and already authorized; and
- the host provides a native heartbeat or recurring-monitoring lifecycle that can wake the same parent context.

Wait directly for short work. Use an exact scheduler or another host-native facility when exact wall-clock timing or an isolated execution context is required. Do not create a heartbeat to replace the business owner, keep an orphaned shell alive, add a parallel worker, bypass an approval boundary, or repeatedly reconsider a task that needs human judgment.

## Parent-owned lifecycle

Before creating the heartbeat, bind its instruction or task-local scratch to the smallest sufficient live state:

- main goal and current stage goal;
- canonical run, managed task, or process identity;
- current chunk, date range, checkpoint, and reusable partial progress;
- the cheapest authoritative progress and health signals;
- success, failure, stall, and blocker evidence;
- the already-authorized recovery or next-stage rule; and
- the decision boundary that must return to the user.

Use one heartbeat attached to the existing parent task or thread. Choose the lowest frequency that still permits useful intervention. A heartbeat turn should read only the bounded evidence needed to distinguish `healthy unchanged`, `material progress`, `terminal success`, `retryable failure`, `stall`, or `decision required`.

When the canonical run is healthy but unchanged, do not restart it, create another run, launch a child, repeat expensive validation, or send a progress message merely because the heartbeat fired. Record only the minimal host-required state and end the wake promptly.

Whenever a wake observes material progress that changes what the next wake must inspect, update the same heartbeat instruction or task-local scratch before ending the wake. Replace stale process, checkpoint, chunk, date-range, counter, or next-transition details; do not accumulate a history inside the heartbeat prompt. Keep durable history in the project's canonical artifacts when it is actually needed.

At terminal success, verify the task's terminal evidence and remove or disable the heartbeat in the same closure. At terminal failure, cancellation, supersession, or a decision blocker, either rebind that same heartbeat to an explicitly authorized recovery with a live owner or reclaim it and report the terminal state. Completion must not leave a recurring wake behind.

## Usage discipline

A heartbeat can reduce avoidable usage by preserving the parent context and replacing ad hoc manual polling, standalone tasks, duplicate agents, and repeated full-state reconstruction. It does not make model turns free.

Keep usage proportionate:

- prefer one thread-attached heartbeat over multiple overlapping monitors;
- use the longest safe interval and bounded read-only checks;
- inspect checkpoint deltas or compact counters instead of rereading full logs and artifacts;
- stay silent when nothing materially changed unless the host requires an acknowledgement;
- update the heartbeat binding only with current next-check state; and
- reclaim it immediately when monitoring no longer advances the main goal.

## Example: long-range chunk processing

For a deterministic materialization that processes a long range in chunks, the parent can bind one heartbeat to the canonical run, its current chunk, checkpoint, worker or process identity, and terminal manifest. On each wake:

1. Check the existing run and its actual worker tree plus the current chunk/checkpoint progress.
2. If the same run is healthy and progress evidence is advancing, do not restart it or create a new run. Update the heartbeat to the new current chunk or checkpoint when that changes the next check, then end the wake.
3. If there is no material change, perform no extra work and end promptly.
4. If the process exits, inspect the checkpoint, terminal manifest, and failure evidence. Continue from reusable progress only when the recovery rule is already authorized; never discard completed chunks merely to simplify recovery.
5. When the final chunk reaches its accepted terminal state, remove or disable the heartbeat immediately.

The example illustrates lifecycle and usage discipline only. Its staging or current-vintage output does not become canonical, PIT-valid, model-ready, or accepted merely because the heartbeat reports progress or completion.
