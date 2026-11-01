# Playbooks — automation and response guide

Page: `/playbooks/index` (also `/playbooks`). Applies to AlistoIR Community 3.0.3. Visibility and management/run/approval actions depend on your account's role and record scope.

## What is a playbook?

A playbook is a saved sequence of supported actions for repeatable triage, case handling, notifications or endpoint response. It combines **when to run**, **which record/endpoint it concerns**, and **what ordered steps to perform**.

Playbooks are different from autonomous AI investigation. The investigation engine gathers evidence and assesses readiness; a playbook executes configured procedures. A playbook does not automatically obtain a correct AI verdict, and an AI recommendation does not automatically authorize a playbook response.

| Concept | Meaning |
|---|---|
| Playbook definition | Saved name, trigger, conditions and steps. |
| Active / Inactive | Whether the definition is enabled for use; not whether a run is executing. |
| Queue item | Requested work awaiting processing or approval. |
| Execution | A particular run, with status and logs. |
| Endpoint result | Separate confirmation that a dispatched command actually achieved its intended effect. |

## 1. Community limit: five active playbooks

Community permits **a maximum of five active playbooks** across the enforced inventory, not five per category or filtered screen. Existing inactive drafts do not consume an active slot. A hidden/internal definition can also consume a slot if active.

At capacity, **Add Playbook** and activation controls for inactive definitions are disabled. To enable another, pause an existing active playbook first. The server/database also enforce the limit; changing browser controls does not bypass it.

Default Community templates are seeded as inactive drafts. Review their targets, integrations and effects before activating. The five-playbook limit concerns enabled definitions, not five execution-history rows.

## 2. Page layout and permissions

| Area | Purpose |
|---|---|
| Summary cards | Inventory, enabled/manual definitions, pending/running execution and approval counts. |
| Filters | Category, Trigger Type, Status and Search Name. |
| Destructive Action Approval Queue | Visible to permitted managers when approval storage is ready. |
| Playbook catalog | Definitions, action counts, status and available controls. |
| Recent Executions | The latest 12 execution records in scope. |
| How Playbooks Work / details | Context about usage and saved configuration. |
| Create/Edit/Run/Pause/Delete dialogs | Separate management or execution actions. |

Definition management is permission-controlled; manual-run eligibility is also checked separately. An ordinary analyst/client may be able to read information without seeing every manager control. Missing controls can be expected, not a page failure.

## 3. Summary cards

| Card | Meaning |
|---|---|
| Total Playbooks | Definition records in the relevant scope. |
| Active Playbooks | Enabled definitions, not currently executing runs. |
| Manual Trigger Playbooks | Definitions with Manual trigger, including inactive ones. |
| Pending / Running Executions | Execution records in those processing states. |
| Awaiting Approval | Queue items waiting on approval, shown when the workflow is available. |

Cards are not narrowed by every catalog filter. Approval counts and execution counts come from different records and need not match. A zero backlog is not proof that every integration is healthy.

## 4. Filters and catalog columns

Filters combine. Select **Filter** to apply them or **Reset** to restore the default catalog within your scope.

| Filter | Meaning |
|---|---|
| Category | Organizational classification such as Endpoint, Network, Identity, Email, Cloud, Threat Intel, Malware, Containment, Validation, Triage or Other. |
| Trigger Type | Manual, Alert, Schedule or API. |
| Status | All, Active or Inactive definitions. |
| Search Name | Partial matching on playbook name, not a search of all step parameters or logs. |

The catalog orders active definitions before inactive ones, then category/name.

| Column | Contents |
|---|---|
| Playbook | Name and shortened description. |
| Category | Classification, not a guarantee of integration readiness. |
| Trigger | How the playbook is intended to be invoked. |
| Severity | Saved/default severity or Any; do not confuse it with execution outcome. |
| Actions | Number of configured steps, not a successful-step count. |
| Status | Active or Inactive. |
| Last Run | Most recent recorded start, or Never. It does not prove success. |
| Controls | View details, Edit, Pause/Activate, Delete and eligible Manual Run. |

### Colors

