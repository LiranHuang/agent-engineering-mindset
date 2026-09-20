# Approval and Resource Boundaries

## Approval Boundary Gate

Before executing any protected, irreversible, destructive, externally visible, privacy-sensitive, or authority-changing action, create an active decision record containing:

- the exact action and target;
- why approval is required;
- the authorized approver or role;
- the allowed choices and their meaning;
- an expiry, revision, or nonce when stale responses are possible;
- the evidence required to prove authorization;
- the safe state while approval is pending.

Use an interactive approval mechanism when the current channel supports one. Otherwise request an explicit, unambiguous response that names the action or selected choice. Never treat request delivery, a displayed prompt, a reaction from an unknown actor, silence, or an unrelated affirmative message as authorization.

Before executing the protected action:

1. Confirm the response came from an authorized actor.
2. Match it to the active decision record, target, choice, and current revision or nonce.
3. Confirm it has not expired, been superseded, or already been consumed.
4. Record the authorization evidence without exposing secrets.
5. Execute only the approved scope; request new approval if the target or effect changes.

If the current interface cannot provide adequate identity or response evidence, keep the action pending and report the missing capability. Do not downgrade silently to a weaker approval path.

## Resource And IO Envelope Gate

Before dispatching, accepting, or continuing any task that may touch large datasets, model artifacts, logs, media, browser traces, mounted drives, or other high-throughput resources, require a resource envelope.

A resource envelope records:

- expected input size and output size;
- max RSS soft ceiling and hard stop;
- chunk size, shard size, or streaming strategy;
- required-column or prefilter rules;
- concurrency limit;
- spill/output location;
- cache and dirty-page risk on mounted or network-backed paths;
- partial progress artifact plan;
- stop condition and reusable blocked-state callback.

Default rules:

- Do not run unbounded full-table reads, full CSV/JSON loads, or broad in-memory materialization over multi-GB files.
- Prefer streaming, required-column reads, chunking, indexes, exact-key filters, or bounded shards.
- Default to one worker for large IO unless the parent explicitly approves parallel shards.
- If memory pressure appears, stop instead of pushing through; return completed shard/file-offset state and reusable partial ledgers.
- Do not assume filesystem cache is harmless after it has caused host pressure. Prevent excessive RSS, page cache, dirty-page, and writeback pressure in the task design.
- Do not clear caches, kill unrelated processes, reboot environments, or restart services as a normal recovery path unless explicitly approved or the system is unrecoverable.

For user-visible reports about memory pressure, report process RSS, available memory, cache/dirty/writeback indicators, and active task owners before proposing disruptive recovery.
