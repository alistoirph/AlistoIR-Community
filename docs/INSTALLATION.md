# Installation — AlistoIR Community 3.0.0

## 1. Prepare a new VM

Use Ubuntu Server 24.04 LTS x86-64 with 2 vCPUs, 4 GB RAM, at least 40 GB disk, a stable IP/hostname and Internet access. Use a dedicated lab VM; do not install over an existing AlistoIR deployment. The installer refuses an existing app directory/environment or a non-empty target database.

Network access:

| Direction | Purpose |
|---|---|
| Administrator -> AlistoIR TCP 22 | SSH installation/maintenance; restrict to management networks |
| Browser/Wazuh/endpoints -> AlistoIR TCP 443 | UI, ingestion and supported callbacks |
| AlistoIR -> Wazuh configured API port (typically 55000) | Manager/agent API |
| AlistoIR -> configured Indexer port (typically 9200) | Historical evidence searches |
| AlistoIR -> configured AI/TI destinations | Explicitly enabled integrations |

Port 80 provides an HTTPS redirect when reachable. Preserve normal Wazuh agent connectivity separately. Do not expose management ports broadly just to complete setup.

## 2. Download the actual installer

Open [the release](https://github.com/alistoirph/AlistoIR-Community/releases/tag/v3.0.0). Download:

- `alistoir-community-v3.0.0-one-click.zip`
- `SHA256SUMS.txt`

The automatically generated GitHub Source code archives are documentation-only.

Optional GitHub CLI download:

```bash
gh release download v3.0.0 --repo alistoirph/AlistoIR-Community --pattern 'alistoir-community-v3.0.0-one-click.zip' --pattern 'SHA256SUMS.txt'
```

## 3. Verify SHA-256 before installation

Linux, from the download directory:

```bash
sha256sum -c SHA256SUMS.txt
```

Windows PowerShell:

```powershell
Get-FileHash .\alistoir-community-v3.0.0-one-click.zip -Algorithm SHA256
Get-Content .\SHA256SUMS.txt
```

The hash must exactly match. Stop if it differs and re-download from the official release.

## 4. Upload to Ubuntu and extract

Replace `ubuntu` and `ALISTOIR_IP` with your VM's administrator account and address:

```powershell
scp .\alistoir-community-v3.0.0-one-click.zip ubuntu@ALISTOIR_IP:/home/ubuntu/
ssh ubuntu@ALISTOIR_IP
```

On Ubuntu:

```bash
sudo apt update
sudo apt install -y unzip ca-certificates
umask 077
mkdir -p ~/alistoir-community-install
unzip ~/alistoir-community-v3.0.0-one-click.zip -d ~/alistoir-community-install
cd ~/alistoir-community-install/alistoir-community-v3.0.0-one-click
```

The ZIP filename, internal directory and core application version identify `3.0.0`.

## 5. Install

Replace `192.0.2.10`, organization and email below with your real values. Use the final IP/hostname you will access; it is used for HTTPS and callback URLs.

```bash
sudo env \
  APP_BASE_URL="https://192.0.2.10" \
  APP_TIMEZONE="Asia/Manila" \
  PROFESSIONAL_ADMIN_TIMEZONE="Asia/Manila" \
  PROFESSIONAL_ORGANIZATION_NAME="Example SOC" \
  PROFESSIONAL_ADMIN_NAME="Administrator" \
  PROFESSIONAL_ADMIN_EMAIL="admin@example.com" \
  bash install.sh
```

`PROFESSIONAL_*` variable names and internal `alistoir-professional` paths are compatibility names. The installed edition is **AlistoIR Community** and requires no paid license.

The installer configures Nginx, PHP, MariaDB, HTTPS, schema/migrations, background jobs, security services and the first administrator. Let it finish. If package downloads are slow, monitor them rather than repeatedly restarting the installer. If it fails after creating the app/environment, do not delete data or force an overwrite: see troubleshooting.

The final console output contains a generated administrator password, ingest key and enrollment token. Save them privately in a password manager. Do not post the output to GitHub or include it in screenshots.

## 6. Verify services and HTTPS

```bash
sudo systemctl is-active nginx php8.3-fpm mariadb cron
sudo nginx -t
sudo curl --cacert /etc/nginx/ssl/alistoir-professional/root-ca.pem \
  -s -o /dev/null -w 'Login HTTP %{http_code}\n' \
  https://192.0.2.10/auth/login
```

Replace the example IP. Expect active services, valid Nginx config and HTTP 200 for the login page.

Open `https://YOUR_ALISTOIR_HOST`, sign in with the administrator email/generated password, and change the password when prompted.

## 7. Trust HTTPS appropriately

The default installer creates a local CA. Its public certificate is:

```text
/etc/nginx/ssl/alistoir-professional/root-ca.pem
```

Verify its SHA-256 file hash over your trusted SSH connection before importing a copied certificate:

```bash
sudo sha256sum /etc/nginx/ssl/alistoir-professional/root-ca.pem
```

Use `Get-FileHash -Algorithm SHA256` to compare the same copied file on Windows. Import only the public certificate. Never copy or share `root-ca.key` or the server private key.

For browser access, a trusted CA in the relevant user's store may suffice. Windows service callbacks may need the CA in **Local Computer / Trusted Root Certification Authorities**, which requires administrator approval and affects machine services. The Wazuh forwarder bundle uses its own CA file; it does not install system-wide trust automatically.

## 8. Continue with configuration

Follow [CONFIGURATION.md](CONFIGURATION.md) and [WAZUH_SETUP.md](WAZUH_SETUP.md). Autonomous processing starts disabled. An installed service alone does not prove live evidence collection or AI availability.
