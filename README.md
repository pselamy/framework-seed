# framework-seed

A discipline for running autonomous agents that don't lie about being done.

Most "agent framework" content sells plumbing: routing, tools, memory stores. The hard part isn't plumbing. It's stopping a worker from confidently reporting success it can't prove, and not losing what it learned the next time it restarts.

This repo is the public methodology for that, plus (in progress) an installable kit to enforce it.

## The core idea

Three things keep an autonomous fleet trustworthy:

1. **"Done" means proven**: evidence at the real artifact, not a flipped checkbox or a green pipeline.
2. **Knowledge compounds**: one fact, one file, one index; captured once, routed to a home, not stuck in a chat log.
3. **Changes are reproducible and reversible**: declarative infra, runtime knobs over restarts, every incident becomes a permanent guard.

The differentiator is the **enforcement layer**: hooks the harness runs so the rules can't be skipped, a priority queue the worker can't quietly reprioritize, and a regression ratchet that turns each failure into a test. Principles in a prompt are advisory. Hooks are not.

## What's here today

- **`HANDBOOK.md`**: nine principles, each stated so you can adopt it without this tooling. This is the whole methodology and it's usable as-is.

## What's coming

An installable Claude Code plugin that enforces the handbook:

- **done-guard**: block a "mark done" with no artifact evidence.
- **reprioritize-guard**: workers can't demote queue priority; "blocked" is a status, not a lower priority.
- **PR-hygiene**: one PR per unit, auto-merge at creation, "merged" (not "opened") counts as evidence.
- A `CLAUDE.md` template and a self-drive loop with a watchdog.

This builds on the existing ecosystem rather than reinventing it: [`prime-radiant-inc/iterative-development`](https://github.com/prime-radiant-inc/iterative-development) (Apache-2.0; same completion-is-evidence principle, plus paired adversarial review) and the [`superpowers`](https://github.com/obra/superpowers) plugin set. The queue is the public [`laneq`](https://github.com/selamy-labs/laneq) (lease-based, priority-ordered). We add the enforcement pieces those don't have.

The kit is not built yet. Until it ships, treat this as a handbook.

## Design constraints

- **Installable and version-pinned, not a scaffold you fork.** A maintained plugin you pin to beats another stale generator that drifts and locks you to one vendor.
- **Pattern-only.** Ideas drawn from public, proven sources; no code copied from proprietary projects.

## License

Apache-2.0. See [`LICENSE`](LICENSE). Matches the ecosystem this builds on, so adopt, fork, and contribute back freely.
