# Reports — complete reporting guide

Page: `/reports/index`. Applies to AlistoIR Community 3.0.3. The Report Center requires an eligible manager/administrator account and the applicable deployment feature permissions. Linked workflows enforce their own access checks.

The Report Center brings together report history, case reporting, alert coverage, audit activity, analyst case monitoring, SLA summaries and recorded threat hunts. Use it to review operational activity and prepare evidence-supported management/client updates.

Opening this page reads stored data. It does not start autonomous investigations, collect endpoint evidence or generate every report automatically. Counts reflect available records and access rules; they are not proof of complete telemetry or incident-detection accuracy.

## 1. How to use the Report Center

1. Open **Reports** and choose **Period**.
2. For custom dates, choose **Date Range**, enter Date From and Date To in ascending order, then select **Apply**.
3. Review the KPI cards and the detailed tables. Check their date bases below before comparing counts.
4. Use **Case Reporting** to open a case or generate its PDF, or **Security Posture** for a separate executive/metrics report.
5. Review **Generated Reports** to open a saved snapshot, regenerate a case report or download a stored file when available.
6. Verify the report's dates, evidence, verdicts and limitations before sharing it.

For example, to prepare a weekly review, select This Week, review new alert volume and case linkage, inspect SLA breaches and analyst case records, then generate relevant case PDFs. This Week is a calendar week, not the previous rolling seven days.

## 2. Period and date controls

| Control | Meaning |
|---|---|
| Today | Current application-local calendar day. |
| This Week | Monday through Sunday of the current calendar week. |
| This Month | First through last day of the current calendar month; the default. |
| Date Range | Custom calendar dates, including the start and end days. |
| Apply | Reloads the center using the selected period. |
| Reset | Returns to the default current-month view; it does not delete records. |

The application converts local date boundaries to UTC for its queries, using the configured application timezone. Alert dates here normally describe received/created time, not the original endpoint event time.

If only one custom date is supplied, it is used for both endpoints. Enter valid dates in ascending order: although the implementation attempts to swap reversed timestamps, reversed calendar inputs can produce unexpected day boundaries. Empty/invalid ranges can fall back to an internal current-period range; do not treat a displayed blank date as an intentional all-time report.

Applying the filter changes the Report Center. It does not impose the same window on every linked page: Case Reports carries date parameters, while Security Posture, SLA Monitoring, Threat Hunt Activity and Executive Snapshot open their own workflows. Recheck filters after navigating.

## 3. KPI cards

| Card | What is counted / time basis |
|---|---|
| Reports Generated | Report registry records by generated time, falling back to created time. This is a record count, not necessarily successfully delivered PDFs. |
| Alerts In Period | Non-deleted stored alerts created in the selected period within the applicable scope. |
| Audit Events | Visible recorded audit-log events whose audit timestamp falls in the period. |
| Open Cases Created | Cases created in the period whose current status is neither closed nor false_positive. This is not the entire outstanding backlog. |
| Closed Cases In Period | Cases currently closed or false_positive, selected by the first available closed/resolved/updated/created timestamp. |
| SLA Breaches | Cases created in the period with a stored response or resolution breach flag. A case breaching both is normally one case in this card. |
| Users In Audit Trail | Distinct non-null user IDs represented by visible audit events, not all accounts or concurrent users. |
| Login / Logout Events | Audit actions exactly classified as login or logout; not all authentication attempts or endpoint logons. |
| Case / Alert Changes | Audit events targeting a case or alert. This is not a count of distinct incidents or necessarily only field edits. |

The cards do not all describe the same cohort. For example, a case created last month and closed this week can contribute to Closed Cases In Period but not Open Cases Created for this week. Status reflects current stored state, not a reconstructed historical end-of-day snapshot.

SLA flags depend on tracking data being populated and updated. A zero breach count does not prove every case had a policy or that every target was measured correctly.

## 4. Quick-action tiles

| Tile | Use |
|---|---|
| Security Posture | Open the dedicated executive/metrics posture report generator and saved snapshots. |
| Case Reports | Open the case listing with the selected date parameters; inspect its filters and generate per-case reports. |
| SLA Monitoring | Review operational SLA state in its own workspace. |
| Threat Hunt Activity | Open basic hunting and recorded hunt workflows. |
| Executive Snapshot | Open the dashboard for operational charts and current SOC context. |

These are navigation shortcuts, not one-click exports of the entire Report Center.

## 5. Generated Reports

This table shows up to **15 most recent report registry records** in the period. The full Reports Generated card can exceed the visible rows.

| Column | Meaning |
|---|---|
| Report | Stored title/code identifying the generated record. |
| Type | Stored report classification. |
| Target | Related entity; a case target can link to its case. |
| Format | Registered output format; it does not guarantee a file remains on disk. |
| Status | Stored generation state, separate from case status or investigation verdict. |
| Generated By | Recorded generator/author, with a system fallback. |
| Generated | Generation timestamp or creation fallback. |
| Downloads | Recorded download events, with last-download age when available. |
| Action | Open, Regenerate, Download or disabled No File, according to the current route. |

