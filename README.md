# Self-Hosted AI Infrastructure

Three Linux servers running containerized AI agents, automation workflows, vector memory, and live publishing properties — connected by a private WireGuard VPN mesh.

Built and operated by [Nathan Cowlishaw](https://www.linkedin.com/in/nateaz/) · Infrastructure started May 2026

---

## Architecture Overview

```
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│    Agent Server     │     │  Publishing Server   │     │     Hub Server      │
│                     │     │                      │     │                     │
│  AI Agents          │     │  nginx               │     │  Qdrant (vector DB) │
│  Automation         │◄────►  WordPress (3 sites) │◄────►  n8n (automation)  │
│  Workflows          │     │  Ghost               │     │  Ollama (local LLM) │
│                     │     │  Postiz              │     │  WireGuard hub      │
└─────────────────────┘     └─────────────────────┘     └─────────────────────┘
         │                           │                           │
         └───────────────────────────┴───────────────────────────┘
                              WireGuard VPN Mesh
```

All three servers communicate over a private WireGuard VPN mesh. No service is exposed directly — all public traffic routes through nginx reverse proxies.

---

## Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| OS | Linux (Debian/Ubuntu) | All three servers |
| Containers | Docker + Docker Compose | All services containerized |
| Networking | WireGuard | Private VPN mesh between servers |
| Reverse Proxy | nginx | TLS termination, routing |
| Local LLM | Ollama | On-premise model inference |
| Vector DB | Qdrant | Agent memory, semantic search |
| Automation | n8n | Workflow orchestration, webhooks |
| Publishing | WordPress (×3) | Live content properties |
| Publishing | Ghost | Long-form publishing |
| Publishing | Postiz | Social scheduling |
| Version Control | GitHub | Canonical system truth |

---

## Live Properties

| Site | Role |
|------|------|
| [turquoiseufo.net](https://turquoiseufo.net) | Primary organism publishing site |
| [talkingtree.org](https://talkingtree.org) | Nature and ecology content |
| [westdesertjournal.com](https://westdesertjournal.com) | Southwest regional news |

All three run on the Publishing Server behind nginx with managed TLS.

---

## Operational Philosophy

**Documentation before deployment.** Every service has a written purpose and recovery procedure before it goes live.

**GitHub as canonical truth.** All configuration decisions, architectural choices, and governance records are committed to version control. If it isn't in git, it doesn't exist.

**Recovery procedures written before systems go live.** The worst time to write a runbook is during an outage.

**Nothing important exists in only one place.** Memory, configuration, and state are replicated or versioned.

**Least Resistance Doctrine.** Complexity is a cost. The right solution is the simplest one that safely accomplishes the objective.

---

## Repository Structure

```
self-hosted-ai-infrastructure/
├── README.md               # This file — system overview
├── architecture/           # Diagrams and topology documentation
├── docs/                   # Operational runbooks and governance records
└── compose/                # Reference Docker Compose configurations (sanitized)
```

*Architecture diagrams, detailed runbooks, and compose configurations coming in subsequent commits.*

---

## Status

| Component | Status |
|-----------|--------|
| WireGuard mesh | Live |
| Agent Server | Live |
| Publishing Server | Live |
| Hub Server | Live |
| Qdrant | Live |
| n8n | Live |
| Ollama | Live |
| WordPress (×3) | Live |
| Ghost | Live |
| Postiz | Live |

---

## About

This infrastructure supports a portfolio of AI agent systems, publishing properties, and automation workflows operated independently without managed cloud services or a team.

The stack is self-hosted on dedicated Linux servers. All services run in Docker containers. The WireGuard mesh connects them into a single private network. GitHub is the system of record for all configuration, decisions, and documentation.

**Connect:** [LinkedIn](https://www.linkedin.com/in/nateaz/) · [GitHub](https://github.com/natearizona)
