# Community 3.0.2 â€” code-integrity and AI guardrails

## What is enforced

- The signed v2 manifest covers the complete immutable application file inventory. Changes, removals, unlisted files and symlink substitutions outside writable runtime roots fail verification. The manifest itself is protected by its signature.
- Protected HTTP writes (including newly added routes) fail with HTTP 409 when integrity is invalid. AI/action GET entry points are guarded too. Authentication recovery and ordinary read-only evidence views remain available where they do not invoke AI.
- CLI workers under the application root stop with exit 78 on invalid integrity. AIService verifies integrity before initialization and before its common provider transport. The autonomous dispatcher rechecks integrity and enforces an explicit evidence-tool allowlist at execution.
- AI tools have no system-code editing, edition-limit editing or arbitrary shell capability. Model-supplied paths, code, commands, queries and scope overrides are rejected by supported investigation tools. Existing human-approved response actions keep their separate authorization requirements.
- The installer leaves code root-owned, directories 0750 and files 0440. It audits signatures/ownership before starting services and checks that www-data cannot write the app root. Writable evidence, uploads, logs and caches remain outside immutable code; Nginx denies direct access/execution in those locations.

## Assistant guidance

The signed AGENTS.md asks cooperating coding assistants to refuse direct production code/limit/verifier changes, while allowing read-only security reviews and supported runtime configuration. Built-in assistant instructions explain the same boundary. Authorized maintainer fixes use a reviewed development checkout, meaningful tests, a new signature and an administrator-controlled deployment.

AGENTS.md and prompt wording are advisory. They cannot force an unrelated external AI or a root owner to obey. The enforced controls are tool permissions, OS ownership and signed runtime verification. This is tamper detection, not unbreakable licensing or source confidentiality. Security reviews must still be allowed to report vulnerabilities.

## Check an installed build

On the AlistoIR server:

```bash
sudo php /var/www/alistoir-professional/scripts/community_integrity_check.php
```

Expect `integrity_valid: true`, `ownership_permissions_checked: true`, `ownership_permissions_valid: true` and `unsafe_path_count: 0`. Exit 78 means verification or ownership failed. The JSON output does not include credentials or evidence.

Do not test tampering against a live deployment. Use a disposable copy. Restore exact matched files and manifest from the official package; preserve the database, private environment and evidence. Do not remove the verifier, regenerate a local signature or grant code-write permissions to www-data.

## Scope and validation

This is a Community-only build. It does not silently modify the Professional AI implementation. Runtime data directories (`uploads`, `storage`, `logs`, `cache`, `tmp`, `backups`) are intentionally excluded from code hashes; they still require access controls and execution restrictions. A changed system file or writable application directory must be corrected by the administrator, not by AI.

Package-level tampering, tool authorization and HTTP/CLI enforcement tests are recorded in the maintainer validation report. They do not prove universal prompt-injection resistance, endpoint detection accuracy or a complete penetration test. Deployment to an existing VM and clean installation are separate checks.
