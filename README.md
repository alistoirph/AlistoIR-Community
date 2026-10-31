# AlistoIR Community 3.0

**Wazuh-focused, AI-assisted security investigation for self-hosted SOC labs and small teams.**

This repository provides the installer download, documentation, and issue tracking. No application source tree is committed here; the installer contains the runtime files required by the application.

## Download the official release

[Download 3.0.3](https://github.com/alistoirph/AlistoIR-Community/releases/tag/v3.0.3)

Choose **`alistoir-community-v3.0.3-one-click.zip`** under Assets and download **`SHA256SUMS.txt`**. Do not use GitHub's automatic **Source code (zip)** archive: it contains this documentation repository, not the installer.

**Status: official Community 3.0.3 release.** Validate configuration and evidence collection in your environment before rollout. Validation evidence and remaining limits are recorded in [Release notes](docs/RELEASE_NOTES.md).

## What is new in 3.0.3

- Mandatory outbound privacy sanitization for OpenAI, Anthropic, DeepSeek and configured local AI endpoints.
- Recognized credentials are withheld; identities, email addresses, IP addresses and domains receive aliases. Identified raw logs and notes are withheld.
- Evidence references, technical JSON fields, rule IDs, MITRE techniques and common hashes remain available for investigation.
- Invalid or unsupported content blocks the AI request. Privacy gaps must remain visible in the model's assessment.
- Existing source protection, signed integrity checks and investigation tool restrictions remain enabled: 219 PHP files obfuscated and 469 immutable files signed.

See [AI privacy scope and limitations](docs/AI_PRIVACY.md). Sanitization is data minimization and cannot guarantee recognition of every sensitive detail in arbitrary text.

Read [release notes and validation limits](docs/RELEASE_NOTES.md) before installing. The ZIP is for a fresh installation; existing deployments require a backed-up upgrade procedure.

## Documentation

Open the [documentation folder](documentation/README.md) or read the [Investigation Center guide for analysts and clients](documentation/INVESTIGATION_CENTER.md).

## Start here

1. [Install on Ubuntu 24.04](docs/INSTALLATION.md).
2. [Configure AlistoIR, AI, and autonomous investigations](docs/CONFIGURATION.md).
3. [Connect Wazuh with the setup wizard](docs/WAZUH_SETUP.md).
4. [Run the tester acceptance checklist](docs/TESTING.md).
5. [Troubleshoot common problems](docs/TROUBLESHOOTING.md).

## Included capabilities

- Wazuh alert ingestion and supported evidence collection.
- Autonomous investigation queues, configurable Rule IDs, findings and evidence gaps.
- AI Copilot, timeline assistance and evidence-backed recommendations.
- Case management and AI-generated tasks, subject to analyst review.
- Playbooks with existing approval and safety controls.
- Local IOC feed library and external threat-intelligence integrations.
- Wazuh setup wizard with generated deployment files and a one-command installer.
- Signed integrity checks for official protected policy/enforcement files.

AI requires your own configured provider/infrastructure. Provider charges are not included. Collection support depends on the event, endpoint platform, available telemetry and permissions; unsupported or missing evidence can result in an inconclusive finding. AI recommendations are not guaranteed ground truth.

## Community limits

| Item | Limit |
|---|---:|
| Configured Wazuh managers | 1, including an environment-configured manager |
| Active accounts | 5, including administrators |
| Active playbooks | 5 |
| Active unique, unexpired saved IOC feed indicators | 3,000 |

Inactive accounts/playbooks and expired IOC indicators do not consume their respective active quotas. A disabled but still-configured Wazuh manager continues to occupy the manager slot. Alert, endpoint and investigation counts have no edition quota; practical capacity, API budgets and operational safeguards still apply.

No paid activation is required. Community is Wazuh-only: other SIEM ingestion/configuration and the System Updates UI are unavailable.

## Requirements

Ubuntu Server 24.04 LTS x86-64, sudo/root access, static IP or stable hostname, and Internet access for Ubuntu packages. Start with **2 vCPUs, 4 GB RAM and 40 GB disk** for a lab; increase resources and storage for your workload. Configure a reachable Wazuh Manager API and Indexer for evidence collection.

## Security and licensing

Read [SECURITY.md](SECURITY.md), [licensing status](LICENSE_STATUS.md), and [third-party notices](THIRD_PARTY_NOTICES.md). Public download availability does not itself make the application open source; an open-source license has not been selected for this release.

Report reproducible, sanitized bugs through [Issues](https://github.com/alistoirph/AlistoIR-Community/issues). Never attach passwords, tokens, `.env` files, deployment ZIPs containing ingest keys, or raw client/SOC evidence.