- **Active:** green; **Inactive:** gray.
- **Category:** gray; **Trigger:** cyan/info; **Actions count:** blue. These are visual groupings, not risk scores.
- Saved severity uses the alert-style labels: Critical red, High yellow, Medium blue, Low green.
- Execution **Pending:** yellow; **Running:** blue; **Completed:** green; **Failed:** red; **Cancelled/unknown:** gray.

Always read the label: a green Active badge is not a green Completed execution, and neither certifies endpoint remediation.

## 5. View details, pause, activate or delete

**View details** shows name, category, trigger, version, description and configured actions. Inspect this before running a template or unfamiliar definition.

**Pause / Activate** opens a confirmation. Pausing disables the definition and frees an active Community slot; it does not undo actions already performed or recall dispatched endpoint tasks. Do not assume it cancels every queued/in-flight request—inspect queue/execution state separately. Activation enables future use under its trigger and permissions; it does not itself run the procedure immediately.

**Delete** is different from Pause. Deletion is blocked while associated queue/execution records are pending or running. It can remove the definition and associated queue records; do not assume every historical item remains available through normal catalog/history joins. Preserve required operational records and choose Pause when you simply need to stop new use or free a slot.

## 6. Create or edit a playbook

| Field | Purpose |
|---|---|
| Playbook Name | Required identifying name; creation form allows up to 150 characters. |
| Status / Active | Whether the definition is enabled; Community quota applies. Save an inactive draft during preparation where the UI permits. |
| Category | Organizational grouping. |
| Trigger Type | Manual, Alert (Auto), Schedule or API. |
| Default Severity | Saved severity/default metadata. Use explicit trigger conditions for alert selection rather than assuming this field is the filter. |
| Trigger Conditions | Alert-selection conditions; the create form provides a visual condition editor, while Edit can expose JSON. |
| Run Every / Schedule Interval | Supported schedule cadence, used by the background scheduler. |
| Description | Purpose, prerequisites, expected effects and limitations. Creation form allows up to 2,000 characters. |
| Playbook Steps | Ordered actions and their parameters. |
| Available Variables | Runtime placeholders for alert, case and execution context. |
| Advanced actions JSON | Supported structured action input; not arbitrary executable code. |

Changes affect future use of the definition; they do not rewrite evidence of already-performed actions or guarantee all queued work uses the new version. Review pending work before editing an active response procedure.

## 7. Trigger types

| Trigger | Behavior and prerequisites |
|---|---|
| Manual | An authorized user explicitly runs an active manual playbook. The catalog Run button appears for that combination. |
| Alert (Auto) | Ingestion/runtime hooks evaluate active alert-trigger definitions and queue matching work. Requires explicit supported conditions and functioning processing. |
| Schedule | Background scheduling queues work according to the interval; requires the playbook queue scheduler/worker. |
| API | Intended for authorized integration/API invocation. Merely selecting API does not create an unauthenticated public execution endpoint. |

### Alert conditions

Supported selectors include **severity, rule_id, agent_group and source**. Different supplied fields must match together; severity/rule lists can contain alternative allowed values. Group/source matching can use the configured string context, so verify against actual ingested fields.

Some form help suggests leaving conditions empty to match all alerts. **Do not rely on that wording:** the current runtime skips definitions without usable trigger conditions. Configure explicit conditions and verify with a controlled lab alert. A selected Default Severity is not a substitute for those conditions.

This scope is separate from the Rule IDs configured under [Autonomous Investigation Settings](INVESTIGATION_SETTINGS.md). Pausing autonomous investigation is not a master switch for all playbook triggers or integrations.

### Schedule intervals

Options are **Hourly, Every 6 hours, Every 12 hours, Daily and Weekly**. They are interval-based scheduling choices, not a promise to run at an exact wall-clock time. Worker availability, approval and backlog can delay execution.

Scheduled work does not necessarily have an alert/case context. Steps requiring a specific record can fail or be skipped if no valid context exists. Do not guess record IDs to make a schedule execute.

## 8. Templates and visual step builder

