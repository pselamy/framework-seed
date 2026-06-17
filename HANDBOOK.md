# The Handbook

Nine principles for orchestrating agents that don't lie about being done. Ordered from the differentiating core outward; each stands on its own. The first three are the enforcement layer — the reason this exists.

---

## 1. "Done" means proven

A task is not done because a model said so, a checkbox flipped, or CI went green. It is done when there is **evidence at the real artifact** and a **reproducible path** to that result.

- "Tests pass" is not done if the tests were weakened to pass.
- "PR opened" is not done — **merged** is done.
- "Email sent" is not done until the message is in the sent record.
- "Deployed" is not done until the running system serves the new behavior.

Grade the trajectory, not just the output: a right answer reached by shortcutting, leaking, or overfitting to the check is a latent failure. Define *done* — naming the artifact and the required evidence — before work starts.

## 2. Verify the artifact, not a proxy

Logs, exit codes, and "it should work" are proxies. Verify the thing itself: open the deployed page, read the sent message, query the row, run the binary. The most common false-done is trusting a green pipeline over the feature actually working. When you report *done / passed / sent / green*, you are asserting you looked. If you didn't, say so.

## 3. Enforce at the tooling layer, not the prompt

A rule in a prompt is advisory; a worker under pressure routes around it. Move the rule into a hook the harness runs, so it can't be skipped:

- **done-guard** — block "mark done" that carries no artifact evidence.
- **reprioritize-guard** — a worker may not *demote* a queue item. Priority is the orchestrator's call; "blocked" is a status, not a lower priority. Without this, workers quietly demote whatever's inconvenient and fight the plan.
- **PR-hygiene** — one PR per unit; auto-merge at creation; "PR" counts only when *merged*; block duplicate PRs for the same task.

The generalization: **if a behavior must always happen, make the machine do it.** Memory and good intentions don't survive restarts and context windows.

## 4. Regression ratchet — every incident becomes a test

You may be surprised by a failure once. The PR that fixes an incident must also add the guard that catches its class next time:

- packaging issue → structure test
- runtime edge case → unit case
- config/permission gap → policy test
- otherwise → at minimum an alert

Recur once, never twice. The test suite becomes the accumulated memory of everything that has bitten you.

## 5. Knowledge as files — one fact, one file, one index

Knowledge in a chat or one agent's head is lost on restart and invisible to everyone else. Make it durable and shared:

- **One fact per file**, with frontmatter (slug, one-line description for recall, type).
- A **type taxonomy** — who the user is / how to work / project state / pointers to external resources. Different types, different rules.
- A **single index** loaded every session: one line per fact, pointing at the file. The index is the map; the files are the territory.
- **Capture once, then route** — a learning becomes a skill (reusable procedure), a memory (durable fact), a doc (long-form), or a pointer. Don't re-derive it; don't ask the human for what the system already records.
- **Link liberally.** A dangling link marks a fact worth writing later.
- **Recalled knowledge reflects when it was written** — verify a named file or flag still exists before acting on it.

## 6. Capture once, route — stop being the human bus

If the human is the only path between two agents' knowledge, the system is broken. Every learning is captured once and routed to a system-of-record. **Check the system-of-record before asking a human.** The human's attention is the scarcest resource — spend it only on what genuinely needs it (an approval, a credential, a judgment call), and make that surface explicit and current.

## 7. Infrastructure as declarative source of truth

Every infra, repo-setting, cloud-resource, secret, cron, runtime-config, or deployment change goes through a declarative source of truth — committed, reviewable, reproducible — never a hand-run command on a box. A workload must be reconstructable from its declared state alone.

- **Secrets via a declared mapping from a secret store**, never hand-placed files.
- **Prefer keyless / federated identity** over long-lived static credentials; store a secret only where no federation exists.
- **Adopt-and-delete** — a migration removes the code it supersedes in the same change. A half-migration that leaves both paths live is a future incident.

## 8. Build on standards; tune at runtime; don't restart

- **Library-first.** Reach for a proven open standard (observability, config, API typing, CI, auth) before custom machinery. Custom code is a liability you maintain forever.
- **Max knobs, no restart.** Operational behavior — limits, strategy, feature flags — should be tunable at runtime. Restarts drop live state and cause the outages they're meant to fix.
- **Reproducible and reversible.** The whole environment as committed, rollback-able state.

## 9. Identity, routing, the self-improving loop

- **Each actor acts as itself**, with its own scoped credential. An orchestrator commits as the product or ops identity of the repo it touches — never as a human, never as a worker's account. Clean attribution is a security property, not cosmetics.
- **Dispatch with one-writer discipline.** One writer per unit; validate the produced artifact; hand off via durable state, not a live session.
- **Turn failure into durable improvement.** A learning becomes a skill, a memory, or a test (principles 4 + 5 closing the loop). The goal is a fleet that needs *fewer* corrections over time, not the same one repeated.
