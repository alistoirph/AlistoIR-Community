# Integrations — configuration and status guide

Page: `/integrations/index`. Applies to AlistoIR Community 3.0.4. Access requires a System Administrator/platform-administrator context. Configuration pages and linked workflows enforce their own access checks.

The **Core Integrations** page summarizes Wazuh ingestion, the selected AI provider, threat-intelligence services and notification channels. Use **Configure Integrations** to open `/integrations/configuration`, where settings are saved. This overview is not a live service-health dashboard.

<!-- toc:start -->

## Table of contents

- [1. How to use it](#1-how-to-use-it)
- [2. Read an integration card](#2-read-an-integration-card)
- [3. Configuration modes and secrets](#3-configuration-modes-and-secrets)
- [4. Wazuh Native Integration](#4-wazuh-native-integration)
- [5. AI providers and routing](#5-ai-providers-and-routing)
- [6. Threat-intelligence providers](#6-threat-intelligence-providers)
- [7. Notification channels and tests](#7-notification-channels-and-tests)
- [8. Email SMTP](#8-email-smtp)
- [9. Community restrictions](#9-community-restrictions)
- [10. Verify an end-to-end workflow](#10-verify-an-end-to-end-workflow)
- [11. Troubleshooting](#11-troubleshooting)

<!-- toc:end -->

## 1. How to use it

1. Open **Integrations** as an authorized administrator.
2. Review the integration cards and their **Source** labels.
3. Select **Configure Integrations**.
4. Choose the relevant configuration mode, enter your deployment's values and save the relevant section.
5. Return to the overview and confirm which provider/channel is effectively configured.
6. Use a supported connection test or a small authorized workflow to verify actual behavior. Review returned errors and the destination's result.

For a new deployment, configure Wazuh forwarding and evidence access first, then the AI provider, then the intelligence/notification services you need. A green configuration badge alone is insufficient to conclude that autonomous investigation has all required evidence.

## 2. Read an integration card

| Item | Meaning |
|---|---|
| Name | Integration/service being described. |
| Category | Its purpose, such as SIEM / XDR, AI Provider, Threat Intel or Notification. |
| Configured — green | Current configuration checks found the required settings or selected/enabled state for that card. It is not proof of a successful live API call. |
| Missing — gray | Required settings are absent, disabled, unusable, or the AI provider is not the selected provider. Read the explanatory text. |
| Details | Model, endpoint, integration mode, channel or supported lookup purpose. |
| Explanatory text | Additional setup requirements or why the integration is unavailable. |
| Source | Where the overview determined its settings/status. |
| Action | Configure/navigation link or a supported notification test, where provided. |

An unselected AI provider can display Missing even when another provider is correctly active. The page does not require every card to be green. It also does not query every external service when you open it.

The configuration detector rejects some obvious placeholder secrets, but a non-placeholder key can still be revoked, expired, unauthorized or over quota. Check actual workflow results before declaring the integration operational.

## 3. Configuration modes and secrets

| Mode/source | Interpretation |
|---|---|
| Configured in UI | Use settings saved through Integration Configuration for the applicable organization context. The overview commonly labels this Integration Settings. |
| Use .env fallback | Use server-provided configuration/defaults. The overview can label this Config. UI values may not be effective while this mode is selected. |
| Disabled | Disable that integration through its effective setting. Stored credentials can still exist. |
| Enabled by .env/default | Used by some default/free intelligence providers instead of requiring a UI API key. |
| Inferred | A value was constructed from application configuration, such as the ingest endpoint; it was not discovered by a connection test. |
| Detected | Generic fallback source label; not a health-check result. |

For secret fields that say **Leave blank to keep current secret**, blank preserves the stored value. It does not erase the credential. Use Disabled to stop a supported integration; replace the secret and revoke the old credential at its issuing service when rotating it. Masked values indicate a saved secret without displaying it in full.

Use the save button for the relevant section: **Save Wazuh Policy**, **Save AI Provider**, **Save Notifications**, **Save Threat Intel** or **Save SMTP**. Test actions use saved effective configuration; save before testing. Saving one section is not a global connection test.

The overview resolves an organization context from the current account, or the first configured organization where necessary. Do not assume that one view independently tests every organization or every user's data access.

## 4. Wazuh Native Integration

The card displays the configured integration mode, manager label and ingest endpoint. The endpoint can be explicitly configured or inferred from the application's base URL as `/alerts/ingest.php`.

Its status checks ingestion-related configuration. It does **not** prove that a manager record exists, that its Manager API/Indexer is reachable, that a forwarder is running, that TLS trust is correct or that recent alerts arrived.

Follow the [Wazuh Setup guide](../docs/WAZUH_SETUP.md) to:

1. Configure the saved Wazuh Manager API and Indexer connection through Wazuh Managers.
2. Test API and Indexer authentication/access separately.
3. Generate the deployment files using the setup wizard.
4. Install the forwarding service on the Wazuh manager and verify actual received alerts.

**Wazuh Ingestion Policy** in Integration Configuration manages trusted sender IPs/CIDRs, rate-limit settings and notification severity/event-age controls. It is separate from Manager API/Indexer credentials. Match trusted addresses to the sender address actually observed by AlistoIR, accounting for your network/proxy setup.

Blank Trusted IPs can fall back to the environment policy; do not assume blank means either deny-all or unrestricted access. Inspect the effective policy before enabling forwarding. Retain normal TLS verification and use the supported CA configuration for private certificates.

The Wazuh deployment ZIP contains an ingest credential. Keep it private; it is different from the publicly downloadable AlistoIR server installer ZIP.

## 5. AI providers and routing

| Card | Purpose / setup |
|---|---|
| OpenAI | Select the provider and configure its API key and model. |
| Claude / Anthropic | Select Anthropic and configure its API key and compatible model. |
| Ollama / Local AI | Select Local, configure the model and an endpoint reachable from the AlistoIR server. |
| DeepSeek | Select DeepSeek and configure its API key and model. |
| AI Provider Routing | Shows the effective selected provider/model and configuration source for shared AI workflows. |

Configure the provider under **AI Provider**, select its mode, enter the appropriate model/key or local URL, review maximum tokens/temperature and select **Save AI Provider**. A displayed default model is not confirmation that your provider account supports it. Use the model available to your own deployment.

The selected provider must be effectively enabled and configured for its card to become Configured. Other provider cards can remain Missing. The overview does not imply parallel calls to every listed model, automatic failover or a successful model inference.

For Local AI, `localhost` refers to the **AlistoIR server**, not the administrator's browser, Windows workstation or another VM. A valid saved URL can still point to an unavailable listener, missing model or incompatible API.

Verify AI through a small authorized workflow in [Alert View](ALERT_VIEW.md) or the [Investigation Center](INVESTIGATION_CENTER.md). Generating that result can send sanitized SOC context to the selected provider and can use its API quota. Check both generation success and the supporting evidence; model availability is separate from investigation completeness.

The shared privacy boundary sanitizes supported outbound AI context, including requests to configured Local endpoints. It cannot guarantee recognition of arbitrary sensitive prose. A service labeled Local can still have a URL outside your network. See [AI privacy](../docs/AI_PRIVACY.md).

## 6. Threat-intelligence providers

| Card | Application's supported purpose |
|---|---|
| VirusTotal | IP, domain, hash and URL enrichment. |
| AbuseIPDB | IP reputation/abuse context. |
| MISP | Supported lookup and IOC-import workflow; API URL and key are required. |
| OpenCTI | GraphQL indicator import into local IOC feeds; API URL and token are required. |
| AlienVault OTX | Pulse context for supported observables, including IP/domain/hash/URL/email. |
| GreyNoise | Internet scanning/noise context for public IPs. |
| Shodan | Public-IP exposed-service, host, ASN and available vulnerability context. |
| URLScan.io | Supported URL/domain intelligence lookups. |
| CISA KEV | Known-exploited-vulnerability catalog context for CVEs. |
| EPSS | Exploit-probability context for CVEs. |
| CIRCL hashlookup | Best-effort known-file hash context. |
| ThreatFox | Supported IOC reputation/malware-family context; the application expects an abuse.ch Auth-Key. |
| URLhaus | Supported malicious-URL, host and payload context. |
| MalwareBazaar | Supported malware-hash lookup context. |

These describe AlistoIR's integration paths, not a guarantee that every observable or API feature is available under your provider account. Quotas, supported indicator types, network reachability and provider responses affect actual results. Some default/public services do not require a key in this form; others do.

Save the relevant modes, credentials and URLs under **Threat Intelligence Providers**, then use supported enrichment/import workflows to verify the saved settings. The overview has no dedicated live test button for every intelligence card.

Configuring MISP/OpenCTI credentials alone does not create a scheduled IOC source. Add the appropriate MISP Import/OpenCTI Import source in the IOC workspace and verify synchronization separately. A green intelligence card does not prove that imports ran or that matching is current.

See [Threat Intelligence Hub](THREAT_INTELLIGENCE.md) for feeds, trust exceptions, enrichment, confidence interpretation and Community's **3,000 active unique saved IOC** limit. An external reputation result remains supporting evidence, not a final malicious/benign verdict.

Intelligence lookups can disclose the queried observable to that service. **AI outbound sanitization does not cover these separate intelligence API requests**, and it does not sanitize exported reports or notification messages automatically.

## 7. Notification channels and tests

| Channel | Required configuration | Overview action |
|---|---|---|
| Slack | Mode and valid webhook; optional/displayed channel context. | Send Test Notification |
| Telegram | Mode, bot token and chat ID. | Send Test Notification |
| Microsoft Teams | Mode and supported webhook URL. | Send Test Notification |
| PagerDuty | Mode and routing key. | Send Test Event |

1. Open Configure Integrations and complete the channel's settings.
2. Select **Save Notifications**.
3. Confirm the intended channel/destination and use its test button.
4. Check the success/error message and verify receipt in the destination system.

The buttons are disabled when the current configuration check is incomplete. Tests send **real outbound messages/events**, not a preview. PagerDuty sends a trigger event that can create or update an incident according to the destination service; review/resolve the test there as appropriate. Repeated tests can be grouped by the application's daily test deduplication key.

A successful transport result does not prove a human read or acknowledged it, or that every future alert will notify. Actual workflows still depend on trigger/event rules, severity/age controls, recipient settings and provider delivery. Tests do not change an alert's SLA acknowledgement, close a case or enable autonomous collection.

If delivery fails, check saved mode, webhook/key, recipient/chat access, outbound connectivity and administrator-accessible error logs. A webhook is a credential; do not paste it into public GitHub issues or screenshots.

## 8. Email SMTP

Email SMTP is configured on **Integration Configuration**, rather than a separate card on the overview. Set mode, host, port, encryption, username/password and sender identity according to your mail server, then select **Save SMTP**.

Use the recipient field and saved-configuration SMTP test to verify outbound mail. This sends a real email. Saving SMTP does not import mailboxes or automatically turn every SOC action into an email notification.

Account-creation mail, reset mail and supported notifications depend on their own workflow and delivery outcomes. A successful send result does not guarantee inbox placement or receipt. For account behavior, see [User Management](USERS.md).

## 9. Community restrictions

Community is **Wazuh-only for SIEM ingestion**. The Sumo Logic/Manage SIEM Sources card is removed from this overview, and other SIEM ingestion/configuration is unavailable. AI, supported threat-intelligence integrations and notification services are separate from that restriction.

Community supports **one configured Wazuh manager**, including an environment-configured connection. Edit the existing connection rather than adding another. A disabled but still-configured manager can continue to occupy that slot.

Provider accounts, network access, rate limits and any external charges remain your responsibility. Configuring an integration does not increase Community's account, playbook or saved-IOC capacity.

## 10. Verify an end-to-end workflow

Use an authorized benign test event and verify each relevant stage:

1. **Forwarding:** the manager sends the event to the correct ingest endpoint using the expected credential and permitted sender address.
2. **Ingestion:** the event becomes a new alert with the expected source, host and timestamps.
3. **Evidence:** Manager API/Indexer collection works for the intended scope; an ingestion success does not imply raw evidence availability.
4. **AI:** the effective provider can return an assessment and explain evidence gaps.
5. **Notification:** the configured rule/workflow delivers the expected message to the intended destination.
6. **Analyst workflow:** ownership, investigation review, disposition and case handling are recorded where required.

Review [Autonomous Investigation Settings](INVESTIGATION_SETTINGS.md) separately to enable automatic work and select rule scope. Integration configuration alone does not enable every worker or guarantee evidence completeness.

## 11. Troubleshooting

| Symptom | What to check |
|---|---|
| Configured but API call fails | Badge reflects configuration, not a live health check; inspect credentials, permissions, endpoint, quota, DNS and TLS. |
| Several AI cards say Missing | Only the effective selected provider is expected to be active; read AI Provider Routing. |
| Saved key/model seems ignored | Mode may still use .env fallback; inspect the effective Source and organization context. |
| Clearing a secret did not remove it | Blank preserves secrets in fields labeled that way; disable or rotate using the supported workflow. |
| Wazuh Configured but no alerts | Check trusted sender policy, ingest credential, forwarder service, endpoint reachability and actual event forwarding. |
| Alerts arrive but investigation has no evidence | Test Manager API/Indexer separately; inspect archive/index coverage, permissions and investigation findings. |
| Cannot add another Wazuh manager | Community's one-configured-manager restriction; edit the existing connection. |
| Local AI works on the workstation only | The endpoint must be reachable from AlistoIR; localhost is the server hosting the application. |
| MISP/OpenCTI configured but no imports | Add/enable the supported IOC source and verify its scheduler/history and destination checks. |
| Notification test disabled | Save complete, effectively enabled channel settings first. |
| Test succeeds but no operational notifications | Verify workflow triggers, notification policy and delivery/recipient rules separately. |
| SMTP missing from the card grid | Open Configure Integrations and use Email SMTP. |
| Private intelligence URL rejected | Use supported destination-validation configuration; do not disable URL/TLS protections globally. |
| Security validation failed | Refresh the signed-in form and retry; keep CSRF/integrity controls enabled. |
| Access denied | System/platform-administrator authorization is required for this overview. |

Related guides: [Configuration](../docs/CONFIGURATION.md), [Wazuh Setup](../docs/WAZUH_SETUP.md), [Threat Intelligence](THREAT_INTELLIGENCE.md), [AI privacy](../docs/AI_PRIVACY.md) and [Troubleshooting](../docs/TROUBLESHOOTING.md).
