# Self-Hosted AI Infrastructure

Self-hosted Linux infrastructure — multiple servers, a Mac workstation, and a portable field node — running containerized services, native ACP agent runtimes, automation workflows, vector memory, and live publishing properties, connected by a private WireGuard VPN mesh.

Built and operated by [Nathan Cowlishaw](https://www.linkedin.com/in/nateaz/) · Infrastructure started May 2026

---

## Current snapshot — September 2026

What changed since the June 2026 baseline documented below:

- **Agent layer.** Roughly twenty named agent seats across Claude Code, Codex CLI, Cursor, Gemini CLI, Grok CLI, Prime Agent, and self-hosted open-source runtimes, orchestrated from desktop and mobile over the Agent Client Protocol (ACP) via Paseo. Most service workloads run under Docker Compose; ACP agent runtimes stay native where upstream requires.
- **Isolation rule.** One seat, one git worktree, one credential boundary — adopted after a vendor IDE auto-committed another seat's files into a shared checkout.
- **Automation.** n8n retired (July 2026) in favor of Windmill: dual-run period, then per-caller scoped-token authentication before the legacy path was closed.
- **Continuity.** The repository is canonical memory. A wake/close protocol lets any agent, on any runtime, cold-start from repository state; 100+ ratified operating decisions live in a version-controlled ledger.
- **Recovery.** Designed around rebuilding from documented bootstrap procedures, preserved state, and fresh authentication ceremonies rather than relying on disk images. A portable field node joined the fleet in September.
- **Edge.** Cloudflare Tunnels and Access (JWT service auth) in front of the reverse proxies.

*This public repository is a sanitized overview. Hostnames, addresses, credentials, seat rosters, and internal governance records live in the private operating repository and are not published here.*

---

## Architecture Overview (June 2026 baseline)

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
| Automation | Windmill (n8n retired July 2026) | Workflow orchestration, webhooks, scoped-token auth |
| Publishing | WordPress (×3) | Live content properties |
| Publishing | Ghost | Long-form publishing |
| Publishing | Postiz | Social scheduling |
| Version Control | GitHub | Canonical system truth |

---

## Live Properties

| Site | Role |
|------|------|
| [turquoiseufo.org](https://turquoiseufo.org) | Home of the organism — Nathan Arizona LLC / Turquoise UFO |
| [turquoiseufo.net](https://turquoiseufo.net) | Turquoise UFO private desert tours (business site) |
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
| Windmill | Live |
| n8n | Retired (July 2026) |
| ACP agent runtimes (Paseo) | Live |
| Ollama | Live |
| WordPress (×3) | Live |
| Ghost | Live |
| Postiz | Live |

---

## About

This infrastructure supports a portfolio of AI agent systems, publishing properties, and automation workflows operated independently without managed cloud services or a team.

The stack is self-hosted on dedicated Linux servers plus a Mac workstation and a portable field node. Most service workloads run in Docker containers; ACP agent runtimes stay native where upstream requires. The WireGuard mesh connects them into a single private network. GitHub is the system of record for all configuration, decisions, and documentation.

**Connect:** [LinkedIn](https://www.linkedin.com/in/nateaz/) · [GitHub](https://github.com/natearizona)
