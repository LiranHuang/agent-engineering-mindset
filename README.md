# 🧭 Agent Engineering Mindset

A deployable OpenClaw skill for keeping multi-agent engineering work aligned, isolated, verifiable, and runnable from one clean canonical source.

It helps a manager or parent agent answer five questions before work continues:

1. **Are we advancing the real stage goal?**
2. **Are we changing the true target instead of a substitute?**
3. **Are we reusing the canonical owner instead of creating another path?**
4. **Can this change be verified, merged, and promoted safely?**
5. **Who owns every artifact until it is cleaned, archived, or adopted?**

## ✨ What it governs

- 🧭 **Stage anchors** — goals, continuation base, target surface, boundaries, and acceptance source.
- 🌳 **Isolated worktrees** — one writable owner per task with explicit base revision and path ownership.
- ✅ **Verified merges** — test the task branch, inspect semantic conflicts, then retest the merged tree.
- 🚀 **Clean canonical runtime** — run services only from an explicitly verified canonical commit.
- ♻️ **Artifact minimality** — reuse or repair before creating files, scripts, agents, services, state, or entrypoints.
- 🗂️ **Lifecycle ownership** — every retained entity has an owner, status, reference, and cleanup condition.
- 📦 **Resource envelopes** — bound memory, IO, cache pressure, concurrency, and partial progress.
- 🔐 **Approval boundaries** — separate safe continuation from actions requiring explicit authority.
- 🤝 **Parent continuation** — child PASS is evidence; the parent still drives the original request to terminal.
- 💬 **Minimal reporting** — expose decision-relevant facts without dumping internal process.

## 🧠 Operating model

```text
stage anchor
    ↓
aligned smallest next action
    ↓
reuse / repair / lifecycle decision
    ↓
resource + approval boundaries
    ↓
isolated task worktree
    ↓
verified task commit
    ↓
canonical merge + merged-tree tests
    ↓
clean canonical runtime
    ↓
parent acceptance + lifecycle cleanup
```

## 🚀 Start here

The repository root is the installable skill package because it contains `SKILL.md`.

From the repository root, install for all local agents:

```bash
openclaw skills install . --global
```

If an older copy already exists, review the difference first. Replace it only intentionally:

```bash
openclaw skills install . --global --force
```

### Install for one agent

```bash
openclaw skills install . --agent <agent-id>
```

### Install from a remote Git repository after publication

```bash
openclaw skills install git:<owner>/agent-engineering-mindset --global
```

OpenClaw may present an install-policy review. Approve only the exact reviewed package; never bypass a blocked policy.

## 🔍 Verify installation

```bash
openclaw skills info agent-engineering-mindset --json
openclaw skills check --agent <agent-id> --json
```

For agents with an explicit `agents.entries.<id>.skills` allowlist, add `agent-engineering-mindset` to the existing list without replacing unrelated entries.

## 🧩 Skill boundaries

- This standalone skill owns stage alignment, parent continuation, worktree/merge/runtime integrity, artifact lifecycle, resource bounds, decision gates, and minimum repair verification.
- Routing and collaboration layers may transport work, but must not redefine these engineering gates.
- Project anchors and project-specific vocabulary remain inside each project, never in this generic package.

## 📦 Repository layout

```text
SKILL.md                                      # compact executable procedure
README.md                                     # human deployment guide
references/stage-anchors-and-continuation.md  # parent-state gates
references/version-workspace-runtime.md       # branch/worktree/merge/runtime rules
references/artifact-lifecycle.md              # minimality and cleanup semantics
references/approval-and-resource-boundaries.md # approvals and bounded IO/RSS
references/communication-and-verification.md  # reporting and final checks
```

## 🛡️ Safety defaults

- A local commit does not authorize push, merge, deployment, or restart.
- A clean textual merge does not prove semantic compatibility.
- A child result does not close the parent request.
- A delivered approval request does not authorize the protected action by itself.
- A temporary artifact without cleanup ownership is unfinished work.
- A runtime must not depend on an unmerged worktree or dirty checkout.

## 📄 License

MIT — see [`LICENSE`](LICENSE).
