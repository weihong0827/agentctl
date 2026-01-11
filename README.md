# 🎛️ Agent Orchestrator

> Centralized management for AI coding agents — shared MCPs, sandboxed execution, pluggable workflows.  
> **One config, whole team.**

---

## The Problem

AI coding agents are transforming how developers work. But as teams adopt these tools, new challenges emerge:

| Challenge | What Happens |
|-----------|--------------|
| **Configuration Drift** | Alice has GitHub + Postgres MCPs. Bob only has filesystem. Carol's running outdated versions. Nothing works consistently. |
| **No Standardization** | What does "code review" mean? Which MCPs? What system prompt? Every developer reinvents workflows independently. |
| **Security Concerns** | Agents with full system access can read secrets, hit production APIs, modify critical files. Teams either accept risk or avoid agents entirely. |
| **Onboarding Friction** | New devs spend hours configuring tools. Docs get stale. Tribal knowledge grows. The gap between "installed" and "productive" widens. |
| **Agent Lock-in** | Today it's Claude Code. Tomorrow, Codex. Next month, something open-source. Switching means rebuilding everything. |

---

## The Vision

A unified layer between humans and AI agents — abstracting away the chaos.

```
                         ┌─────────────────────────────────┐
                         │       Human Interfaces          │
                         │    CLI  •  Slack  •  Web GUI    │
                         └───────────────┬─────────────────┘
                                         │
                                         ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                                                                            │
│                          Agent Orchestrator                                │
│                                                                            │
│    ┌────────────────┐    ┌────────────────┐    ┌────────────────┐         │
│    │                │    │                │    │                │         │
│    │   MCP Configs  │    │   Workflows    │    │    Agents      │         │
│    │   (shared)     │    │  (templates)   │    │  (pluggable)   │         │
│    │                │    │                │    │                │         │
│    └────────────────┘    └────────────────┘    └────────────────┘         │
│                                                                            │
└────────────────────────────────────────┬───────────────────────────────────┘
                                         │
                                         ▼
                          ┌─────────────────────────────────┐
                          │           Sandbox               │
                          │   Docker  •  Firecracker  •  …  │
                          └───────────────┬─────────────────┘
                                          │
                                          ▼
                          ┌─────────────────────────────────┐
                          │          AI Agents              │
                          │  Claude Code  •  Codex  •  …    │
                          └─────────────────────────────────┘
```

---

## Core Concepts

### 🔌 Shared MCPs

MCPs (Model Context Protocol servers) extend what agents can do — filesystem access, GitHub, databases, and more.

**The old way:** Every developer configures MCPs locally. Configs diverge. Debugging is painful.

**The new way:** Define MCPs once in version control. Everyone gets the same capabilities. Update in one place, propagate everywhere.

```
MCPs defined centrally
        │
        ├──▶ Dev 1 (frontend) gets: filesystem, github
        ├──▶ Dev 2 (backend) gets:  filesystem, github, postgres  
        └──▶ Dev 3 (data) gets:     filesystem, postgres, bigquery
```

---

### 📋 Workflow Templates

Workflows are **preconfigured agent setups** for common tasks — combining an agent, MCPs, and system prompts into a reusable template.

| Workflow | Agent | MCPs | Purpose |
|----------|-------|------|---------|
| `code-review` | Claude Code | filesystem, github | PR review with context |
| `feature-dev` | Claude Code | filesystem, github, memory | Building new features |
| `data-analysis` | Claude Code | filesystem, postgres | Query and analyze data |
| `quick-task` | Claude Code | filesystem | Simple one-off tasks |

**Why it matters:**
- New devs run `agent-orch run --workflow code-review` on day one
- Consistent behavior across the team
- Best practices encoded, not documented

---

### 🤖 Pluggable Agents

Today's best agent might not be tomorrow's. Agent Orchestrator abstracts the underlying agent:

```
                    ┌─────────────────────┐
                    │  Your Workflows     │
                    │  Your MCPs          │
                    │  Your Team Config   │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
        Claude Code         Codex         Open Source
```

Switch agents without rewriting workflows. Compare performance across agents. Avoid lock-in.

---

### 🔒 Sandboxed Execution

Agents run in isolated environments with explicit boundaries:

| Layer | Protection |
|-------|------------|
| **Filesystem** | Only mounted workspace is accessible |
| **Network** | Configurable — allow, deny, or allowlist |
| **Resources** | CPU and memory limits prevent runaway processes |
| **Secrets** | Injected at runtime, never stored in agent context |

**Choose your isolation level:**
- **Docker** — Good balance of security and convenience
- **Firecracker** — Stronger isolation for sensitive workloads
- **Local** — No isolation, for trusted development

---

### 🖥️ Flexible Interfaces

Same orchestrator, multiple ways to interact:

| Interface | Use Case |
|-----------|----------|
| **CLI** | Developers in terminal |
| **Slack** | Non-technical team members, quick asks |
| **Web GUI** | Config management, workflow builder |
| **API** | CI/CD integration, custom tooling |

The interface changes. The underlying config stays consistent.

---

## Who Is This For?

| Role | Benefit |
|------|---------|
| **Engineering Leads** | Standardize AI tooling across the team |
| **Platform Teams** | Provide secure, governed agent access |
| **Individual Devs** | Stop configuring, start building |
| **Security Teams** | Audit and control agent capabilities |

---

## Design Principles

1. **Config as Code** — Everything in version control, reviewable, auditable
2. **Sensible Defaults** — Works out of the box, customize when needed  
3. **Escape Hatches** — Never block power users from going deeper
4. **Agent Agnostic** — Today's choice shouldn't be tomorrow's regret
5. **Security by Default** — Sandboxed unless explicitly opted out

---

## Roadmap

| Phase | Focus |
|-------|-------|
| **v0.1** | CLI + Docker sandbox + Claude Code |
| **v0.2** | Additional agents (Codex, open-source) |
| **v0.3** | Slack interface |
| **v0.4** | Web GUI for config management |
| **v1.0** | Team features — sharing, permissions, audit logs |

---

## Getting Started

See [IMPLEMENTATION.md](./IMPLEMENTATION.md) for setup instructions and technical details.

---

## License

MIT

---

<p align="center">
  <i>Stop syncing configs. Start shipping code.</i>
</p>