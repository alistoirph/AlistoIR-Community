# AlistoIR Community 3.0.2

Official maintenance release, 31 October 2026.

## Changes

This release combines the Community code-integrity/AI guardrails introduced in 3.0.1 with mandatory source protection. Eligible first-party PHP uses control-flow obfuscation. Failed required protection aborts the build; it cannot silently fall back to stripped source. Critical license, AI service, autonomous investigation and integrity files must contain actual protection tokens and pass syntax checks.

Community signs its complete immutable application inventory only after the final Community overrides are obfuscated. The 468-file signed inventory detects changed, missing, added and symlink-substituted code outside writable runtime data. Protected HTTP operations fail with 409 on invalid integrity, and application CLI workers stop with exit 78. Authentication recovery and ordinary read-only evidence views remain available where they do not invoke AI.

The autonomous tool boundary denies code editing, arbitrary shell execution and model-supplied paths/commands/scope overrides. The installer checks the signed payload and root-owned permissions before starting services. Limits remain 1 configured Wazuh manager, 5 active accounts, 5 active playbooks and 3,000 active saved unique IOC feed indicators.

## Source-protection scope

The payload contains 361 PHP files: 218 obfuscated, 113 mixed UI templates compacted, 26 declarative files separately classified, 2 migration/documentation helpers compacted and 2 vendor files preserved. These exceptions are deliberate and are not reported as obfuscated. HTML/JS/CSS/SQL and integration scripts remain inspectable. This is not complete source encryption and requires no proprietary PHP decoder extension. See [SOURCE_PROTECTION.md](SOURCE_PROTECTION.md) and [CODE_GUARDRAILS.md](CODE_GUARDRAILS.md).

Obfuscation and AGENTS.md cannot force an external AI or a privileged root owner to refuse editing. OS permissions, signature checks, server-owned scope and explicit tool authorization remain the enforced controls. No open-source license grant or production verdict-accuracy certification is implied.

## Validation

- 29 build-protection fixtures passed, including real-engine semantic parity and rejection of failed, readable, invalid and fake-marker outputs.
- The extracted protected payload passed edition/license, investigation, rule scope, adaptive planning, reasoning, coverage, verdict safeguard, recovery, endpoint recommendation, collection-health and Wazuh wizard/bundle regression suites.
- Account and single-manager limit tests passed. Signed integrity/tampering checks and 8 isolated HTTP/CLI enforcement checks passed.
- All 361 PHP files passed Linux syntax checks. Linux edition/license and 51 guardrail assertions, including symlink substitution, passed. The complete signature/ownership audit reported zero unsafe paths.
- SQLite-dependent fixtures passed against the protected payload on the build host; the Linux test VM lacks the optional PHP SQLite driver. No production extensions were installed for tests.
- Final archive checks, protected-file hashes, private-value scans and SHA-256 checksums passed. No configured AI provider was called and no live SOC evidence was used.

Complete clean installation of this exact ZIP, an in-place deployment upgrade, representative verdict-accuracy benchmarking, a full penetration test and a comprehensive dependency/license audit remain separate tasks. An isolated runtime test does not replace them.

## Download and installation

Download **alistoir-community-v3.0.2-one-click.zip** and **SHA256SUMS.txt** from this release. Verify the checksum before extraction. GitHub's automatic Source code archives contain repository documentation, not the installer. Follow [INSTALLATION.md](INSTALLATION.md) for direct download on the Ubuntu VM.

This ZIP is a **fresh installer**, not an in-place upgrade script. Do not run it over an existing application/database. Preserve evidence, private runtime settings and certificates, and use an administrator-controlled, backed-up upgrade procedure.

An open-source license has not been selected; see [licensing status](../LICENSE_STATUS.md).
