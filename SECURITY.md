# Security

## Testing release scope

3.0.0 is the current official Community release. Do not assume that a passed automated test or signed package makes the entire application vulnerability-free. Keep the OS patched, restrict management access, use strong unique credentials/SSH keys and plan evidence retention.

## Controls included

- Protected internal/test/maintenance routes and no production development tests.
- CSRF/role checks on management flows; hashed, manager-bound ingest keys.
- Verified TLS and private CA handling for setup/forwarding.
- Restricted/validated IOC destinations, pinned DNS, no automatic redirects and bounded decoded responses.
- Server-side Community quota enforcement and signed integrity checks for official protected files.
- Private runtime configuration outside the web root.

Signed integrity detects changes to official protected files. A root/source owner can remove checks or alter database controls. This is not unbreakable licensing and does not protect arbitrary OS files.

## Report a vulnerability

Contact the repository owner privately. If GitHub's private vulnerability-reporting option is available, use it; otherwise arrange a private channel with the owner. Do not publish exploit details involving a live system, passwords, tokens or raw SOC/client evidence in a normal issue.

For a report, include affected release/hash, reproduction steps using sanitized lab data, impact, and a safe suggested fix. Do not perform destructive testing against another organization's deployment.

## Never publish

- `.env`, database dumps, evidence archives or installation logs containing one-time credentials.
- Wazuh deployment ZIPs containing ingest keys.
- AI/provider credentials, license signing keys or Community release signing keys.
- Private CA/server keys. Only public CA certificates should be copied to clients.

This repository contains documentation and release metadata. Application installation is through the named release asset; GitHub Source code archives are not installers.