Starter flows include **Alert Triage, Phishing Response, Malware Containment, Endpoint Isolation, Network Block, Notify & Escalate**, and **Start blank**. Templates are starting points, not validated policy for every client environment. They can contain mutating or external-notification steps.

Select an action type and **Add**. Configure each step, use **↑ / ↓** to reorder, and remove unwanted steps. Steps execute top-to-bottom; put context-producing work, such as Create Case, before actions needing that case.

A warning/skip is different from an exception. **Continue if step fails** logs an error and allows later steps to continue where supported; without it, an exception normally stops subsequent steps. It does not roll back earlier successful actions. A final Completed record can therefore still contain skipped steps or tolerated errors—read the execution log.

Advanced JSON is used by the create path when no visual-builder steps are supplied. Despite summary wording about overriding the builder, do not assume pasted JSON replaces non-empty visual steps. Use one clear source of configuration and inspect saved details.

## 9. Available action types

| Action | What it does / important context |
|---|---|
| Notify | Sends an in-app or configured external notification. Targets can include current user, managers, case assignee/creator, analyst role or configured external channels. Availability and delivery depend on configuration. |
| Update Alert Status | Updates the context alert to a supported state. This is a real workflow change, not an AI recommendation. |
| Create Case | Creates managed case work with supported assignment/default fields and optional source-alert linkage. Verify duplicates and resulting case. |
| Add Case Comment | Adds a comment to the context case. An Internal option, where offered, concerns supported export visibility, not universal secrecy. |
| Add Artifact | Creates/links a supported observable with type/value/risk context. A stored risk label is not independent verification. |
| Update Case Status | Changes the context case's workflow state; review closure evidence and policy first. |
| Assign Case | Uses a supported assignment mode, such as current user or least-loaded analyst. |
| Set Case Priority | Applies P1/Critical through P4/Low priority where supported. |
| Add Case Tag | Adds categorization to the context case. |
| Webhook | Sends a supported HTTP request to a validated destination with configured method/body. Review data and destination before enabling. |
| Wazuh Active Response | Dispatches a configured Wazuh response command using endpoint/manager context and validated inputs. Dispatch is not proof of endpoint success. |
| Isolate Host | Requests the supported endpoint-isolation path. Verify scope, approval, execution and preserved management access. |
| Release Isolation | Requests supported restoration of isolation state. Verify connectivity/result afterward. |

The builder's allowed actions do not authorize arbitrary shell commands, arbitrary record access or unsupported action names. Required integrations, permissions and target validation remain in force.

External notification/webhook content is **not automatically sanitized by the external-AI privacy gate**. Do not place credentials in message templates or URLs. Provider delivery and response action results require their own verification.

## 10. Runtime variables and execution context

| Context | Example variables |
|---|---|
| Alert | `{{alert_id}}`, `{{alert_severity}}`, `{{rule_id}}`, `{{rule_description}}`, `{{src_ip}}`, `{{dest_ip}}`, `{{agent_id}}`, `{{agent_name}}` |
| Case | `{{case_id}}`, `{{case_number}}`, `{{case_title}}`, `{{case_severity}}` |
| Execution | `{{execution_context}}`, `{{triggered_by}}`, `{{timestamp}}` |

Values exist only when the execution context supplies them or an earlier step creates them. Missing placeholders can become empty or lead to failed/skipped actions; they are not automatically inferred. Templates can disclose real client data through external destinations, so review resolved values and audience.

## 11. Run manually

1. Open and review an **active Manual-trigger** playbook you are authorized to run.
2. Select its Run button.
3. Check its name and enter **Execution Context**.
4. Reference the correct internal record where required, for example `alert:123` or `case:45`—formatting examples only, not real records to use.
5. Submit once and read the returned queue/approval/execution outcome.
6. Open history and verify per-step and endpoint results.

Referenced alerts/cases must be accessible to your account. A recent pending/running queue item for the same playbook can block repeat manual submissions (the current guard checks a five-minute window). Do not repeatedly click Run to bypass delays.

Submitting does not guarantee immediate execution. An inactive definition, missing queue backend, approval or integration problem can prevent completion.

## 12. Destructive Action Approval Queue

