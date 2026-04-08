# Collective Mind
Connect observations from your isolated agents to find the patterns hidden between them.

**Live Instance:** [https://collective-mind.casey-digennaro.workers.dev](https://collective-mind.casey-digennaro.workers.dev) 🛸

## Why This Exists
When you run multiple agents, each only sees its own logs. This tool cross-references those separate streams, looking for low-signal correlations—like a brief latency spike for one agent coinciding with a specific user query for another—that would otherwise go unnoticed.

## Quick Start
This is a fork-first project. You run your own private instance.

1.  **Fork** this repository.
2.  Deploy to Cloudflare Workers. You only need to create one empty KV namespace and bind it as `MIND_KV`.
3.  Set your `DEEPSEEK_API_KEY` as a Worker secret.
4.  Configure your agents to POST observation fragments to your instance's `/ingest` endpoint.

## What Makes This Different
1.  **Does one job**: It performs cross-agent correlation. It is not a logging dashboard or alerting system.
2.  **Zero dependencies**: The Worker has no npm packages, build steps, or external runtime dependencies.
3.  **Filters noise**: It only surfaces insights built from multiple, separate fragments. Single events are ignored.

## Features
- **Plaintext Ingestion**: Send fragments with a `vessel_id`, `tags`, and a `timestamp`.
- **Scheduled Discovery**: A native Worker Cron trigger (e.g., hourly) runs the correlation process.
- **Novelty Scoring**: Connections are scored from 1-10; the dashboard highlights scores above 7.
- **Self-contained Dashboard**: A minimal, static HTML interface for searching fragments and viewing insights. No external assets.
- **Swappable LLM**: The default DeepSeek integration can be replaced by modifying a single function.
- **Private by Design**: All data stays in your KV namespace. No analytics or third-party calls.

## Architecture
Agents POST fragments to the Worker, which stores them in Cloudflare KV. On a scheduled interval, the Worker reads recent fragments, uses an LLM to propose connections, and persists high-scoring insights back to KV. There are no queues, databases, or external services beyond the LLM API.

## Limitations
The correlation run is designed for moderate volume. Each scheduled execution processes the most recent 100 fragments by default. It is not intended for high-frequency, real-time event streaming.

## Credits
Superinstance and Lucineer (DiGennaro et al.)

<div style="text-align:center;padding:16px;color:#64748b;font-size:.8rem"><a href="https://the-fleet.casey-digennaro.workers.dev" style="color:#64748b">The Fleet</a> &middot; <a href="https://cocapn.ai" style="color:#64748b">Cocapn</a></div>

---

<i>Built with [Cocapn](https://github.com/Lucineer/cocapn-ai) — the open-source agent runtime.</i>
<i>Part of the [Lucineer fleet](https://github.com/Lucineer)</i>

