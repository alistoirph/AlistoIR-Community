# AlistoIR Community documentation

Practical guides for analysts, clients and administrators using AlistoIR Community 3.0.

Start with [Installation](../docs/INSTALLATION.md) for a new deployment, or [Investigation Center](INVESTIGATION_CENTER.md) for daily investigation work. Each detailed guide includes section links where applicable.

<!-- toc:start -->

## Table of contents

- [Installation and configuration](#installation-and-configuration)
  - [Installation](../docs/INSTALLATION.md)
  - [Configuration](../docs/CONFIGURATION.md)
  - [Integrations](INTEGRATIONS.md)
  - [Integration Configuration](INTEGRATION_CONFIGURATION.md)
  - [Wazuh Setup Wizard](WAZUH_SETUP_WIZARD.md)
    - [Wizard screenshot](WAZUH_SETUP_WIZARD.md#wizard-screenshot)
  - [Wazuh setup](../docs/WAZUH_SETUP.md)
  - [Acceptance checks](../docs/TESTING.md)
- [Alerts and investigations](#alerts-and-investigations)
  - [Dashboard](DASHBOARD.md)
  - [Alerts](ALERTS.md)
  - [Alert View](ALERT_VIEW.md)
  - [Investigate Logs](INVESTIGATE_LOGS.md)
  - [Wazuh Evidence and AI Investigation](WAZUH_AI_INVESTIGATION.md)
  - [Investigation Center](INVESTIGATION_CENTER.md)
  - [Autonomous Investigation Settings](INVESTIGATION_SETTINGS.md)
- [Assets, collaboration and automation](#assets-collaboration-and-automation)
  - [Assets / CMDB](CMDB.md)
  - [Analyst Chat](CHAT.md)
  - [Direct Messages](DIRECT_MESSAGES.md)
  - [Playbooks](PLAYBOOKS.md)
- [Hunting, intelligence and reporting](#hunting-intelligence-and-reporting)
  - [Threat Intelligence Hub / IOC Manager](THREAT_INTELLIGENCE.md)
  - [Threat Hunting](THREAT_HUNTING.md)
  - [Advanced Hunt / Multi-Filter](ADVANCED_HUNT.md)
  - [SLA Monitoring](SLA.md)
  - [Reports / Report Center](REPORTS.md)
- [Administration, privacy and support](#administration-privacy-and-support)
  - [User Management](USERS.md)
  - [AI privacy](../docs/AI_PRIVACY.md)
  - [Code integrity and AI guardrails](../docs/CODE_GUARDRAILS.md)
  - [Source protection](../docs/SOURCE_PROTECTION.md)
  - [Troubleshooting](../docs/TROUBLESHOOTING.md)
  - [Release notes](../docs/RELEASE_NOTES.md)

<!-- toc:end -->

## Installation and configuration

| Guide | Who it is for | What you will learn |
|---|---|---|
| [Installation](../docs/INSTALLATION.md) | Administrators | Download, verify and install the official Community package on Ubuntu. |
| [Configuration](../docs/CONFIGURATION.md) | Administrators | Configure accounts, integrations and automatic investigation scope. |
| [Integrations](INTEGRATIONS.md) | System administrators | Configure Wazuh, AI, intelligence and notifications; interpret status, sources, tests and Community restrictions. |
| [Integration Configuration](INTEGRATION_CONFIGURATION.md) | System administrators | Configure every integration field, mode, saved test, ingest policy, AI provider, intelligence service and SMTP setting. |
| [Wazuh Setup Wizard](WAZUH_SETUP_WIZARD.md) | System administrators | Follow wizard steps, configure CAs, generate/install the private forwarder bundle, verify readiness and revoke keys. |
| [Wazuh setup](../docs/WAZUH_SETUP.md) | Administrators | Connect the Manager API, Indexer and alert forwarder. |
| [Acceptance checks](../docs/TESTING.md) | Deployment owners | Check that a deployment works with controlled lab evidence. |

## Alerts and investigations

| Guide | Who it is for | What you will learn |
|---|---|---|
| [Dashboard](DASHBOARD.md) | Analysts, clients and administrators | Understand every dashboard section, metric, chart, drill-down and interpretation limit. |
| [Alerts](ALERTS.md) | Analysts and clients | Search and triage alerts, interpret badges, review ownership/SLA, and use permitted actions and exports. |
| [Alert View](ALERT_VIEW.md) | Analysts and clients | Understand evidence, AI tools, SLA, endpoint collection, case history and response workflows. |
| [Investigate Logs](INVESTIGATE_LOGS.md) | Analysts and clients | Correlate ingested alerts by endpoint, IP or rule, understand symmetric time windows, and pivot to Wazuh Dashboard. |
| [Wazuh Evidence and AI Investigation](WAZUH_AI_INVESTIGATION.md) | Analysts and clients | Collect bounded Indexer evidence, inspect query audits and interpret AI reports and limitations. |
| [Investigation Center](INVESTIGATION_CENTER.md) | Analysts and clients | Understand the workspace, follow evidence collection, interpret recommendations and record an analyst decision. |
| [Autonomous Investigation Settings](INVESTIGATION_SETTINGS.md) | System Administrators | Enable or pause automatic work, select Rule IDs and understand scope changes. |

## Assets, collaboration and automation

| Guide | Who it is for | What you will learn |
|---|---|---|
| [Assets / CMDB](CMDB.md) | Analysts and administrators | Manage asset identity/business context, understand matching and colors, and import inventory safely. |
| [Analyst Chat](CHAT.md) | Analysts and team managers | Use channels, replies, mentions, attachments, search and channel controls. |
| [Direct Messages](DIRECT_MESSAGES.md) | Analysts and clients with access | Use one-to-one chat, understand recipient eligibility, unread/seen signals and message sharing. |
| [Playbooks](PLAYBOOKS.md) | Analysts, managers and administrators | Understand triggers, action steps, approvals, execution history and the five-active-playbook Community limit. |

## Hunting, intelligence and reporting

| Guide | Who it is for | What you will learn |
|---|---|---|
| [Threat Intelligence Hub / IOC Manager](THREAT_INTELLIGENCE.md) | Managers and analysts with access | Understand local intelligence, trust exceptions, feeds, matching, refresh, export and the 3,000-active-IOC limit. |
| [Threat Hunting](THREAT_HUNTING.md) | Analysts and managers | Search local alerts and linked observables, understand regex limits, interpret results and manage hunt completion/exports. |
| [Advanced Hunt / Multi-Filter](ADVANCED_HUNT.md) | Analysts and managers | Build nested filters, use quick/manual queries and AI helpers, save searches and interpret bounded results. |
| [SLA Monitoring](SLA.md) | Analysts and managers | Monitor alert/case deadlines, understand timers, filters, compliance formulas, reporting and coverage limits. |
| [Reports / Report Center](REPORTS.md) | Managers and administrators | Generate case/posture reports, understand KPI date bases, analyst audit, SLA, downloads and reporting limits. |

## Administration, privacy and support

| Guide | Who it is for | What you will learn |
|---|---|---|
| [User Management](USERS.md) | System administrators | Create/edit accounts, understand roles, group scope, five-active-account capacity, passwords, 2FA and deletion. |
| [AI privacy](../docs/AI_PRIVACY.md) | Analysts, clients and administrators | Understand outbound sanitization, aliases, withheld evidence and limitations. |
| [Code integrity and AI guardrails](../docs/CODE_GUARDRAILS.md) | Administrators | Understand signed-code enforcement and supported maintenance boundaries. |
| [Source protection](../docs/SOURCE_PROTECTION.md) | Administrators and deployment owners | Understand protected PHP, deliberate exceptions and protection limits. |
| [Troubleshooting](../docs/TROUBLESHOOTING.md) | Analysts and administrators | Diagnose collection, connectivity, provider and integrity problems. |
| [Release notes](../docs/RELEASE_NOTES.md) | All users | Review changes and validation scope. |

Examples in these guides are illustrative. Never publish real client evidence, credentials or private configuration in GitHub issues.
