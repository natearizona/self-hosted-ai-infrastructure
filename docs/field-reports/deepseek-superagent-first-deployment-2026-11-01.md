# Field report: DeepSeek dsh-superagent in a private multi-agent deployment

**Author:** Turquoise UFO (Arizona, USA)  
**Date:** 2026-11-01  
**Status:** Community deployment notes, not an official DeepSeek integration or an upstream bug confirmation  
**Upstream:** [deepseek-ai/dsh-superagent-prototype](https://github.com/deepseek-ai/dsh-superagent-prototype), baseline commit [6337fc8](https://github.com/deepseek-ai/dsh-superagent-prototype/commit/6337fc823ce20251c8de8085948165c81aaa701e)

## Why we tried it

Turquoise operates a private collection of specialized AI coding and operations agents. The recurring bottleneck is not only inference or model quality. It is human attention spent locating the right agent, delivering tasks, waiting for acknowledgments, and reconciling results.

We commissioned a single **dsh-superagent** prototype named **Mack** as an isolated experiment. Our immediate goal was deliberately modest: prove a real GitHub assignment loop, honest failure behavior, and a private mobile communication channel before considering cross-agent orchestration.

The prototype's stated focus on reducing human attention is directly relevant to our use case. We are evaluating that philosophy in practice, not claiming that the current release is a production-ready orchestration framework.

## Environment and scope

- Self-hosted Linux runtime on a private network.
- One disposable private GitHub sandbox repository and a dedicated GitHub App identity.
- A separately restricted Telegram interface for notifications and private conversation.
- No public management panel; no privileged infrastructure access from Telegram.
- A separate human-led review path before broadening tools, repository scope, or authorization.

We do **not** publish our network topology, private repository, service credentials, chat identities, resident registries, or internal operational logs here.

## Observation 1: the GitHub watcher was blocked by partial API permissions

The GitHub App could read issues and pull requests, and the agent successfully opened a narrowly scoped pull request. However, the watcher repeatedly encountered `HTTP 403: Resource not accessible by integration` when reading pull-request check runs or commit statuses. Those failed reads impaired watcher health even though the GitHub assignment path itself worked.

In our adapted runtime we separated the permission-limited CI observation from the underlying PR/task poll. A missing CI read must **not** become either `passed` or `not configured`. The watcher should continue servicing independent authorized work, while exposing the CI read failure clearly.

This is an observation from our particular installation and extension work. We have not yet prepared a public, independently reproducible upstream regression against an unmodified checkout. We would welcome maintainer guidance on the intended checks/statuses permission requirements and failure semantics.

## Observation 2: distinguish CI evidence from the absence of evidence

Our acceptance tests explicitly modeled these outcomes:

| Reported outcome | Required evidence |
| --- | --- |
| Passed | A successful, attributable CI observation |
| Failed | A check or status actually reporting failure |
| Pending | A check or status actually still running |
| Unavailable | A failed or unauthorized observation, with reason |
| Not configured | Independently established absence of applicable CI configuration |

In particular, `HTTP 403`, network failure, and timeout should never masquerade as a successful build or as proof that no CI exists. We tested fault cases within our sandbox and retained private build and test evidence for independent review. We are preparing a smaller public reproduction before making a code-level upstream claim.

## Observation 3: a mobile conversation should not become a privileged execution channel

The initial GitHub worker and the later Telegram integration serve different purposes. We tested outbound GitHub/CI alerts to a private Telegram chat, then a restricted two-way conversational surface. Telegram-triggered repository modifications, shell commands, and new assignments remain outside the conversational channel's authority.

Our practical lesson: enforce channel permissions in the runtime and tool binding, not merely through an agent's prompt. Also test unauthorized senders and ensure a messaging outage cannot halt an unrelated GitHub worker.

**The Telegram bridge is a Turquoise extension, not an upstream feature or an official DeepSeek product.** We are not presenting it as an installable plugin yet.

## Observation 4: delegation still needs acknowledgment and recovery

A task can have a durable record while its intended operator never receives an actionable message. In a distributed agent environment, the missing layer is often: recipient discovery, authenticated handoff, acknowledgment, timeout, retry without duplicate execution, and reconciliation of the final artifact.

We see promise in the dsh-superagent's ticket/worker design, but have not commissioned it as an organization-wide dispatcher. That role would need narrow per-resident adapters, principal verification, idempotency, and explicit human authority boundaries.

## What we can share next

We are interested in collaborating on a **small, sanitized, reproducible** test or extension rather than dropping a large private-infrastructure patch into an upstream thread.

Questions for the DeepSeek Harness maintainers and community:

1. What is the expected behavior when a GitHub App can read PRs but not checks/statuses? Should CI observation degrade independently of the assignment watcher?
2. Is there an intended distinction between `CI unavailable` and verifiably `no CI configured` in the prototype?
3. Is a general Source/notification adapter, separate from the worker's execution rights, a useful community plugin boundary?
4. Which lifecycle guarantees should an external dispatcher rely on for acknowledgment, retry, and task de-duplication?

We welcome corrections and pointers to existing work. We will keep the prototype's upstream and our extension clearly separate.

---

**Project:** [Turquoise UFO](https://turquoiseufo.org)  
**Public infrastructure overview:** [natearizona/self-hosted-ai-infrastructure](https://github.com/natearizona/self-hosted-ai-infrastructure)  
**Upstream community:** [DeepSeek Harness Discussions](https://github.com/deepseek-ai/deepseek-harness/discussions)  
**Disclosure note:** This document contains sanitized deployment observations only; private infrastructure evidence has not been published.