Visible to permitted managers when approval storage is available. The panel shows **Playbook, Requested By, Queued, Context and Approval Action**, with an optional note up to 500 characters.

Recognized destructive operations, including isolation/release and relevant blocking/response configurations, are routed through the supported approval policy. Approval detection is action/configuration-dependent; do not assume every harmless-looking name is non-destructive or every arbitrary action has a human gate. Inspect actual configured steps and the returned approval state.

- **Approve & Execute:** Authorizes and attempts execution. It is not a dry run; verify target, requestor, context and effects first.
- **Reject:** Records rejection and cancels the queued request. Associated execution reporting can show Failed with rejection context.

The approval panel shows up to **25** oldest pending requests, while the summary count can include more. No visible queue is not proof of a universal approval bypass or that no work exists; check role, schema and full workflow state.

A chat message saying approved, an AI recommendation or a green Active badge is not the application's approval action. An approved request can still fail during execution.

## 13. Recent Executions and history

The main page shows the latest **12** execution records in scope, with **Playbook, Status, Started, Finished and Context**. Review the context/log details offered for a run. Do not infer success from Last Run alone.

The dedicated `/playbooks/history` workspace supports playbook, status and execution-type filters with paginated records (normally 25 per page). Record-access checks can further restrict visible context.

| Execution status | Meaning |
|---|---|
| Pending | Recorded but not executing/completed yet; inspect queue and approval state. |
| Running | Processing state; a stale record can require worker/integration investigation. |
| Completed | The execution path finished, possibly with skipped steps or continued errors. Verify intended business/endpoint effects. |
| Failed | A blocking error or recorded failure occurred; earlier steps may already have changed state. |
| Cancelled | Work was cancelled where applicable; not a reversal of previously performed actions. |

A playbook dispatching Wazuh work can finish before independent endpoint validation. Use response/task reports and [Alert View](ALERT_VIEW.md) validation tools to verify remediation.

## 14. First-use workflow

1. Prepare an inactive, narrow-scope procedure and review its targets/integrations.
2. Start with a low-impact lab flow, such as a case comment or in-app notification, using a valid authorized context.
3. Check step ordering, variables, failure policy and approval behavior.
4. Activate only when a Community slot is available.
5. Run or generate a controlled matching event; inspect queue and history.
6. Verify output in the actual alert/case/endpoint workflow.
7. Document limitations and ownership before widening automation.

Do not use a containment template as a harmless connectivity test. Testing one flow does not certify every action, endpoint platform or client environment.

## 15. Troubleshooting

| Symptom | What to check |
|---|---|
| Add Playbook / Activate disabled | Five active Community definitions may already occupy the shared quota. Pause one first; filtered counts can hide other active definitions. |
| No Run button | Manual Run requires an active Manual-trigger definition and eligible role. |
| No Edit/Delete/approval controls | Management/approval permission or workflow availability. |
| Empty alert conditions never trigger | Configure explicit supported conditions; empty configurations are skipped by current runtime. |
| Scheduled playbook does not run | Interval, queue scheduler/worker, active state, context and approvals. |
| Pending for a long time | Worker health, queue backend, approval and integration dependencies. |
| Duplicate run warning | A recent pending/running request exists; inspect it before retrying. |
| Template variables empty | Missing alert/case context or wrong step order. |
| Completed but no expected effect | Inspect skipped/tolerated errors, destination delivery and endpoint results. |
| Partial side effects after Failed | Earlier steps are not automatically rolled back. Review actual records before retrying. |
| Cannot delete | Pending/running queue or execution records can block deletion. |
| Security validation failed | Refresh the signed-in page and retry; do not disable CSRF/integrity controls. |
| Queue backend/schema warning | Administrator must complete supported installation/migration, not edit protected code to suppress the warning. |

Continue with [Alert View](ALERT_VIEW.md), [Investigation Center](INVESTIGATION_CENTER.md), [Investigation Settings](INVESTIGATION_SETTINGS.md), [Chat](CHAT.md), [Configuration](../docs/CONFIGURATION.md) and [Troubleshooting](../docs/TROUBLESHOOTING.md).
