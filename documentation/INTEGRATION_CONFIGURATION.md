# Integration Configuration — administrator guide

Page: `/integrations/configuration`. Applies to AlistoIR Community 3.0.4. Open it from **Integrations → Configure Integrations** using an authorized System Administrator/platform-administrator account.

This page saves Wazuh ingestion policy, AI routing, notification channels, intelligence-provider settings and outbound SMTP. For the card overview and status interpretation, see [Integrations](INTEGRATIONS.md).

<!-- toc:start -->

## Table of contents

- [1. How to configure a service](#1-how-to-configure-a-service)
- [2. Modes, badges and secret handling](#2-modes-badges-and-secret-handling)
- [3. Wazuh Ingestion Policy](#3-wazuh-ingestion-policy)
- [4. AI Provider](#4-ai-provider)
- [5. Notification Channels](#5-notification-channels)
- [6. Threat Intelligence Providers](#6-threat-intelligence-providers)
- [7. Email SMTP](#7-email-smtp)
- [8. Save and test actions](#8-save-and-test-actions)
- [9. Community scope and related settings](#9-community-scope-and-related-settings)
- [10. Troubleshooting](#10-troubleshooting)

<!-- toc:end -->

## 1. How to configure a service

1. Open **Integration Configuration** and identify the section you intend to change.
2. Select the appropriate **Mode**. To use values entered here, choose **Configured in UI** where offered.
3. Enter the service's settings. Preserve masked secrets by leaving their replacement fields blank when instructed.
4. Select that section's save button and read the returned result.
5. Reopen/review the section and the Integrations overview to confirm the effective configuration.
6. Use its saved-configuration test, or an authorized enrichment/AI/Wazuh workflow where no test button exists.

Save one section at a time. A save/test reload redirects back to the page, so unsaved edits elsewhere can be lost. A green badge confirms configuration checks, not a successful end-to-end connection or complete investigation coverage.

The page operates on the current organization context, falling back to the first configured organization where necessary. It is not a bulk editor for every organization or individual user's integrations.

## 2. Modes, badges and secret handling

| Mode | Effect |
|---|---|
| Configured in UI | Use supported values saved through this page for the applicable organization. |
| Use .env fallback | Use the server environment/default configuration; entering a UI value does not necessarily make it effective in this mode. |
| Disabled | Disable the integration through its effective configuration. Existing credentials can remain stored. |
| Enabled by .env/default | Used by certain default/public intelligence providers instead of a UI credential mode. |

| Badge | Interpretation |
|---|---|
| Configured — green | Effective settings appear enabled and sufficiently complete. It does not verify connectivity or provider authorization. |
| Disabled — gray | Effective mode is disabled. |
| Using .env — cyan/info | Global/environment configuration is the indicated source where this badge is shown. |
| Needs setup — yellow | Required usable configuration was not established. |

Fields labeled **Leave blank to keep current secret** retain their previous saved secret. A blank password/key field is not evidence that no credential exists, and it is not a delete operation. Masked Current values are display hints, not values to copy into the replacement field.

To rotate a key, enter the replacement, save, verify the intended service, and revoke the old key at its issuing service. To stop an integration, use Disabled where supported. If secret-storage readiness/encryption configuration is unavailable, saving new secrets can be rejected; ask the administrator to resolve storage configuration rather than weakening protection.

## 3. Wazuh Ingestion Policy

This section controls supported ingest admission/rate policy and notification gating. It does not replace Wazuh Manager API or Indexer configuration.

| Field | Purpose / how to set it |
|---|---|
| Trusted Wazuh IPs or CIDRs | Permitted sender addresses, one IP/network per line. Use the source AlistoIR actually sees, including legitimate NAT/proxy effects. |
| Per-IP rate limit | Request limit for an individual source IP within the configured window. |
| Per-key rate limit | Request limit associated with an ingest credential within the window. |
| Global rate limit | Broader request budget applied by the supported ingest policy. |
| Window seconds | Duration of the rate-limit accounting window; the form requires at least 60 seconds. |
| Slack notify minimum severity | Low, Medium, High or Critical threshold for the supported ingest notification path. This is not the alert's detection severity editor. |
| Max event age minutes | Notification freshness control; it is not event retention, an Indexer query window or a purge setting. |

Example trusted-source entries, using documentation-only addresses:

```text
192.0.2.10
198.51.100.0/24
```

Replace these with your own permitted source addresses. A CIDR allows the entire stated network; choose only the scope you intend. Blank trusted-source input can fall back to the environment policy. It does not universally mean deny-all or unrestricted ingestion.

Choose positive rate budgets based on expected traffic and the configured window. Request rate is not necessarily a one-to-one count of security events. Independent IP/key/global constraints can reject traffic even when one budget remains available. Check forwarding retries and ingestion errors when changing limits; increasing a budget does not repair a bad key, TLS failure or disallowed source.

Select **Save Wazuh Policy**, then verify actual received alerts and the intended notification behavior. The maximum-event-age field permits zero, but confirm the consuming workflow's semantics before using it as a special value; do not assume it disables ingestion or notification universally.

Use the **Wazuh Managers** link for Manager API/Indexer addresses, credentials and connection tests. Use the [Wazuh Setup guide](../docs/WAZUH_SETUP.md) for CA trust and forwarder deployment. This policy form does not install the forwarding service or generate every connection credential.

## 4. AI Provider

| Field | Purpose |
|---|---|
| Mode | UI configuration, environment fallback or disabled state. |
| Provider | OpenAI, Anthropic, DeepSeek or Local/Ollama. |
| Model | Model identifier supported by your chosen service. Suggestions change with provider; a custom name can be entered. |
| API key | Replacement provider credential; leave blank to preserve the saved secret. |
| Local AI URL | Endpoint for the Local provider; it must be reachable from AlistoIR. |
| Max tokens | Requested generation budget; form range 1–32,768. Provider/model and workflow limits still apply. |
| Temperature | Generation variation setting; form range 0–2. Higher is not a guarantee of better accuracy. |

### Configure a hosted provider

1. Choose **Configured in UI** and select the provider.
2. Enter that provider's model and appropriate API key. When switching vendors, replace the credential as needed; preserving an old vendor's key does not make it valid for the new vendor.
3. Review the token/temperature settings and select **Save AI Provider**.
4. Check **AI Provider Routing** in the Integrations overview.
5. Test a small authorized AI workflow and inspect its actual response/error and evidence limitations.

Suggested models are UI hints, not confirmation of current provider availability, account entitlement or endpoint compatibility. A syntactically complete configuration can still fail because of quota, an unsupported model or provider permissions. This page has no dedicated “test every AI model” button.

### Configure Local/Ollama

Select Local/Ollama, use the installed model identifier and configure the supported local API endpoint. The suggested URL is:

```text
http://localhost:11434/api/generate
```

This works only when that service is actually available on the AlistoIR server. `localhost` does not refer to your browser/workstation or a separate AI VM. For another host, use the appropriate reachable endpoint and secure the service/network according to your deployment.

Selecting a provider controls shared AI routing; it does not enable automatic investigation scheduling or force a new investigation of every existing alert. Configure automation separately in [Autonomous Investigation Settings](INVESTIGATION_SETTINGS.md).

Supported outbound AI context passes through the shared privacy layer, including Local-provider requests. Sanitization does not guarantee recognition of every sensitive detail or make an external endpoint internal. See [AI privacy](../docs/AI_PRIVACY.md).

## 5. Notification Channels

The four channels share **Save Notifications**. Review all four modes before saving that form: it submits the supported settings for Slack, Teams, Telegram and PagerDuty together.

| Channel | Fields | Saved test |
|---|---|---|
| Slack | Mode, Webhook URL, Channel. The webhook's destination permissions still determine delivery. | Test Saved Slack |
| Microsoft Teams | Mode, Webhook URL. | Test Saved Teams |
| Telegram | Mode, Bot token, Chat ID. Use a destination accessible to that bot. | Test Saved Telegram |
| PagerDuty | Mode, Routing key. | Test Saved PagerDuty |

For each service, select the mode, enter valid destination credentials, select **Save Notifications**, then use its **Test Saved…** button. Unsaved field edits are not the credentials used by that test, and testing does not save those edits automatically.

Tests send real messages/events. PagerDuty's test sends a trigger event that can create/update an incident under the destination's rules. Verify the destination and handle the test incident there as appropriate. A successful transport result does not acknowledge an AlistoIR alert or demonstrate that a human received the message.

Notification configuration supplies transport settings. Actual operational messages still depend on the calling workflow, trigger/recipient settings and notification policy. A successful test does not guarantee every alert/case/task action will send a notification.

## 6. Threat Intelligence Providers

Most providers offer Mode and a masked/replacement credential field. MISP/OpenCTI also need an API URL; some public/default providers have no key field.

| Provider | Fields specific to configuration |
|---|---|
| VirusTotal | Mode and API key. |
| AbuseIPDB | Mode, API key and Confidence minimum (0–100; displayed fallback 25). This is provider-specific reputation configuration, not the AI verdict-confidence threshold. |
| MISP | Mode, API URL, API key and Verify TLS certificate. |
| OpenCTI | Mode, API URL and API token. |
| AlienVault OTX | Mode and API key. |
| GreyNoise | Mode and API key. |
| Shodan | Mode and API key. |
| URLScan.io | Mode and API key. |
| CISA KEV | Enabled by .env/default or Disabled; no key field. |
| EPSS | Enabled by .env/default or Disabled; no key field. |
| CIRCL hashlookup | Enabled by .env/default or Disabled; no key field. |
| ThreatFox | Mode and Auth-Key; the application's supported path expects a key. |
| URLhaus | Mode and Optional Auth-Key for supported lookups. |
| MalwareBazaar | Mode and Optional Auth-Key for supported hash lookups. |

Choose only the providers you need, enter their supported settings and select **Save Threat Intel**. This submits the provider section together, not just the last key edited. Review effective enabled/disabled states after saving. “No API key required” does not guarantee unlimited use, service availability or complete indicator coverage.

Keep **Verify TLS certificate** enabled for MISP with a correctly trusted certificate. A private/self-hosted API URL may also be subject to destination validation. Use the supported private-CTI destination controls described in [Configuration](../docs/CONFIGURATION.md#private-cti-destinations-and-download-safety); do not disable protections globally to bypass an error.

This page saves provider settings; it does not run every enrichment or create scheduled feeds. Add the appropriate source in the IOC workspace for MISP/OpenCTI import, then verify synchronization separately. See [Threat Intelligence](THREAT_INTELLIGENCE.md) for provider purpose, feeds, trust exceptions, confidence and the Community saved-IOC limit.

Intelligence API calls can disclose the queried observable to the intelligence service. They are separate from LLM requests and are not covered by the AI outbound sanitizer. The same separation applies to notification payloads and local exports.

## 7. Email SMTP

| Field | Purpose |
|---|---|
| Mode | UI configuration, .env fallback or Disabled. |
| Host | SMTP server hostname/address, reachable from AlistoIR. |
| Port | Mail server's supported SMTP port; form default 587. |
| Encryption | TLS, SSL or None; match the mail service's required transport. |
| Username | Mail service authentication identity where required. |
| Password | Replacement credential; blank preserves the saved password. |
| From email | Sender identity permitted by your mail service. |
| From name | Display name used for outgoing mail. |
| Test recipient email | Destination used for Send Test Email; not a global SOC notification recipient setting. |

### Save and verify SMTP

1. Enter settings supplied by your mail administrator/provider. Use the required transport and sender identity.
2. Select **Save SMTP**.
3. Enter a recipient you control, then select **Send Test Email**.
4. Review the application result and confirm delivery in that mailbox.

The test uses saved effective SMTP settings. If the submitted test-recipient field is blank, the handler falls back to the current user's email. It sends a real message and does not save a permanent recipient list.

A send success is not proof of inbox placement or that every notification workflow is enabled. Check spam/quarantine, allowed sender, authentication and mail-server responses when delivery is missing. Account-creation and reset email depend on their own workflows; see [User Management](USERS.md).

## 8. Save and test actions

| Button | Saves/tests |
|---|---|
| Save Wazuh Policy | Ingest policy fields in that form. |
| Save AI Provider | AI mode, provider and associated generation settings/credential. |
| Save Notifications | All four supported channel settings in the notifications form. |
| Test Saved Slack / Teams / Telegram / PagerDuty | Uses persisted effective settings and sends an outbound message/event; does not save unsaved channel edits. |
| Save Threat Intel | Supported provider configuration in that form. |
| Save SMTP | SMTP configuration in that form. |
| Send Test Email | Uses saved SMTP and submitted test-recipient selection; does not save SMTP edits automatically. |

Each form has a signed-in security token. If a stale session/token causes Security validation failed, refresh the form and re-enter only the intended changes. Keep CSRF/integrity protections enabled. Concurrent administrators can change settings between page load, save and test; recheck the effective values rather than assuming every tab is current.

## 9. Community scope and related settings

Community remains Wazuh-only for SIEM ingestion and supports one configured Wazuh manager. This form does not unlock additional managers or other SIEM sources. Use Wazuh Managers and the setup wizard for connection/forwarding work.

Provider configuration does not increase Community's five active accounts, five active playbooks or 3,000 active unique saved IOC capacity. External provider quotas, availability and charges are separate from application edition limits.

Automatic investigation enablement/rule scope, playbook triggers, SLA policies, IOC source schedules and individual case decisions are configured in their respective workspaces. Saving integrations alone does not make those workflows complete.

## 10. Troubleshooting

| Symptom | What to check |
|---|---|
| Values saved but service still uses another setting | Mode and effective source; .env fallback can take precedence over entered UI values. |
| Blank secret field after reload | Expected for masked/replacement inputs; inspect the Current indicator. |
| Cannot save a new secret | Secret-storage readiness/encryption configuration and returned validation error. |
| Test used old credentials | Save first; Test Saved buttons intentionally use persisted configuration. |
| Other unsaved changes disappeared | Save/test redirects reload the page; work one form at a time. |
| Provider changed but AI fails | Enter the matching vendor key/model; preserved credentials may belong to the previous vendor. |
| Local AI unavailable | Server-side reachability, listener/API compatibility and installed model; localhost means the AlistoIR host. |
| Wazuh stopped forwarding | Trusted sender address, key, TLS, rate budgets/window and forwarder retry/error logs. |
| Older alerts have no Slack notification | Notification freshness/severity policy can suppress notification without implying alert deletion. |
| MISP/private URL rejected | Verify destination allowlist, URL, TLS trust and supported private-CTI policy. |
| Provider configured but IOC feed empty | Import source, scheduler, provider response, quota and Community capacity. |
| Notification test button disabled | Complete and enable saved effective settings first. |
| SMTP succeeds but mail missing | Confirm recipient, sender authorization, spam/quarantine and mail-server delivery results. |
| Needs setup / Configured seems inaccurate | Badges inspect configuration, not live health; verify with the real supported workflow. |
| Access or organization error | Administrator authorization and the applicable organization record/context. |

Related guides: [Integrations overview](INTEGRATIONS.md), [Wazuh Setup](../docs/WAZUH_SETUP.md), [AI privacy](../docs/AI_PRIVACY.md), [Threat Intelligence](THREAT_INTELLIGENCE.md), [Autonomous Investigation Settings](INVESTIGATION_SETTINGS.md) and [Troubleshooting](../docs/TROUBLESHOOTING.md).
