# Autonomous Investigation Settings — administrator guide

Page: `/investigations/settings` on your AlistoIR server. Applies to Community 3.0.3.

## What is this page for?

This page controls **whether automatic investigation runs and which Wazuh Rule IDs it covers**. It is a platform-wide policy, not a filter for one analyst's screen or one alert. Only a **System Administrator** can access and change it.

Use [Investigation Center](INVESTIGATION_CENTER.md) to monitor and review investigations. Use this settings page to control their automatic scope.

![Illustration of autonomous investigation settings and Rule ID selection](../docs/images/06-autonomous-settings.png)

*Instructional illustration, not a screenshot of a live deployment. Examples are illustrative; actual layout may vary.*

## Controls on the page

| Control | What it does |
|---|---|
| Enable AI autonomous investigation | Enables or pauses the automatic investigation workflow. |
| Rule IDs | Selects all rules or a specific list of numeric Wazuh rule identifiers. |
| Save settings | Applies the switch and Rule IDs together. Changing a field alone does not save it. |
| Back to Investigation Center | Returns to the investigation workspace; use Save settings first to keep changes. |

## Enable AI autonomous investigation

### When enabled

The worker can discover matching alerts without an investigation, including eligible older alerts, and process allowed queued or active runs within its normal budgets and deadlines. Available Wazuh evidence, supported endpoint collection and AI assessment can participate in the workflow when their integrations and prerequisites are ready.

Enabling this switch does not configure a provider, repair an Indexer connection, deploy an endpoint collector or enable adaptive planning by itself. Collection and adaptive planning are shown as separate indicators in Investigation Center.

### When disabled

- New automatic investigations stop being started.
- Further automatic processing of existing runs pauses.
- Previously collected evidence remains available.
- Callbacks from tasks already dispatched are still accepted; turning the switch off does not recall those tasks.
- Manual AI tools and analyst decisions remain available, subject to their own permissions and controls.

**This is an automatic-investigation pause, not a master switch for every AI feature.** To prohibit external AI use, administrators must also review provider configuration and network policy. See [AI privacy](../docs/AI_PRIVACY.md).

Saving disabled settings does not change every run's displayed status to Cancelled. An investigation may retain Queued, Collecting or Evaluating while automatic work is paused. Read the collection indicator and rule scope as well as its status.

## Rule IDs

These are **Wazuh Rule IDs**, not AlistoIR Alert IDs, investigation references or MITRE technique IDs.

| Value | Meaning |
|---|---|
| `0` | All rules are in automatic scope. Use zero by itself. |
| `100901` | Only alerts with that rule identifier are in scope. |
| `100901, 61634, 92057` | Alerts matching any of these three rule identifiers are in scope. |
| `100901 61634 92057` | Space-separated input is also accepted. |

The rule numbers above are formatting examples, not a recommended detection policy. Choose IDs observed in your own Wazuh alerts.

### Input rules

- Enter `0` alone, or up to 100 numeric entries separated by commas or spaces.
- Do not combine `0` with specific IDs: `0, 100901` is invalid.
- Each identifier must contain 1–10 digits; names, ranges and wildcards are not accepted.
- Empty input is invalid, including when saving the switch as disabled. Keep `0` or your intended list in the field.
- Repeated IDs are consolidated, leading zeroes are removed and the saved list is normalized.

For example, `100901-100910`, `*`, `T1059` and `PowerShell` are not valid Rule ID entries.

Selecting `0` removes the rule restriction. It does not bypass alert eligibility, account access, telemetry requirements, supported checks, response approval, worker budgets or AI provider availability. It also does not guarantee an immediate or conclusive verdict for every alert.

### What happens when the list changes?

If you change from `0` to selected IDs, existing runs for other rules stop being processed automatically. Their evidence remains available and they wait until their rule is allowed again. Allowing the rule again lets the worker resume applicable work within its normal limits; it does not reset all deadlines or guarantee success.

Broadening the list can also bring matching older alerts without investigations into discovery. The change is not limited to alerts received after you press Save. Review scope carefully before switching to `0` in a busy environment.

Starting an existing alert from Investigation Center also respects the enabled state and Rule ID policy. It is not a way to bypass the settings.

## How to configure selected rules

1. Confirm that alert ingestion, Wazuh evidence access, endpoint collection where required and the chosen AI provider work with approved lab evidence.
2. Open a representative alert and identify its **Wazuh Rule ID**.
3. Sign in as System Administrator and open `/investigations/settings`.
4. Turn on **Enable AI autonomous investigation**.
5. Enter the selected numeric Rule IDs.
6. Select **Save settings** and confirm **Settings saved.**
7. Return to Investigation Center. Verify **Collection enabled** and the displayed Rule IDs.
8. Use one controlled alert to check queuing, returned evidence, gaps and AI output before widening scope.

The worker is normally scheduled every minute. Processing time depends on the queue, available evidence, provider latency and operating budgets. Saving settings is not an immediate run command.

## Common operating choices

| Goal | Switch | Rule IDs | Expected effect |
|---|---|---|---|
| Pilot a small rule set | On | Selected IDs | Matching eligible work runs automatically. |
| Cover all rules | On | `0` | All rules are in scope; normal collection and safety limits remain. |
| Pause automatic work | Off | Keep the intended valid scope | Existing evidence remains; dispatched callbacks may still arrive. |
| Pause one rule while allowing others | On | A list excluding that rule | Out-of-scope runs wait; other allowed work can continue. |

This page has no exclusion expression such as “all except one.” To exclude a rule, use an explicit list of the rules you want to allow.

## Verify the change

In Investigation Center, check the collection indicator and Rule IDs. Open a matching investigation and inspect actual evidence progress. Compare with a rule outside the configured list when validating selected-rule scope.

Do not use the presence of an old AI summary as proof that the new configuration has processed an alert. Compare timestamps, collection status and evidence. A successful save confirms the policy was stored; it does not prove every integration is healthy.

Changes are saved as a platform-wide policy and recorded through the application audit mechanism when available.

## Troubleshooting

| Symptom | What to check |
|---|---|
| Only a system administrator can change settings | Use an account with the System Administrator platform role. An ordinary analyst or tenant administrator is not sufficient. |
| Security validation failed | Refresh the page and submit again from the current signed-in session. Do not disable CSRF or integrity protection. |
| Rule ID validation error | Use digits only, keep `0` by itself and stay within the entry limit. |
| Unable to save settings | Ask the deployment administrator to confirm installation/migration and database health. |
| Collection enabled but nothing progresses | Check matching rules, alert eligibility, the scheduler, worker health and integration readiness. |
| Existing run stays queued after a rule-list change | Its rule may now be excluded. Restore the intended allowed scope if it should run. |
| Events still arrive after disabling | Previously dispatched task callbacks are still accepted; the switch does not recall endpoint tasks. |
| Adaptive planning is still paused | It is a separate capability; this page only saves collection enablement and Rule ID scope. |
| Result is Inconclusive | Review missing evidence. Selecting more rules or enabling automation does not create unavailable telemetry. |

Continue with [Investigation Center](INVESTIGATION_CENTER.md), [Configuration](../docs/CONFIGURATION.md), [Wazuh setup](../docs/WAZUH_SETUP.md) or [Troubleshooting](../docs/TROUBLESHOOTING.md).
