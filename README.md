# Framework Seed

> A discipline layer for autonomous engineering agents: proof before done, durable knowledge, and tooling hooks that make reliable behavior the default.

Framework Seed is not another agent scaffold. It is the operating method behind agents that can work for longer than a chat window without drifting, forgetting, or claiming victory too early.

The core idea is simple:

**An agent fleet does not become trustworthy by adding more tools. It becomes trustworthy when the harness forces good engineering behavior.**

## Why this exists

Most agent projects start with plumbing:

- model routing
- tools
- memory stores
- orchestration graphs
- queues

Those matter, but they are not the hard part. The hard part is operational discipline:

- How does an agent prove that work is actually done?
- Where does new knowledge live after the session ends?
- What prevents a worker from silently lowering priority on inconvenient work?
- How does a failure become a permanent guard instead of a repeated lesson?
- How do multiple agents act with clean ownership and auditable handoffs?

Framework Seed names those rules and turns them into reusable methodology.

## The product shape

### 1. Proof of work

Completion requires evidence at the real artifact, not a proxy.

- A deployed feature is checked in the running product.
- A sent message is verified in the sent record.
- A merged change is verified after CI and deploy, not when the PR opens.
- A bug fix includes the regression guard that would catch the same class next time.

### 2. Knowledge as files

Agent memory should compound across restarts and across workers.

Framework Seed uses a file-backed knowledge model:

- one fact per file
- frontmatter for type and recall
- a session-loaded index
- links between related facts
- clear routing between memory, skills, docs, state, and external pointers

### 3. Enforcement hooks

If a behavior must always happen, the harness should enforce it.

Examples:

- **done-guard**: block a completion claim with no artifact evidence
- **reprioritize-guard**: workers may mark work blocked, but may not demote priority
- **PR hygiene**: one unit of work, one PR, merged evidence before done
- **regression-ratchet**: each incident adds a test, policy, alert, or guard

### 4. Public building blocks

Framework Seed is meant to connect public pieces rather than hide behind private infrastructure.

Related public surfaces:

- [Patrick Selamy on GitHub](https://github.com/pselamy)
- [selamy.dev](https://selamy.dev)
- [resume.selamy.dev](https://resume.selamy.dev)
- [agent-skills](https://github.com/selamy-labs/agent-skills)
- [laneq](https://github.com/selamy-labs/laneq)
- [resume-as-code](https://github.com/pselamy/resume)

## What this repo contains

| File | Purpose |
|---|---|
| [`HANDBOOK.md`](./HANDBOOK.md) | The core operating principles for autonomous engineering agents |
| `README.md` | The public product frame and entry point |

Layer 1 is the handbook: the method.

Layer 2 is an installable plugin/template built on existing serious ecosystems rather than a greenfield framework. The intended direction is to contribute enforcement hooks, templates, and conventions on top of maintained public agent tooling.

## Design principles

1. **Real artifact over proxy**  
   Logs and green checks are not enough when the user-facing artifact is what matters.

2. **Machine-enforced discipline**  
   Critical rules belong in hooks, policies, and workflow gates, not only in prompts.

3. **Knowledge compounds**  
   Every durable learning gets routed to the right system of record.

4. **Reproducible change**  
   Infrastructure, repo settings, runtime config, and deployment state should be declarative and reviewable.

5. **Clean identity**  
   Each actor should act with scoped credentials and auditable ownership.

## Non-goals

Framework Seed does not publish private fleet details, internal agent names, billing mechanics, credentials, or proprietary implementation specifics.

It is a public methodology repo: enough to teach the pattern, not enough to leak the private system it came from.

## Status

This repo currently publishes the handbook and product framing. The installable plugin/template layer is still in progress.

Gold bar for the next version:

- privacy scan passes
- links are live
- plugin/template installs cleanly from public sources
- a fresh setup can demonstrate proof-of-work, knowledge-as-files, and done-guard behavior without private dependencies
