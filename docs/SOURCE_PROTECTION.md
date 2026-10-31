# Source protection maintenance build 3.0.2

Official Community and Professional build pipelines now require the configured PHP control-flow obfuscator to complete successfully. A required failure aborts the build; there is no stripped-source fallback for required application logic. Critical license, AI service, autonomous agent and investigation files must contain real PHP control-flow protection and pass syntax checks. Community also requires protection of its integrity verifier.

## Exact scope

- Eligible first-party PHP logic uses YAK Pro control-flow obfuscation, with function/class/property identifiers preserved for compatibility.
- Mixed HTML/PHP UI templates and migration/documentation helpers are explicitly exempt and may only have whitespace/comments removed. They are not reported as obfuscated.
- Small declarative PHP files with no transformable control flow are reported separately.
- Third-party vendor PHP is preserved, including its notices.
- HTML, JavaScript, CSS, SQL, shell/PowerShell/Python integration scripts and configuration examples remain inspectable. This is not complete source encryption or an ionCube/SourceGuardian build. No proprietary PHP decoder extension is required.

## Integrity and verification

Each build checks final protected file hashes against its private protection report. Community signs the complete immutable application manifest only AFTER all Community overrides are obfuscated. Never change PHP after signing. The signing authority, obfuscator, private reports and build fixtures are excluded from installer assets.

Source protection supplements existing authorization, TLS, permissions and edition/license checks. It does not guarantee that an external AI will refuse editing, hide all business logic or prevent a privileged root owner from changing local enforcement. Security reviews may still report vulnerabilities. Maintain readable development source separately and ship only reviewed, validated builds.

Configuration belongs in supported UI/private runtime settings. Treat both installer ZIPs as fresh-install packages; an existing deployment needs a backed-up upgrade procedure. Runtime regression tests are separate from clean installation and live integration testing.
