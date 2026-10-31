# AlistoIR Community 3.0.3

Official maintenance release, 31 October 2026.

## What's new

A mandatory outbound privacy gate now covers AIService requests to OpenAI, Anthropic, DeepSeek and configured local endpoints. It withholds recognized secrets, nested credential objects, identified raw logs and notes; pseudonymizes recognized identities, emails, IP addresses, domains and common absolute paths; and sanitizes JSON snapshots embedded in legacy prompts. Evidence references, technical field names, MITRE techniques, rule IDs and common hashes remain available. Aliases remain stable during repeated sanitization within the service context, and alias maps are not transmitted or logged.

The AI is instructed to explain privacy-related evidence gaps and avoid treating aliases as actionable IOCs. Invalid UTF-8, excessive nesting, unsupported message content and serialization failures block requests. Provider credentials remain in required authentication headers. See [AI_PRIVACY.md](AI_PRIVACY.md) for limitations: this is data minimization, not a guarantee of complete DLP or verdict accuracy. Threat-intelligence API lookups are outside this LLM boundary.

## Validation

- Both final protected editions passed 34 synthetic privacy checks and the existing investigation, settings, adaptive planning, reasoning, coverage, verdict safeguard, worker recovery, endpoint recommendation, collection-health and Wazuh wizard suites.
- Mock tests exercised all four actual provider transports: sanitized bodies, authentication retained and invalid content prevented from reaching execution. No actual provider requests or customer evidence were used.
- 33 source-protection checks passed without readable fallback. Community passed 12 signed-integrity checks, 50 Windows guardrail checks and 51 Linux guardrail checks, plus Linux privacy checks. The reference deployment passed signature/ownership checks and HTTPS login validation.
- The Community payload contains 362 PHP files, including 219 obfuscated files, with 469 immutable files signed. See [SOURCE_PROTECTION.md](SOURCE_PROTECTION.md) for deliberate exceptions.

## Download and installation

Download **alistoir-community-v3.0.3-one-click.zip** and **SHA256SUMS.txt** from the release assets. GitHub's automatic Source code archives contain this documentation repository, not the installer. Follow [INSTALLATION.md](INSTALLATION.md) for direct Ubuntu downloads. The installer is for fresh installations; existing deployments need a backed-up upgrade procedure. Do not run the fresh installer over an existing database.

---

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