**Open** takes supported security-posture records to their saved snapshot. **Regenerate** creates a case report from current case data; it is not a guaranteed byte-for-byte copy of the earlier report. **Download**, when offered and authorized, retrieves a stored file. **No File** means no supported file action is available for that row.

Case reports can be streamed directly rather than retained on disk. Their registry entry can remain after the download. Save the PDF when generated if you need the exact version for a client or audit.

The current center can route case rows to Regenerate even when older stored-file metadata exists. Do not infer permanent file storage or deletion solely from that button. Missing paths/files, changed access or an unavailable target can also prevent download; ask the administrator to check the record rather than repeatedly generating replacements.

Downloads are application audit events, not unique readers, proof of client receipt or public GitHub installer statistics. Repeated downloads can increase counts, and generation/streaming paths need not be counted identically.

## 6. Case Reporting and case PDFs

The case table lists up to **12 newest cases created in the reporting period**, with Case, Status, Severity, Assigned, Alerts, Artifacts, Created and a report action.

1. Open the case number and confirm its identity, scope and linked evidence.
2. Review analyst resolution, AI recommendation freshness, tasks, assets and timeline. Resolve inconsistencies before presenting the report as final.
3. Use the row's report/PDF action or the case's report control.
4. Save and review the downloaded document before sending it.

Alerts counts refer to eligible linked alerts, while Artifacts counts refer to case-associated artifact records. Neither measures evidence completeness or the number of confirmed malicious IOCs. A case can have multiple alerts without representing multiple incidents.

The case PDF uses available stored case context, including assessment/verdict context, SLA, assets, linked alerts, observables, tasks, comments and history. It is a summarized report, not every raw event or attachment. Current bounded sections include up to 15 linked alerts, 10 assets, 20 artifacts, 15 evidence records, 20 tasks, 12 related-alert suggestions, 30 comments and 40 history entries. Refer to the original case for omitted material.

A report export does not mean a new endpoint investigation or fresh AI analysis ran. A stored AI recommendation is separate from the analyst's recorded resolution; confirm both and acknowledge missing or stale evidence.

## 7. Alert Severity Summary

| Column | Interpretation |
|---|---|
| Severity | Stored detection severity. |
| Total Alerts | Eligible alerts created in the period for that severity. |
| New | Those currently carrying status new. |
| Linked To Case | Alerts with at least one case-alert relationship; an alert is counted once here even if linked to several cases. |
| Coverage | Rounded percentage: linked alerts / total alerts × 100. |

**Coverage means case linkage**, not sensor coverage, MITRE coverage, completed investigation, analyst review rate or AI accuracy. A relationship can exist to an older case; it is not restricted to cases created within the same period. Some alerts can be investigated without requiring a separate case.

## 8. Audit activity and action summary

The recent audit table shows up to **15 newest visible events**: Time, User, Action, Target and IP. The action-summary table shows up to **eight most frequent action labels** in the period.

Use these for traceability: who performed a recorded action, against which target, and when. The IP is the address recorded by the application for that request, not necessarily an attacker endpoint. System events may have no named user.

These are bounded views of application audit records, not the full Wazuh event archive or an independently immutable compliance ledger. Actions with missing audit instrumentation cannot be reconstructed from this screen. A frequent action is not inherently suspicious, and an audit event can represent a request rather than verified business completion.

## 9. Analyst Case Monitoring and Analyst Audit modal

The table shows up to **12 assignees** with eligible, non-deleted assigned cases. It is sorted by Closed cases, then In Progress, Open, Total and name. It is a workload snapshot, not a reliable ranking of analyst quality or time spent.

Unlike the Case Reporting table, its period uses the first available **closed_at, resolved_at, updated_at, created_at** timestamp. This is a fallback chain, not necessarily the chronologically greatest timestamp. Unassigned cases are omitted. Current assignee attribution is not proof that the analyst performed every historical action.

| Column | Meaning |
|---|---|
| Analyst | Current assignee; click the name to open Analyst Audit. |
| Total | Eligible assigned cases in this activity-based period. |
| Closed | Status exactly closed, with false-positive-status count shown separately when present. |
| Open / In Progress / Pending | Counts for those exact current statuses. |
| SLA annotation | Cases with either stored response or resolve breach flag. |

In **Analyst Audit**, review Total/Open/In Progress/Pending/Closed/SLA Breached, priority distribution, resolution badges and up to **30 most recent assigned case rows** for that analyst in the same period. Case rows include status, severity, priority, verdict, SLA and last activity, with links back to the case.

Priority colors are P1 red, P2 yellow, P3 blue and P4 gray. Lifecycle badges commonly show Closed green, Open yellow, In Progress blue and Pending cyan. Colors organize the view; read the text because a green lifecycle status is not proof of benign activity.

### Important verdict-count limitation

The modal's current aggregate classifies explicit true_positive resolutions **and closed cases with no resolution set** as True Positive. Those blank-resolution closed cases can also appear under Not Set. False Positive can come from the resolution or false_positive status. Benign, Informational and Under Investigation use their respective resolution values.

