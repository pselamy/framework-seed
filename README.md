# Framework Seed — public orchestration + knowledge-management methodology

> The privacy-scrubbed, generalized ESSENCE of an agent-orchestration + knowledge-management discipline — distilled to be published as a public, thought-leadership repo + Claude Code plugin. It is **NOT** any private fleet. No internal org/agent/product names, no infrastructure specifics, no billing/subscription model.

## What this is

A small number of agents (or one) doing real engineering work need three things to stay trustworthy at scale:

1. **A discipline that makes "done" mean done** — evidence at the real artifact, not a marked checkbox. *(This is the differentiating core. Lead with it.)*
2. **Knowledge that compounds instead of siloing** — one fact, one file, one index; capture once, route to the right home.
3. **Changes that are reproducible and reversible** — declarative infrastructure, runtime knobs over restarts, every incident becomes a permanent guard.

Most "agent framework" content sells the *plumbing* (routing, tools, memory stores). The scarce, valuable thing is the **discipline layer** — the rules and the *enforcement hooks* that stop an autonomous worker from confidently lying about completion. That is what this seed is known for.

## Two layers

### Layer 1 — Methodology Handbook (the WHY)  ·  `HANDBOOK.md`
Docs-first. The principles you run on, each stated so a reader can adopt it without our tooling. This is the thought-leadership.

### Layer 2 — Working Starter Kit (the HOW)  ·  *built on existing marketplaces, not greenfield*
Packaged as a **Claude Code plugin** so "clone/install and you have it" works and others can contribute back. **Do not greenfield this** — build on and contribute to the existing serious ecosystem:

- **`prime-radiant-inc/iterative-development`** (Apache-2.0, Claude Code plugin) — its core principle is ours: *completion = behavior evidence at the correct seam, not marked-done*, plus paired-auditor parallel-adversarial review. Build on it.
- **`superpowers`** — the companion plugin set it pairs with.

We publish **our differentiating pieces as plugins/contributions** on top of that ecosystem:
- the **enforcement hooks** — a *done-guard* (block a "done" with no artifact evidence), a *reprioritize-guard* (workers may not demote queue priority — that's the orchestrator's call), *PR-hygiene* (one PR per unit, auto-merge-at-creation, evidence-requires-merged);
- the **priority queue** — already public: **`laneq`** (lease-based, priority-ordered, multi-consumer);
- the **regression-ratchet** convention;
- the **knowledge-as-files** memory framework + index convention;
- the **self-drive loop** (interval or self-paced) with a watchdog;
- a **`CLAUDE.md` template**.

Connect the existing public pieces (`laneq`, the public skills repo) rather than duplicate them.

## Naming proposals (pick one — owner's call)

| Name | Angle |
|---|---|
| **Proof of Work** | the discipline core: nothing is done without proof at the artifact. (Reclaims the phrase for engineering rigor.) |
| **Ledger** | every claim has an entry + evidence; the queue + memory are append-mostly ledgers. |
| **Keel** | the unglamorous structural spine that keeps an autonomous fleet upright. |
| **Plumb** | "true to plumb" — straight, verified, level; pairs with the false-done discipline. |
| **Cairn** | knowledge-as-files: each fact a marked stone; the trail compounds. |

## Guardrails (non-negotiable before publish)
- **Scrub all internal specifics** — agent names, org names, product names, the real fleet, any secret. Privacy-scanner gate (same bar as the public skills repo) must pass.
- **Never expose the billing/subscription/quota model.** Frame everything on the architecture and the discipline, never on how it is paid for.
- **Gold-bar quality only.** Slop kills the credibility flywheel. Ship fewer, better principles.
- **Pattern-only.** Ideas drawn from public/proven sources; never copy code from unlicensed/proprietary projects.
- **Consumable & version-pinned, NOT a codegen-scaffold you fork.** Ship Layer 2 as an *installable, pinned* plugin/template built on a *maintained* upstream — not a generator that emits framework code which then drifts from the generator and locks to one vendor. ("A maintained framework people pin to" > "another stale scaffold." Keep it maintained or don't ship it.) Derived from the AgentStack scout.

## Verify (real artifact)
This seed is *done* only when: a **public repo** exists with the handbook + an **installable plugin/template**, the **privacy scan is green**, and it **cleanly bootstraps a fresh orchestrated-fleet-with-knowledge-management setup using only public pieces**. This repo delivers **Layer 1 (the handbook)**; Layer 2 (an installable plugin/template built on prime-radiant/superpowers) is in progress.
