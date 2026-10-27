# Wazuh setup: generated files and one-command installation

There are **two different ZIPs**:

- The Community release installer installs the AlistoIR server.
- The wizard's **`alistoir-wazuh-deployment.zip`** installs forwarding on your Wazuh manager and contains an ingest credential.

## 1. Configure the connection

As AlistoIR System Administrator, open **Wazuh Setup** (`Admin -> Wazuh Setup`). Click **Configure Wazuh Manager & Indexer**, enter the addresses, credentials and manager key, then save and enable the connection. Community supports one configured manager, including any environment-configured connection. Editing the existing record is the normal way to change it.

Use read-only API/Indexer permissions where practical for collection. Keep TLS verification enabled. Manager API authentication and Indexer evidence search permissions are tested separately.

## 2. Paste public CA certificates if needed

Open the wizard's self-signed certificate section. Paste the Manager API public CA PEM and/or Indexer public CA PEM, then save. Do not paste private keys. Trust applies to this integration only. Confirm the certificate source/fingerprint through a trusted administrative channel before importing it.

## 3. Test the saved connections

Click **Test saved connections**. The Indexer check uses a size-zero match-none query and does not retrieve event content. If it fails, check routing, ports, TLS trust, credentials and permissions. It does not prove raw archive coverage or endpoint readiness.

## 4. Generate the deployment ZIP

Enter the Wazuh server's source IP as AlistoIR sees it. Account for NAT or a trusted reverse proxy. The wizard adds this IP to the existing ingest allowlist and generates a new manager-bound ingest key. It does not overwrite existing keys automatically.

The ZIP includes:

- `wazuh_forwarder_standalone.py`
- `alistoir-wazuh-forwarder.service`
- `wazuh_forwarder.env`
- `install-alistoir-forwarder.sh`
- `check-forwarder.py`
- The public AlistoIR CA when available and a README

Manager API and Indexer passwords are not included. Keep this generated ZIP private; never attach it to a GitHub issue.

## 5. Copy to Wazuh and install

Use your actual administrator account and Wazuh address:

```powershell
scp .\alistoir-wazuh-deployment.zip wazuhadmin@WAZUH_IP:/home/wazuhadmin/
ssh wazuhadmin@WAZUH_IP
```

On the Wazuh manager:

```bash
umask 077
mkdir -p ~/alistoir-wazuh-setup
unzip ~/alistoir-wazuh-deployment.zip -d ~/alistoir-wazuh-setup
cd ~/alistoir-wazuh-setup
sudo bash install-alistoir-forwarder.sh
```

Requires Linux/systemd, Python 3.9+, standard SSL/SQLite modules, sudo, and readable `/var/ossec/logs/alerts/alerts.json`. No third-party Python packages are required by the forwarder.

Optional preflight without changing forwarding configuration:

```bash
sudo bash install-alistoir-forwarder.sh --check-only
```

The installer validates TLS, the ingest credential, source IP and manager binding before changing files. It backs up its own configuration, applies restrictive secret permissions, registers the service and checks startup. Managed reruns preserve cursor state; a failed restart restores the previous files and activation state.

A different existing forwarder is deliberately not overwritten. Plan and perform an explicit migration so two services do not forward the same stream. Do not delete the old cursor or run both forwarders to bypass this check.

Fresh installations start at the end of `alerts.json` to avoid unexpectedly replaying historical alerts. They forward all rule levels by default. Original manager identity is retained in `manager.original_name` when routing through the configured manager key.

## 6. Verify the service and actual alerts

```bash
sudo systemctl status alistoir-wazuh-forwarder --no-pager
sudo journalctl -u alistoir-wazuh-forwarder -n 30 --no-pager
```

Review logs locally and redact them before sharing. Return to Wazuh Setup and refresh. Connectivity heartbeat is authenticated and does **not** create a SOC alert. Generate approved benign activity in your lab and verify that a real Wazuh alert appears in AlistoIR.

Check each result separately:

| Check | Meaning |
|---|---|
| Connectivity check | TLS/key/source-IP/manager binding accepted |
| New saved alert | Actual alert forwarding verified |
| API/Indexer tests | Authenticated access/search path verified |
| Endpoint callback | Supported collector returned a result; review evidence coverage |
| AI configured | Credentials/configuration present; live output still needs validation |

No synthetic alert, archive enablement, endpoint-response policy change or AI enablement is performed automatically by this installer.

You can revoke a generated bundle key from Wazuh Setup. This stops that bundle's forwarding; confirm the intended replacement is ready first. API/Indexer credentials and previously deployed keys are not automatically rotated.
