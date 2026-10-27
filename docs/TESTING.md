# Tester acceptance checklist

Use a dedicated lab with synthetic or approved test data. Record the release asset name, SHA-256, Ubuntu version, CPU/RAM/disk, Wazuh version and test timestamp. Do not attach credentials or raw client evidence.

## Installation

- [ ] SHA-256 matches the release checksum.
- [ ] Fresh Ubuntu 24.04 installation completes and creates an administrator without a paid license.
- [ ] HTTPS/login works; correct public CA trust configured.
- [ ] Administrator password changed; individual accounts/2FA configured as appropriate.
- [ ] Nginx, PHP-FPM, MariaDB and cron are active.
- [ ] Community branding, Wazuh-only integration and edition limits are visible.

## Wazuh and evidence

- [ ] One manager configured; Add New Manager disabled after it is saved.
- [ ] API and Indexer tests complete with TLS verification.
- [ ] Generated deployment ZIP kept private and installed on the intended Wazuh server.
- [ ] Authenticated connectivity check appears in Wazuh Setup.
- [ ] A real approved lab alert arrives and matches its original Wazuh timestamp/rule/host.
- [ ] Supported read-only endpoint investigation returns evidence through a valid callback.
- [ ] Historical/raw archive queries are verified separately when required; retention/storage planned.

## AI and cases

- [ ] Provider available with sufficient budget; use a controlled lab alert for initial validation.
- [ ] Automatic collection enabled for one known Rule ID, then `0` only if intended.
- [ ] Investigation advances from queued through collection/evaluation.
- [ ] Findings cite collected evidence and show meaningful gaps/limitations.
- [ ] Missing telemetry remains inconclusive instead of being assumed clean.
- [ ] Analyst reviews recommendation against known lab ground truth.
- [ ] Case linkage/task recommendations are useful; human disposition remains distinct.
- [ ] Any response action follows its existing approval requirements.

## Limits and security

- [ ] Five active accounts allowed; sixth activation blocked with a clear message.
- [ ] Inactive account frees capacity; administrator included in count.
- [ ] Five active playbooks allowed; Add/Activate disabled at cap.
- [ ] Inactive playbook drafts do not count.
- [ ] IOC active quota is 3,000 unique unexpired indicators; duplicate updates use no new slot.
- [ ] Internal/test/maintenance URLs are not publicly executable.
- [ ] Generated bundle-key revocation works when intentionally tested.

Do not run volume, destructive, or tamper tests on production. Official protected-file checks should be tested only on a disposable copy with a known recovery package.

## Feedback

Open a sanitized issue with expected/actual behavior, exact steps, timestamps and relevant configuration choices without secret values. A service being active or a provider being configured is not proof of complete investigation readiness.

There is no claimed production false-positive rate, missed-threat rate or calibrated verdict accuracy for this release. Controlled fixture tests are not a representative production benchmark.
