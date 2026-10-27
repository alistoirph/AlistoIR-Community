# Troubleshooting

Do not paste `.env`, installer output with credentials, generated Wazuh deployment ZIPs or raw client evidence into issues. Collect sanitized errors and timestamps instead.

| Symptom | Check / action |
|---|---|
| Browser certificate warning | Verify and trust the public local CA for the correct device/store; keep TLS verification enabled. |
| Browser cannot reach AlistoIR | Verify VM address, TCP 443, Nginx/PHP-FPM and routing. |
| Installer refuses an existing installation | Expected safety check. Use a new VM or request an explicit upgrade/recovery procedure; do not delete data to force installation. |
| Slow Ubuntu package downloads | Check network/mirror health and wait for progress. Do not interrupt an active package installation casually. |
| Login available but no alerts | Configure Wazuh, generate/install its deployment bundle, and verify source IP/key/service. A new installation has no sample customer alerts. |
| Alert not found | Use an existing AlistoIR Alert ID, not the Wazuh Rule ID. The alert may also be deleted or outside your access scope. |
| Autonomous investigation disabled | Configure integrations, then enable it in Investigation Settings. |
| Investigation queued | Waiting for the worker/budget or earlier work; cron normally checks each minute. |
| Investigation inconclusive | Required evidence is missing, unavailable or insufficient. Review gaps; do not treat it as a benign verdict. |
| Provider balance/quota error | Restore provider access or intentionally configure another supported provider. No credits are bundled. |
| Indexer connection refused/timeout | Verify address, port, service, routing and allowed source IPs. |
| API/Indexer TLS error | Paste the correct public CA for the integration and verify hostname/IP in the certificate. |
| Endpoint callback fails TLS | The endpoint service needs to trust AlistoIR's public CA; browser-only trust may not suffice. |
| Forwarder installation reports existing service | Perform an explicit migration; do not run two forwarding services or discard state. |
| Forwarder probe returns 401 | Check enabled manager-bound key, expiry/revocation and edition allowance. |
| Forwarder probe returns 403 | Check the source IP as seen by AlistoIR and its allowlist; account for NAT. |
| Add controls disabled | Community active/configured limit reached; disable/pause/remove the appropriate existing item safely. |
| IOC feed redirect/private destination blocked | Use the final HTTPS URL or have an administrator explicitly allow an approved private CTI origin. |
| IOC import skips rows | Review duplicate/invalid rows and the 3,000 active unique IOC quota. |
| Integrity validation failed | Restore matching official protected files and manifest. Preserve database/private runtime; do not edit quota/verifier code. |

## Service and scheduler checks

```bash
sudo nginx -t
sudo systemctl is-active nginx php8.3-fpm mariadb cron
sudo journalctl -u nginx -n 30 --no-pager
sudo journalctl -u php8.3-fpm -n 30 --no-pager
sudo tail -n 30 /var/alistoir/logs/cron-investigations.log
```

Inspect output locally. Redact tokens, email addresses, internal hosts and alert evidence before sharing.

## Paths (compatibility names are expected)

- Application: `/var/www/alistoir-professional`
- Private environment: `/var/alistoir/.env`
- Runtime logs: `/var/alistoir/logs`
- Cron configuration: `/etc/cron.d/alistoir-professional`
- Public local CA: `/etc/nginx/ssl/alistoir-professional/root-ca.pem`
- Public CA copy for setup bundles: `/var/alistoir/wazuh-setup-ca.pem`
- Forwarder environment on Wazuh: `/etc/alistoir-wazuh-forwarder/wazuh_forwarder.env`
- Forwarder cursor on Wazuh: `/var/lib/alistoir-wazuh-forwarder/state.db`

Internal PHP, maintenance and test URLs returning 404 is an intentional security restriction. Production test scripts are not installed in the web root.

## Recovery and upgrades

This prerelease has no supported in-place Community System Updates UI. Back up the database (including triggers), private runtime and certificates before a maintainer-guided migration. These backups contain secrets/evidence: store them privately and never upload them to GitHub. Do not run the fresh installer over an existing deployment.
