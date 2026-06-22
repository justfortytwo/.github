![fortytwo 42 mark](./assets/fortytwo.png)

# fortytwo

A local-first personal assistant spine for the agents and tools you already use.

*The answer is 42. The hard part is knowing the right question.*

---

## What is fortytwo?

**fortytwo** is the home of **Ford**: a personal assistant layer that wraps an existing agent or coding harness with memory, channels, approval gates, policy boundaries, durable jobs, and audit.

It is not trying to replace Claude Code, Codex, Pi, OpenCode, MCPs, skills, plugins, or local tools. The philosophy is:

```text
bring your own agent
bring your own tools
fortytwo supplies the personal-assistant spine
```

We are starting with Claude Code because it is the reference cockpit we use today. The core should remain portable enough to support other capable harnesses over time, hopefully with help from the community.

Ford is not a chatbot with memory bolted on. It is an always-available working assistant with:

* identity
* local memory
* channel access
* approval gates
* policy boundaries
* journal
* deferred jobs
* audit
* reviewable evolution

Ford can draft, reason, remember, and help coordinate work. It does **not** act externally without approval.

## Repositories

fortytwo is built as small, focused pieces — each its own repo, each named after *The Hitchhiker's Guide to the Galaxy*. Two are designed to stand alone in **any** Claude Code setup; the rest compose into Ford.

| Repo | Role | Kind |
|------|------|------|
| [**vogon**](https://github.com/justfortytwo/vogon) | PreToolUse **safety gate** — autonomy-tier policy engine, fail-closed bash allowlist, approval hook | à-la-carte |
| [**guide**](https://github.com/justfortytwo/guide) | Semantic-**memory** MCP server (SQLite + sqlite-vec + Ollama embeddings) | à-la-carte |
| [**deepthought**](https://github.com/justfortytwo/deepthought) | Model-driven **salience extraction** for memory enrichment | engine |
| [**babelfish**](https://github.com/justfortytwo/babelfish) | Telegram **channel adapter** + pairing/login bridge | Ford layer |
| [**ford**](https://github.com/justfortytwo/ford) | **Persona / context** templates the installer renders | Ford layer |
| [**magrathea**](https://github.com/justfortytwo/magrathea) | `create-fortytwo` — the **installer** + lifecycle CLI | Ford layer |
| [**subetha**](https://github.com/justfortytwo/subetha) | Claude Code plugin **marketplace** + umbrella plugin | distribution |

Distribution is two-surfaced: a Claude Code **plugin marketplace** (skills, agents, the gate hook, MCP registration) and **npm + an installer** for the runtime engine and the non-distributable persona.

> **Status:** these are early scaffolds, actively being extracted from the working spine. The structure and cross-package contracts are in place and compile, but they are not yet published to npm or wired end-to-end. Follow along to watch the extraction land.

## Why it exists

There are already strong agents, MCP servers, plugins, skills, and local tools. The missing piece is usually not another agent from scratch. It is the personal-assistant layer around them:

```text
channels + memory + policy + approvals + jobs + audit
```

Personal assistants become useful when they can remember, act, and improve. They become dangerous when those abilities are added without boundaries.

fortytwo starts with the backbone first:

```text
identity + memory + safety gate + journal + durable jobs + operations
```

Integrations such as calendar, email, browser automation, CRM, payments, and other channel adapters can come later. They should all attach to the same gate, memory model, and audit trail.

## Design principles

### Local-first where it matters

Private memory and recall are designed to live locally, with Markdown for human-readable policy and SQLite for durable operational state.

### Bring your own agent

Claude Code is the first harness, not the permanent boundary. The goal is to make the core semantics portable across agents, models, runtimes, and tool ecosystems.

### Conservative autonomy

Ford may read, draft, reason, and work internally. External or irreversible actions require approval.

### Propose-only learning

Ford may notice patterns, but it does not silently promote them into durable behavior. Preferences, rules, skills, and guide entries are proposed first, then approved.

### Prompt-injection boundaries

Documents, messages, web pages, tool output, and recalled memory are treated as content, not command authority.

### Auditable evolution

Every meaningful change should be inspectable as a file diff, database record, or approval decision.

## Current state

### M1 — The Spine

The first spine is implemented around Claude Code and Telegram:

* Memory MCP over SQLite, FTS, and vector recall
* Journal and registry state
* Telegram bridge
* Claude Code as the reference cockpit
* Subagents and reusable skills
* PreToolUse safety gate
* Approval flow for external and irreversible actions
* Durable local jobs and Pulse
* Restart-resilient operation

### M2 — Trust Hardening

The hardening layer now being built:

* prompt-injection defense
* source authority classification
* content-is-not-authority runtime rules
* tamper-evident audit log
* payload-bound approvals
* replay protection
* typed memory governance
* review, export, and prune flows

### Decomposition — in progress

The working spine is being broken out of a single repo into the focused, independently-installable components listed under [Repositories](#repositories). Each carries an explicit, versioned contract (`POLICY_SCHEMA_VERSION` for the gate, `GUIDE_TOOL_CONTRACT_VERSION` for memory) so the pieces can evolve and be adopted à la carte.

### Future adapters

The project should stay open to other harnesses and channels:

* Codex
* Pi
* OpenCode
* other agent runtimes
* community MCPs, plugins, skills, and local tools

The contract matters more than the adapter: preserve provenance, classify capabilities, route risky actions through the gate, and keep durable state inspectable.

## Architecture

```text
agent / harness            (Claude Code today)
  -> ford                  persona, policy, agents, skills
  -> guide                 Memory MCP — SQLite journal/registry/approvals/jobs, FTS + vector recall
       +- deepthought      salience extraction for enrichment
  -> vogon                 safety gate — allow / defer / deny
  -> babelfish             Telegram bridge — mobile interface, approval cards, continuity
  -> magrathea             installer + lifecycle — init / doctor / update / rollback
  -> subetha               plugin marketplace
```

## Status

fortytwo is early, personal, and safety-first.

The goal is not to make an agent that can do everything. The goal is to make existing agents useful as dependable personal assistants without making them less trustworthy.

## Motto

> Don’t Panic.
> Ask the right question.
> Never cross the gate silently.