Therefore, the badges are not guaranteed mutually exclusive or independently confirmed ground truth. **Do not use these aggregates to calculate AI accuracy or a validated true-positive rate.** Open the cases, explicitly record the correct resolution and verify the underlying evidence before reporting a final verdict distribution.

## 10. SLA summary

The SLA table groups cases created in the selected period by priority. It shows Total, Response Breach and Resolve Breach, using stored tracking flags.

A case can contribute to both breach columns, so adding them can double-count cases compared with the SLA Breaches card. Red breach counts indicate recorded target failures, not necessarily a confirmed security incident. This is case SLA reporting, not the alert-level Autonomous Investigation queue or Alert SLA workspace.

Use **SLA Monitoring** and individual cases to review policies, deadlines, live/pending timers and actual response/resolution evidence. Missing tracking should be investigated separately from a genuine on-time result.

## 11. Threat Hunt Activity

The hunt table shows up to **10 newest recorded basic hunts created in the period**, including shortened search term, type, Active/Completed state, stored hits and owner/time. Completed is green; other states are shown with warning styling.

Hits are the stored result count from that hunt workflow, not confirmed compromises or a fresh search of current data. Active does not mean a continuous scan. Completed does not close cases or remediate endpoints. Advanced saved queries and AI chat requests are not automatically equivalent to basic threat_hunts records.

For collection limits, reused active history and completion notes, see [Threat Hunting](THREAT_HUNTING.md). For advanced query construction, see [Advanced Hunt](ADVANCED_HUNT.md).

## 12. Generate a Security Posture report

The **Security Posture** tile opens a separate report workflow with its own permissions and date selection:

1. Choose Report Mode: **Executive Operational Security Posture** or **Cybersecurity Metrics & Trends**.
2. Choose Quarterly, Monthly, Annual or Custom Range; enter dates for Custom Range.
3. Check the reporting period on that page. It does not automatically inherit the Report Center dates.
4. Select **Generate Report**, review the saved snapshot, then use its PDF export when needed.

The workflow summarizes available operational metrics such as incident trends, response/SLA performance, recurring risks and affected assets, with a narrative. Where configured, AI can generate that narrative; a deterministic fallback can be used when AI is unavailable. Report generation and AI analysis are not the same as running new endpoint investigations.

This is a stored posture snapshot rather than a live dashboard. A future generation can differ as records or assessments change. Validate metric definitions, data gaps and recommendations before using it as a formal assurance statement. The center itself does not provide a global Export CSV/PDF button or configure scheduled email delivery.

AI requests use configured integrations and the shared 3.0.3 privacy layer for supported outbound context. Downloaded reports can still contain private SOC information; provider sanitization does not automatically sanitize local PDFs or attachments. See [AI privacy](../docs/AI_PRIVACY.md).

## 13. Reporting limits and troubleshooting

The center's bounded tables have no full-history pagination here: Generated Reports 15, Case Reporting 12, recent audit events 15, action categories eight, analysts 12, analyst case details 30 each, and hunts 10. KPI/aggregate counts can cover more records than the tables show. Use the underlying workspace and narrower dates for further authorized review.

| Symptom | What to check |
|---|---|
| Access denied / plan redirect | Manager/admin role, tenant context and deployment feature permissions. |
| No generated reports | Generation time versus requested dates, report registry creation and target access; cases existing does not automatically create report entries. |
| Counts do not reconcile | Different date bases, current statuses, access rules, case relationships, stored SLA data and table caps. |
| Analyst missing | Case assignment, latest-activity fallback timestamp, access scope and the top-12 limit. |
| True Positive seems inflated | Blank-resolution closed-case fallback; verify and explicitly resolve individual cases. |
| Coverage low | Case-linkage percentage is low; inspect whether cases are appropriate rather than treating it as missing telemetry automatically. |
| Regenerate instead of Download | Streamed case workflow or current action routing; regenerated content can differ. |
| Missing file / No File | Report metadata, target availability, managed file storage and access permissions. |
| PDF omits evidence | Section caps and summarized content; inspect the original case and supporting records. |
| Report data error | Ask the administrator to inspect server logs/schema; partial/default-zero panels after an error are not valid clean findings. |
| Security validation failed in generation | Refresh the signed-in form and retry; do not disable security checks. |
| Posture generation unavailable | Check access, report schema, configuration and returned error; do not assume an AI/provider failure is the only cause. |

Before client delivery, confirm audience, dates/timezone, case references, explicit verdicts, evidence limitations and sensitive content. Preserve the exact exported artifact when an audit requires a fixed version.

Related guides: [Dashboard](DASHBOARD.md), [Alerts](ALERTS.md), [Alert View](ALERT_VIEW.md), [Investigation Center](INVESTIGATION_CENTER.md), [Threat Hunting](THREAT_HUNTING.md) and [Troubleshooting](../docs/TROUBLESHOOTING.md).
