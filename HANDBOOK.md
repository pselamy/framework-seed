# The Handbook — principles for orchestrating agents that don't lie about being done

> Nine principles, ordered from the differentiating core outward. Each is stated to be adopted on its own. The first three are the discipline layer — the reason this exists.

---

## 1. Process-aware "done" — grade the journey, not just the destination
A task is not done because a model said so, or because a checkbox flipped, or because CI is green. It is done when there is **evidence at the real artifact** *and* a **legitimate, reproducible path** to that result.

- "Tests pass" is not done if the tests were weakened to pass.
- "PR opened" is not done — **merged** is done.
- "Email sent" is not done until the message exists in the sent record.
- "Deployed" is not done until the running system serves the new behavior.

Grade the **trajectory**: was the path sound, or did it shortcut, leak, or overfit to the check? A right answer reached the wrong way is a latent failure. Operationalize this with a *done* definition that names the artifact and the evidence required *before* work starts.

## 2. Verify the real artifact, not a proxy
Logs, exit codes, and "it should work" are proxies. Verify the **thing itself**: open the deployed page, read the sent message, query the row that should exist, run the binary. The most common false-done is trusting an indirect signal (a green pipeline) over the direct one (the feature working). When you report *done / passed / sent / green*, you are asserting you looked at the artifact. If you didn't, say so.

## 3. Enforce the discipline at the tooling layer, not the prompt
Principles in a prompt are advisory; an autonomous worker under pressure will route around them. Move the rule into a **hook the harness executes**, so it cannot be skipped:

- **Done-guard** — block a "mark done" that carries no artifact evidence.
- **Reprioritize-guard** — a worker may not *demote* a queue item's priority. Priority is the orchestrator's call; "blocked" is a *status*, not a lower priority. (Without this, workers quietly demote anything inconvenient and fight the plan.)
- **PR-hygiene** — one PR per unit of work; enable auto-merge at creation; "PR is evidence" requires *merged*, not *opened*; block duplicate PRs for the same task.

The lesson generalizing all three: **if a behavior must always happen, make the machine do it.** Memory and good intentions don't scale across restarts and context windows.

## 4. Regression-ratchet — every incident becomes a test in the same fix
You are allowed to be surprised by a failure **once**. The fix PR that resolves an incident must also add the guard that catches its class next time:

- container/packaging issue → a structure test
- runtime edge case → a unit case
- config/permission gap → a policy test
- anything else → at minimum an alert

Recur once, never twice. The test suite becomes the accumulated memory of everything that has ever bitten you.

## 5. Knowledge as files — one fact, one file, one index
Knowledge that lives in a chat or one agent's head is lost on the next restart and invisible to everyone else. Make it durable and shared:

- **One fact per file**, with frontmatter (a slug, a one-line description used for recall, a type).
- A **type taxonomy** — e.g. *who the user is* / *guidance on how to work* / *ongoing project state* / *pointers to external resources*. Different types, different rules.
- A **single index** loaded every session — one line per fact, pointing at the file. The index is the map; the files are the territory.
- **Capture once, then route** — when something is learned, decide its home: a *skill* (reusable procedure), a *memory* (durable fact), a *wiki/doc* (long-form), or a pointer. Don't re-derive it next time; don't ask the human for what the system already records.
- **Link liberally** between facts; a dangling link marks a fact worth writing later.
- **Recalled knowledge reflects when it was written** — verify a named file/flag still exists before acting on it.

## 6. Capture-once, route — stop being the human bus
If the human is the only path between two agents' knowledge, the system is broken. Centralize: every learning is captured once and routed to a system-of-record. **Check the system-of-record before asking a human.** The human's attention is the scarcest resource; spend it only on what genuinely requires a human (an approval, a credential, a judgment call) — and make that surface explicit and current.

## 7. Infrastructure as declarative source-of-truth — never ad-hoc
Every infrastructure, repo-setting, cloud-resource, secret, cron, runtime-config, or deployment change goes through a **declarative source of truth** (committed, reviewable, reproducible), never a hand-run command on a box. A workload must be reconstructable from its declared state alone. Corollaries:

- **Secrets via a declared mapping from a secret store**, never hand-placed files.
- **Prefer keyless / federated identity** over long-lived static credentials; store a secret only where no federation exists.
- **Adopt-and-delete** — a migration must *remove* the code/config it supersedes, in the same change. A half-migration that leaves both paths live is a future incident.

## 8. Build on standards; tune at runtime; don't restart
- **Library-first.** Reach for a proven open standard (observability, config, API typing, CI, auth) before custom machinery. Custom code is a liability you maintain forever.
- **Max knobs, no restart.** Operational behavior (limits, strategy, feature flags) should be tunable **at runtime without a restart**. Restarts are scar tissue — they drop live state and cause the very outages they're meant to fix.
- **Reproducible + reversible.** The whole environment as committed, reproducible, rollback-able state (the declarative ideal). Fewer restarts, more resilient workers.

## 9. Identity, routing, and the self-improving loop
- **Routing identity.** Each actor acts *as itself* with its own scoped credential; an orchestrator commits as the product/ops identity of the repo it touches, never as a human and never as an autonomous worker's account. Clean attribution is a security and accountability property, not cosmetics.
- **Dispatch with one-writer discipline.** Route work to the right actor; one writer per unit; validate the produced artifact; hand off via durable state, not a live session.
- **Learn from failure, structurally.** The system should turn its own failures into durable improvements — a learning becomes a skill, a memory, or a test (this is principles #4 + #5 closing the loop). The goal is a fleet that needs *fewer* corrections over time, not the same correction repeated.

---

### The through-line
Every principle here exists to make an autonomous system **trustworthy without supervision**: it cannot falsely claim done (#1–#3), it cannot repeat a known failure (#4), it doesn't lose or silo what it learns (#5–#6), its changes are reproducible and reversible (#7–#8), and it acts with clean identity while getting better on its own (#9). That trustworthiness — not the plumbing — is the product.
