# n8analyst

Planning and proof-of-concept assets for an evidence-driven, locally hosted network and host forensics orchestrator built around n8n.

The POC uses:

- distributed Security Onion sensors for network visibility and full packet capture;
- n8n as the control plane, not the evidence-processing engine;
- isolated workers for PCAP and disk-image processing;
- local OpenAI-compatible model endpoints;
- immutable object storage for evidence and derivatives; and
- Jira for human review and investigation state.

## Repository layout

| Path | Purpose |
|---|---|
| `deploy/` | POC control-plane deployment files |
| `docs/` | Architecture, trust boundaries, and rollout plan |
| `jira/` | Jira issue and field mapping |
| `prompts/` | Versioned role prompts for analysis agents |
| `schemas/` | JSON schemas shared by workflows, workers, and agents |
| `workers/` | Analysis-worker API contract |
| `workflows/` | Importable n8n workflow skeletons |

## Start here

1. Read [`docs/architecture.md`](docs/architecture.md).
2. Copy `deploy/.env.example` to `deploy/.env` and replace every placeholder.
3. Start the control plane with `docker compose --env-file deploy/.env -f deploy/docker-compose.yml up -d`.
4. Implement an analysis service conforming to `workers/openapi.yaml`.
5. Import the JSON files in `workflows/` into n8n.
6. Configure n8n credentials and replace the Jira project placeholders.

## Safety model

- Raw evidence never enters an LLM context.
- n8n passes object references and normalized JSON, not disk images or large PCAPs.
- Workers mount evidence read-only and have no general internet egress.
- Models cannot choose arbitrary shell commands.
- Every conclusion must cite a deterministic artifact locator.
- Containment and final disposition remain human-approved during the POC.

This repository contains a scaffold, not a production-ready incident response system.

