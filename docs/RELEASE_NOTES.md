# AlistoIR Community 3.0.0

Official Community release, 27 October 2026.

## Included

Wazuh-focused autonomous investigation queues and configurable rule-ID scope; aligned alert and case AI assistance; evidence gaps and analyst-final recommendations; Wazuh Setup Wizard; Community quota enforcement and signed integrity checks. See the README for limits and configuration guides.

Security changes include Nginx internal-path protection before PHP handling, CLI-only maintenance entry points, IOC HTTP destination/TLS/redirect controls, and exclusion of development tests from installed runtime files.

## Validation and limits

The rebuilt package passed archive checks, selected PHP syntax checks, 12 signed-integrity assertions, 87 autonomous investigation regression assertions and 20 settings assertions. The final release ZIP preserves that validated runtime unchanged. A credential/host-marker scan and runtime file inventory were checked before publication; the forwarder env file contains placeholders only.

The additional clean Ubuntu VM booted but was unreachable through SSH, so a fresh installation of this exact package remains pending. Previous VM deployment checks do not replace this test. The temporary VM was stopped and the Professional VM restarted. No representative verdict-accuracy benchmark, full penetration test, comprehensive dependency/license audit or production certification is claimed.

This official release promotes the RC1 application runtime without runtime code changes; the installer ZIP includes updated release documentation and its own checksum. The application uses some historical Professional directory/environment names in both editions.

## Download

Use alistoir-community-v3.0.0-one-click.zip and verify it against SHA256SUMS.txt from the same release. GitHub Source code archives contain documentation only. Installation starts on a dedicated Ubuntu VM; do not overwrite an existing deployment.

## Licensing

An open-source license has not been selected. See LICENSE_STATUS.md and THIRD_PARTY_NOTICES.md. Official release status does not assert an open-source license grant.
