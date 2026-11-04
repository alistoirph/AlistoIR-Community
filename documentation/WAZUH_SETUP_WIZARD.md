# Wazuh Integration Setup — wizard guide

Page: `/integrations/wazuh_setup`. Applies to AlistoIR Community 3.0.4. System Administrator access is required.

The wizard prepares Wazuh alert forwarding using saved connection settings and a generated deployment ZIP. It guides you through connection tests, certificates, installation and readiness checks. For Manager API/Indexer fields and illustrated connection examples, see [Wazuh setup and configuration](../docs/WAZUH_SETUP.md).

<!-- toc:start -->

## Table of contents

- [1. Before you start](#1-before-you-start)
- [2. Save your Wazuh connection](#2-save-your-wazuh-connection)
- [3. Configure public CA certificates](#3-configure-public-ca-certificates)
- [4. Test API and evidence access](#4-test-api-and-evidence-access)
- [5. Generate the deployment bundle](#5-generate-the-deployment-bundle)
- [6. Understand the generated files](#6-understand-the-generated-files)
- [7. Install on the Wazuh manager](#7-install-on-the-wazuh-manager)
- [8. Verify readiness](#8-verify-readiness)
- [9. Bundle history and key revocation](#9-bundle-history-and-key-revocation)
- [10. Reruns and existing forwarders](#10-reruns-and-existing-forwarders)
- [11. Troubleshooting](#11-troubleshooting)

<!-- toc:end -->

## 1. Before you start

Prepare:

- An AlistoIR System Administrator account and a reachable HTTPS application URL.
- The Wazuh Manager API and Indexer addresses, appropriate credentials and trusted public CA certificates where needed.
- The Wazuh manager's source IP **as observed by AlistoIR**, accounting for NAT.
- Administrative shell access on the Wazuh manager, Python 3.9 or newer, systemd and the required installer tools.
- A readable `/var/ossec/logs/alerts/alerts.json` on that manager. Enable Wazuh JSON alert output if necessary.

Two directions must work independently: **Wazuh → AlistoIR** for forwarding, and **AlistoIR → Wazuh API/Indexer** for supported collection. Endpoint callbacks and AI-provider connectivity are additional paths.

There are two different packages: the public AlistoIR Community installer installs the application server; the wizard's **`alistoir-wazuh-deployment.zip`** installs forwarding on the Wazuh manager and contains a private ingest credential. Do not publish the deployment ZIP.

## 2. Save your Wazuh connection

In step **1. Save your Wazuh connection**, select **Configure Wazuh Manager & Indexer**.

1. Add or edit the intended connection.
2. Enter the Manager API and Indexer settings, credentials and trusted CA paths as required.
3. Keep HTTPS/TLS verification enabled and save/enable the connection.
4. Return to the wizard and confirm the displayed manager and enabled state.

Community supports **one configured Wazuh manager**, including an environment-configured connection. Edit the existing connection rather than adding another. A disabled connection does not necessarily free that configured-manager allowance.

Where multiple eligible connections exist in Professional, the wizard can display a manager selector. Verify the selected manager before testing, generating a bundle or revoking a key. Professional connection availability follows its applicable allowance.

All POST actions require an enabled, eligible saved manager. A button appearing on the page does not bypass that server-side requirement. Use the [field-by-field connection guide](../docs/WAZUH_SETUP.md#wazuh-managers-field-by-field-configuration) for the individual Manager API/Indexer fields.

## 3. Configure public CA certificates

Expand **Using self-signed Wazuh certificates? Paste public CAs here** when the integration needs private/self-signed certificate trust.

| Input | Trust direction |
|---|---|
| Wazuh API public CA PEM | Lets AlistoIR validate the Wazuh Manager API's HTTPS certificate. |
| Indexer public CA PEM | Lets AlistoIR validate the Indexer's HTTPS certificate. |

Paste the appropriate **public CA certificate**, then select **Save integration certificates**. At least one non-empty valid certificate is required. Blank fields leave the corresponding certificate setting unchanged. Never paste a private key or an unrelated certificate solely to bypass a TLS error.

The wizard stores the PEM under the private runtime's `wazuh-ca` directory, updates the selected integration's CA path and enables its TLS verification. It makes **no system-wide trust-store change**. Return to Test saved connections after saving.

These Wazuh certificates are separate from the optional **AlistoIR public CA** included in the deployment bundle. That CA lets the forwarder validate AlistoIR's HTTPS endpoint. Trusting the API from AlistoIR does not automatically make Wazuh trust AlistoIR.

## 4. Test API and evidence access

In step **2. Test API and evidence access**, select **Test saved connections**.

| Result | What the test establishes |
|---|---|
| Wazuh API: Connected | Authentication to the saved Manager API succeeded. |
| Wazuh API: Failed | Review address, credentials, routing and trusted CA. |
| Wazuh Indexer: Connected | An authenticated read-only search against `wazuh-alerts-*` succeeded. The probe requests no event content. |
| Wazuh Indexer: Failed or not configured | Review URL/port, credentials, index/search permissions and CA. |

API HTTPS and TLS verification are required for the wizard's test flow. Indexer HTTPS/TLS verification is checked separately. An API success does not imply an Indexer success.

The Indexer probe uses a zero-result query; it does not prove that the required event/time window exists, that raw archives are available or that every collection query is authorized. It also does not trigger endpoint response, collect endpoint evidence or contact an AI provider.

## 5. Generate the deployment bundle

In step **3. Download your deployment bundle**:

1. Enter **Wazuh source IP (as seen by AlistoIR; account for NAT)**.
2. Check that the selected manager is correct and enabled.
3. Select **Generate & download ZIP**.
4. Save `alistoir-wazuh-deployment.zip` privately for transfer to that Wazuh manager.

This input accepts a single IPv4/IPv6 address, **not CIDR or a hostname**. The suggested value can be derived from the Manager API URL; verify it because the API host and the forwarder's actual network source can differ.

Generating a bundle is a configuration change: it adds the explicitly entered source IP to the existing ingest allowlist, creates a new **manager-bound ingest key** and records the deployment. Other existing keys remain valid until revoked. Merely downloading the ZIP does not install/start forwarding.

Repeated generation creates additional credentials/deployment records. Keep track of which bundle is installed before revoking older keys. API/Indexer passwords are not placed in the ZIP.

## 6. Understand the generated files

| File | Purpose |
|---|---|
| `wazuh_forwarder_standalone.py` | Reads local Wazuh JSON alerts and forwards through the supported ingest path. |
| `alistoir-wazuh-forwarder.service` | systemd unit for the generated forwarder deployment. |
| `wazuh_forwarder.env` | Generated endpoint, manager identity, ingest credential and forwarding settings. Contains sensitive configuration. |
| `install-alistoir-forwarder.sh` | Preflight, backup, managed installation and service setup. |
| `check-forwarder.py` | Supported connectivity/TLS/authentication preflight. |
| `README.txt` | Bundle-specific installation and verification instructions. |
| `alistoir-root-ca.pem` | Optional public CA for AlistoIR HTTPS trust when available to the wizard. |

The generated endpoint is based on AlistoIR's configured origin and uses `/alerts/ingest.php`. The default input file is `/var/ossec/logs/alerts/alerts.json`. Some environment keys retain the compatible `TRACKIR_` prefix; that naming does not identify a different product.

If an internal AlistoIR CA is required but absent from the bundle, arrange the supported certificate configuration before installing. Do not disable TLS verification to make an untrusted endpoint pass.

## 7. Install on the Wazuh manager

Transfer the generated ZIP securely to the **Wazuh manager**, not to the AlistoIR application server or a Windows endpoint. For example, from a machine holding the download:

```bash
scp alistoir-wazuh-deployment.zip WAZUH_USER@WAZUH_HOST:~/
```

Replace `WAZUH_USER` and `WAZUH_HOST` with your own values. On the Wazuh manager, extract into a private directory:

```bash
umask 077
mkdir -p ~/alistoir-wazuh-setup
unzip ~/alistoir-wazuh-deployment.zip -d ~/alistoir-wazuh-setup
cd ~/alistoir-wazuh-setup
```

Ensure `unzip` is installed if needed. Optionally run the supported preflight before changing the managed forwarder configuration:

```bash
sudo bash install-alistoir-forwarder.sh --check-only
```

This still makes an authenticated connectivity check; it is not an offline-only validation. Then install:

```bash
sudo bash install-alistoir-forwarder.sh
```

The installer verifies dependencies, JSON-alert-file readability, TLS and manager-bound authentication/source access before replacing its managed files. It requires tools including Python, systemctl, flock, install and tar. Full installation checks Python 3.9+ and the forwarder syntax/runtime prerequisites.

| Installed location | Contents |
|---|---|
| `/opt/alistoir/wazuh-forwarder/` | Forwarder and check script. |
| `/etc/alistoir-wazuh-forwarder/` | Private environment configuration and optional public AlistoIR CA. |
| `/etc/systemd/system/alistoir-wazuh-forwarder.service` | Managed service definition. |
| `/var/lib/alistoir-wazuh-forwarder/` | Persistent forwarder state. |
| `/var/log/alistoir-wazuh-forwarder/` | Forwarder runtime log directory. |
| `/var/backups/alistoir-wazuh-forwarder/` | Timestamped configuration backups for managed changes. |

On a fresh installation, forwarding starts at the **end of the existing alerts file**. Historical entries are not automatically replayed. Existing cursor state is preserved on supported reruns. Generate/observe a new authorized event after installation when verifying receipt.

The installer enables/restarts the service and checks that it is active. Failures during managed replacement have a rollback path restoring previous backed-up files/service state; inspect the error and backup location rather than repeatedly running different installers.

## 8. Verify readiness

Check service state and recent logs on Wazuh:

```bash
sudo systemctl status alistoir-wazuh-forwarder --no-pager
sudo journalctl -u alistoir-wazuh-forwarder -n 30 --no-pager
```

Return to the wizard's step **5. Verify readiness** and refresh.

| Indicator | Meaning / limit |
|---|---|
| Connectivity: Awaiting authenticated forwarder check | No recorded authenticated check for that deployment yet. Downloading a bundle alone does not satisfy this. |
| Connectivity: Last verified … UTC | A check was recorded at that time. It is not a continuous heartbeat or proof that alerts arrived afterward. |
| Connectivity: Key revoked | This deployment credential was revoked and can no longer authenticate. |
| Alert receipt | Latest saved non-deleted Wazuh alert whose recorded manager name matches the configured manager key. Missing/mismatched metadata can affect this check. |
| Endpoint callback | Latest matching completed endpoint investigation with a recorded result summary; review its actual evidence/coverage. |
| AI provider | Effective settings appear configured/enabled; provider availability and generated verdict still need validation. |

These indicators show latest records and can be historical; they do not certify current health or tie every alert/callback to the latest bundle. Verify new entries and relevant timestamps in [Alerts](ALERTS.md).

The forwarder check sends no synthetic SOC alert automatically. Run a supported read-only endpoint investigation to verify callback/evidence separately. Configure and validate AI through [Integration Configuration](INTEGRATION_CONFIGURATION.md), then review [Investigation Center](INVESTIGATION_CENTER.md) findings.

The wizard **does not enable archives, endpoint response or autonomous AI processing**. Enable autonomous investigations through [Investigation Settings](INVESTIGATION_SETTINGS.md) only after confirming required collection paths, provider behavior and intended rule scope.

## 9. Bundle history and key revocation

The wizard displays up to **five newest deployment records** for the selected manager. Each shows its bundle ID, source IP and connectivity/revocation state. This is not necessarily the complete credential history.

**Revoke this bundle key (stops its forwarding)** invalidates the credential associated with that deployment. It does not uninstall the remote service, revoke every other bundle key, remove stored alerts or undo endpoint actions. A forwarder still using the revoked key will fail authentication.

For rotation, prepare/install the intended replacement bundle, verify new forwarding, then revoke the old credential when no longer needed. Do not revoke the only working bundle as a routine connection test. Deleting a downloaded ZIP does not revoke its embedded key.

## 10. Reruns and existing forwarders

The installer recognizes its own managed service and preserves cursor state on supported reruns. It refuses an unrelated service configuration or active legacy forwarder to avoid duplicate forwarding. A second concurrent installer is also rejected.

If another forwarder already exists, inventory its endpoint, identity, cursor/state and service configuration, then plan an explicit migration. Stop/migrate the correct old service through your supported maintenance process rather than installing two forwarders for the same input. Do not delete state files merely to make a check pass: doing so can change replay behavior.

Use the same private installation directory carefully; avoid extracting an unrelated bundle over it without knowing which credentials will become active. Preserve the current working deployment/backup until replacement receipt is verified.

## 11. Troubleshooting

| Symptom | What to check |
|---|---|
| No manager / actions disabled | Save and enable an eligible manager first; Community permits one configured connection. |
| Selected manager unavailable | Manager selection, enabled state and applicable allowance; refresh after configuration changes. |
| Certificate save rejected | Paste a valid public PEM certificate, not a private key; at least one field must be non-empty. |
| API works but Indexer fails | Separate Indexer address, port, credentials, TLS/CA and search permissions. |
| Single source-IP input rejected | Use an address, not CIDR/hostname; match the address seen by AlistoIR. |
| Bundle cannot be generated | Enabled manager, PHP ZIP support, allowed ingest-key creation and server logs. |
| Preflight fails before install | Endpoint reachability, expected source IP, manager-bound credential and AlistoIR certificate trust. |
| alerts.json missing/unreadable | Run on the manager and enable/check JSON alert output; this is not the raw archives file. |
| Existing forwarder refused | Explicit migration is required; avoid duplicate forwarding and preserve state. |
| Service active but no old alerts | Fresh installation starts at end-of-file; verify a new authorized event. |
| Last verified but no new saved alerts | Connectivity probe is separate from actual forwarding, ingest policy and event production. |
| Alerts exist but receipt says none | Check configured manager key versus forwarded manager-name metadata. |
| Callback/provider appears ready but verdict is inconclusive | Inspect evidence coverage, freshness and supported collection; readiness is not proof of a correct final verdict. |
| Forwarding stopped after revoke | Installed forwarder is using a revoked credential; deploy the intended valid bundle. |
| Security validation failed | Refresh the signed-in page and retry without disabling security controls. |

Related guides: [Wazuh connection fields and deployment](../docs/WAZUH_SETUP.md), [Integrations](INTEGRATIONS.md), [Integration Configuration](INTEGRATION_CONFIGURATION.md), [Wazuh evidence and AI investigation](WAZUH_AI_INVESTIGATION.md) and [Troubleshooting](../docs/TROUBLESHOOTING.md).
