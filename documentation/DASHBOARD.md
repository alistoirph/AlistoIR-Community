# Dashboard — complete analyst and client guide

Page: `/dashboard/index` on your AlistoIR server. Applies to AlistoIR Community 3.0.3. Some infrastructure sections require platform-level permissions and will not appear for every account.

## What the dashboard is for

The dashboard provides an operational overview of security alerts, case workload, response performance, documentation quality and platform health. Use it to decide **what needs attention and where to investigate next**.

An alert is a detection record, a case is managed incident work, and an autonomous investigation is the evidence-collection and assessment workflow. Their counts and statuses are different. Open [Investigation Center](INVESTIGATION_CENTER.md) to review autonomous investigation evidence and analyst dispositions.

The dashboard is not a declaration that the environment is secure, an AI accuracy report or a list of confirmed attacks. Read the underlying records before making an incident decision.

## Navigation, scope and freshness

- Counts are scoped by the signed-in account's access permissions. Two users can legitimately see different totals.
- Each section has its own time window. The page does not apply one universal date range to all metrics.
- Hover over the question-mark icons for short explanations. Charts provide details on hover where supported.
- **Show / Hide** expands or collapses a group without deleting data or pausing processing. The browser remembers group preferences locally.
- Linked KPI cards, workload counts, alert references and case references open supporting views. Some cards open a dialog instead of navigating.
- The **Live alert sync** countdown indicates a browser refresh request. The normal active-page interval is about 30 seconds, slowing to about 60 seconds when idle and 120 seconds when hidden; errors can add delay. The first scheduled request is earlier.
- Refreshing the browser is not the same as rerunning ingestion, enrichment or an investigation. Some aggregates are cached; configured cache lifetime and worker schedules can delay changes. Not every panel is necessarily replaced by each live refresh.
- **Today** uses the application's configured local day. Other windows and stored timestamps can use different boundaries. Check the underlying timestamp and timezone when comparing reports.

Zero or No Data can mean an empty dataset, an unavailable dependency, restricted scope or insufficient qualifying records. A zero average does not prove instantaneous service, and a zero percentage is not meaningful without its denominator.

## 1. Main KPI cards

| Card | What it counts | What happens when selected | How to use it |
|---|---|---|---|
| Alerts Today | Non-deleted alert records created during the current application-local day, within scope. | Opens today's alerts. | Monitor ingestion volume. A sudden drop also warrants checking the forwarder and connectivity. |
| Open Cases | Cases whose status is specifically Open. It does not include every In Progress or Pending case. | Opens cases filtered to Open. | Identify newly opened work; use case monitoring for the wider workload. |
| Critical / New | Critical-severity alerts still marked New, not restricted to today. | Opens Critical + New alerts. | Prioritize triage; critical severity is not itself confirmation of compromise. |
| Online Analysts | Active users with recorded activity in the last two minutes, within user scope. | Opens the online-user dialog. | Check recent application presence. This is not proof that someone accepted a particular alert or is available for duty. |
| Unenriched Alerts | Alerts from the last 30 days not yet checked against IOC feeds. | Opens the manual IOC re-check page. | Review enrichment backlog. Re-checking may also rerun enrichment on previously processed alerts; inspect the action before submitting it. |

The Online Analysts label describes application presence; the underlying count uses active user records and is not a staffing or shift-roster measurement.

## 2. Attack Source Map

**Window: last 30 days of alert sources with usable geolocation.**

| Item | Meaning |
|---|---|
| Mapped Source IPs | Source addresses represented in the geolocation dataset. |
| Countries Observed | Countries represented by those geolocated sources. |
| Alerts Represented | Alert counts represented by the mapped source data. |
| Map markers / country drill-down | Geographic context with a way to inspect related alerts where offered. |

Missing geolocation, private addresses and records without suitable source data can leave alerts off the map. Therefore, map coverage need not equal total alert volume.

IP geolocation describes an apparent network location, not a verified attacker identity or physical origin. Hosting providers, VPNs, proxies and shared infrastructure can affect interpretation. An empty map does not mean there were no alerts or attacks.

### Color coding: map markers and hotspot dots

The colors show **the highest recorded alert severity represented by that source or country**, not a country risk rating or a confirmed attacker verdict.

| Color | Severity | Map marker | Hotspot dot | How to interpret it |
|---|---|---|---|---|
| 🔴 Red | Critical | `#da3633` | `#da3633` | At least one represented alert has Critical severity. Prioritize review of its evidence. |
| 🟠 Orange | High | `#f0883e` | `#f0883e` | The highest represented severity is High. |
| 🟡 Yellow | Medium | `#e3b341` | `#e3b341` | The highest represented severity is Medium. |
| 🟢 Green | Low | `#2ea043` | `#2ea043` | The highest represented severity is Low. This does not establish that the source is safe. |
| Gray | Unknown / unrecognized | `#8b949e` | `#6b7280` | No recognized severity is available for the represented data. Treat this as missing classification, not Low. |

For a map marker, the severity is aggregated for that source IP. For a Source Hotspots country row, the dot uses the highest severity among the represented source points. A country with many Low alerts and one Critical alert can therefore show a red dot. That does not make every alert or every source from that country Critical.

**Color and volume are different:** marker size increases with the represented alert count, with a minimum and maximum visual size. Larger markers indicate more represented alerts, not greater confidence of compromise. The pulsing rings are a visual animation; they do not show live packet traffic or prove that an attack is still running.

Select a map marker to inspect its popup: source IP, available city/region/country, alert count, severity, ISP, Last Seen and available AbuseIPDB or VirusTotal scores. Those reputation scores are separate from the severity color. Use the recorded time and underlying alerts to determine whether activity is recent and relevant.

A high-volume hotspot and a high-severity hotspot are different prioritization signals. Review both the count and the color, then inspect the actual alerts. These colors describe this map and hotspot section; other dashboard charts may use their own palette.

### Source Hotspots

Countries are ranked using alert-generating source data. Each row shows the country, number of source IPs, alert count and a severity indicator based on the highest represented severity. Select a row to inspect the country-alert dialog; use its alert links for triage.

A high count can reflect repeated activity from a few sources. It is not a count of unique adversaries or confirmed incidents. The **Alerts** button opens the broader alert list.

## 3. Priority Response Metrics

This group is initially expanded. It combines case SLA information with alert-priority and escalation indicators.

| Metric | Calculation / scope | Analyst or client interpretation |
|---|---|---|
| SLA Compliance | Percentage of scoped cases with at least one SLA target that have no recorded breach of the applicable response or resolution targets. | A service-process indicator. Still-active cases may be included; no recorded breach is not proof that all future deadlines have been met. |
| Mean Time to First Response | Average recorded case response duration, displayed in hours, for cases with a recorded response time. | Shows recorded responsiveness. It is not the autonomous worker's runtime or every alert's acknowledgement time. |
| Mean Time to Resolve | Average recorded case resolution duration, in hours, for cases with a recorded resolution time. | Shows case-resolution speed for qualifying records; unresolved cases without a resolution duration are not included in this average. |
| Critical Alerts Older Than 4h | Critical alerts in New or Open status created more than four hours ago. | Opens a dialog of aged critical alerts. Investigate these promptly; this fixed age threshold is separate from each alert's configured SLA. |
| Unassigned Cases | Active cases in Open, In Progress or Pending with no assigned analyst. | Opens an unassigned-case dialog. Ensure work has an owner. |
| Mean Time to Detect | Average time from a linked alert's creation to its case's creation, for valid non-negative linked times. | In this dashboard, this is an alert-to-case timing proxy. It does not measure the interval from actual attacker activity to detection. |
| Alert → Case Rate (7d) | Distinct alerts created in the last seven days that have a case link, divided by all scoped alerts in that window. | Measures case linkage/escalation. A high or low rate needs context; neither is automatically good or bad. |

For example, 20 case-linked alerts out of 100 qualifying alerts produce a 20% Alert → Case Rate. One alert linked to multiple cases is not counted multiple times in that rate.

Do not compare response and resolution averages as if they include exactly the same records. Their qualifying populations can differ. The hour suffix is decimal time: `0.50h` means 30 minutes.

## 4. Documentation and Quality Metrics

Initially collapsed; select **Show** to expand.

| Metric | What it measures | What to do |
|---|---|---|
| Missing Description | Active cases with an empty or missing case description. | Write a useful summary of scope, findings and next steps. |
| Missing Evidence Files | Active cases without an available case-evidence attachment record. | Check whether an evidence file should be attached. Linked alert data alone does not satisfy this file-specific check. |
| Missing Artifacts | Active cases without directly linked artifact records, such as relevant observables. | Add appropriate verified artifacts where useful; avoid creating meaningless entries just to reduce the counter. |
| Audit Completeness | Percentage of scoped cases with at least one case-history entry. | Indicates basic traceability, not a complete forensic audit, chain of custody or proof that every action was logged. |
| False Positive Rate | Cases with False Positive status divided by cases with Closed or False Positive status. | Indicates the share of those case outcomes classified as false positives. It is not an AI-model false-positive benchmark or a rate over every ingested alert. |

These are case-record checks. **Missing Evidence Files is different from the Investigation Center's evidence sufficiency assessment.** A case can have an attachment while an investigation still lacks required endpoint or Wazuh evidence, and the reverse is also possible.

A high False Positive Rate can prompt detection tuning, but review sample size, classification consistency and detection context first.

## 5. Threat Activity and Analyst Monitoring

### Alert Volume by Severity

A stacked daily chart of the last seven days. Each bar shows alert volume and its severity mix. Hover to inspect individual values.

Use it to identify bursts, changes in severity and abnormal quiet periods. A higher bar means more recorded alerts, not necessarily more distinct attacks. A drop can be caused by ingestion interruption as well as reduced activity.

### Analyst Alert Monitoring

**Window: alerts created within the last 30 days.** Rows group alerts by assigned analyst, with an Unassigned group where applicable. The presence badge shows recent application activity and the total is the alert count for that row/window.

| Column | Meaning |
|---|---|
| New | Alerts with New status. |
| Active | Alerts in New, Open, Investigating or Reviewed status. New is therefore also part of Active. |
| Ack Pending | Assigned alerts without a recorded acknowledgement. |
| At Risk | Alerts whose SLA tracking status is At Risk. |
| Breached | Alerts with a breached SLA state or recorded breach in acknowledgement, triage, escalation or initial investigation. |

Counts overlap; do not add the columns to calculate a total. An acknowledged alert can still have a later-stage breach. Select a count to open the analyst's Alert SLA view and inspect its filters. Not every count adds a dedicated destination filter, so verify the destination before assuming an exact row-for-row match. **Alert SLA** opens the broader alert SLA workspace.

### Analyst Case Monitoring

Shows case workload grouped by analyst, including unassigned work where applicable. This panel is not restricted to the alert panel's 30-day window.

| Column | Meaning |
|---|---|
| Open | Cases in Open status. |
| In Progress | Cases actively marked In Progress. |
| Pending | Cases marked Pending, for example while waiting on an external dependency. |
| Done | Cases with Closed or False Positive status. |
| Breached | Cases with a recorded response or resolution SLA breach. This can overlap status columns, including Done. |

Select a count to open the associated case view. **All Cases** opens the wider case list. The dashboard shows a limited set of grouped rows, so use full views for complete workload review.

### Top Triggered Rules

**Window: last seven days.** Lists high-volume detection rules.

| Column | Meaning |
|---|---|
| Rule | Detection description and Rule ID. |
| Alerts | Number of matching alert records in the window. |
| High/Crit | Matching alerts classified High or Critical. |
| Last Seen | Most recent represented alert time. |

Use this to identify busy detections and candidates for investigation or tuning. A frequent rule is not necessarily faulty, and a rare rule can still be important.

### Recent IOC Matches

Shows recent local IOC-feed or external threat-intelligence matches.

| Column / label | Meaning |
|---|---|
| IP / Indicator | The matched observable. Its link opens the originating alert. |
| Local IOC / Threat Intel | Indicates the type of match source. |
| Source | Feed or provider name. |
| Score | Stored confidence or provider-derived score, displayed as a percentage. Different providers can use different meanings and scales. |
| When | Recorded match time. |
| Threat type, where present | Additional classification supplied with the result. |

A high score is a lead for review, not a universal probability of maliciousness. Open the alert, inspect context and confirm whether the observable is relevant. No matches can mean enrichment has not run, a source is unavailable or no matching indicator exists. **IOC Manager** opens the indicator-management area.

## 6. Case Tags and MITRE Monitoring

Initially collapsed. These charts are derived from tags on scoped cases with **Closed or False Positive status**, not a live measurement of all endpoint behavior.

### Case Tags Monitoring

Shows recognized threat-category tags such as Malware, Phishing, Ransomware, C2 or Brute Force. The badge reports tagged cases in the qualifying dataset. The chart displays a limited set of recognized categories; custom or unrecognized tags may not appear.

A case can have more than one tag, so category totals can exceed the number of tagged cases. Do not treat category counts as unique incident counts.

### MITRE Tactics Monitoring

Shows recognized MITRE tactic tags, such as Execution, Persistence, Credential Access or Lateral Movement. It summarizes case categorization; it does not prove technique execution, detection coverage or a successful attack.

These case-tag charts differ from technique-specific evidence coverage shown in Investigation Center. No Data can simply mean qualifying cases lack recognized tags.

## 7. Compliance Visualizations

Initially collapsed. These charts describe recorded operational outcomes, not regulatory certification.

| Chart | What it represents | Interpretation limit |
|---|---|---|
| SLA Breach Trend | Six monthly buckets of cases with a recorded response or resolution breach, grouped by case creation month. | This is not the exact month each breach first occurred. Changes in case volume also affect counts. |
| Incident Closure by Severity | Closed and False Positive case counts grouped by severity. | These are case outcomes, including false positives; not every bar is a confirmed malicious incident. |
| Analyst Adherence | Per-analyst percentage of qualifying SLA-tracked cases with no recorded applicable response/resolution breach. | Consider workload, sample size, assignment and open cases before comparing analysts. The chart shows a limited set of rows. |
| Audit Completeness | Proportion of cases with versus without at least one case-history entry. | Shares the basic presence test used by the KPI; it does not validate narrative quality or chain of custody. |

## 8. Recent Alerts and Cases

Initially expanded. These are short recent-activity lists, not exhaustive searches.

### Recent Alerts

- **Rule:** Rule ID and a shortened description; select it to open the alert.
- **Severity:** Recorded alert severity.
- **Status:** Current alert workflow status, not its autonomous investigation status.
- **Source IP:** Recorded source address, or a dash when absent.
- **Time:** Relative age; hover where available for the underlying timestamp.
- **View All:** Opens the full alert list.

### Recent Cases

- **Case:** Case number and shortened title; select it to open the case.
- **Sev:** Recorded case severity.
- **Status:** Case workflow status.
- **Analyst:** Assigned analyst, or a dash when unassigned.
- **View All:** Opens the full case list.

An alert's status, case status, AI recommendation and analyst disposition can differ because they describe different records and decisions.

## 9. System Resource Monitor

Visible only to permitted accounts and initially collapsed. These are resources on the **AlistoIR server**, not the Wazuh server or all monitored endpoints.

| Item | Meaning |
|---|---|
| Storage path and status | The monitored filesystem/path and its current storage classification. |
| % used and progress bar | Used fraction of that filesystem; markers show configured thresholds. |
| Free Space / Used Space / Total Capacity | Available, consumed and total capacity for the monitored volume. |
| Thresholds | Warning and critical utilization levels. Read the displayed values rather than assuming fixed limits. |
| CPU Usage | Current sampled/estimated CPU usage and status, with load average and core count. |
| Memory Usage | Memory usage and status, with available and total memory. |

Sustained resource pressure can delay ingestion, searches or AI work. Ask the administrator to investigate capacity and retention. Unavailable means the metric could not be obtained, not zero usage. Do not delete evidence or logs simply to make the card green.

## 10. Alert Queue Operations

A platform-level operations panel, initially collapsed. It monitors the asynchronous **alert-processing queue**, which is distinct from the autonomous investigation run list.

### Queue Health Summary

**HEALTHY / ATTENTION NEEDED** reflects queue health checks and configured thresholds. Read the accompanying message: a missing queue table can show an informational state rather than evidence that work has been processed. **Ops Guide** opens the in-application queue operations guide.

| Metric | Meaning |
|---|---|
| Ready Backlog | Jobs eligible to run now but still waiting. |
| Oldest Ready Lag | Age in minutes of the oldest ready job. |
| Failed Jobs | Jobs marked failed, including exhausted retries. |
| Running Jobs | Jobs currently marked as processing. |

A temporary backlog can be normal. Growth over time, increasing lag or stale running jobs warrants operational investigation. Zero Running Jobs can mean no work is waiting; it does not by itself prove a stopped worker.

### Worker Controls

This subpanel displays the baseline; it is not a form for starting or stopping workers.

- **App Worker Lock:** Application lock behavior used to limit overlapping execution.
- **Lock File:** Configured lock path, or Not configured.
- **Cron `flock`:** A recommendation for scheduler-level overlap protection, not proof that the server cron entry uses it.
- **Pending / Failed / Running Warn Thresholds:** Configured warning limits; the running threshold is expressed in minutes.

An ENABLED lock badge does not prove that a worker is currently alive or making progress.

### Queue Observability

The recent summary is labeled **Last 24 hours**, with a bounded recent-job sample. Treat these as operational sample statistics, not an exhaustive accounting report.

| Field | Meaning |
|---|---|
| Notifications Sent | Successful notification outcomes recorded in the sample. |
| IOC Matched | Recorded matching outcomes. |
| Cases Created | Case-creation outcomes attributed to sampled queue work. |
| Avg Duration | Average recorded job duration in seconds, not full incident resolution time. |
| AI Insight Ready | Insight outcomes generated now or reused from cache, with separate Generated / Cached counts. |
| Next Events Ready | Next-events output generated or reused from cache. |
| AI Insight Failed / Next Events Failed | Failures recorded for those queue steps. |
| Active AI refresh backlog | Pending, Running and Failed counts for insight and next-events refresh work. |
| AI step status badges | Threshold classifications for failures and backlogs; the aggregate signals can include refresh work as well as sampled queue results. |
| Top notification skip reasons | The most frequent recorded reasons a notification was skipped. A skip is not automatically a delivery failure. |
| Last completed / Last failure | Time of the most recent recorded completion or failure, when available. |

Ready or Cached AI output does not certify its verdict or mean a response action was executed. Investigate provider access, quotas, collection dependencies and specific errors when failures recur. Do not disable privacy or integrity controls to clear a warning.

## 11. System Status

Visible to platform-role accounts and initially collapsed. Integration groups can include:

| Group | Typical entries and purpose |
|---|---|
| SIEM and Ingestion | Wazuh manager registration and enabled state. Community supports one configured Wazuh manager. |
| AI Provider | Configured provider/Copilot state. |
| Notifications | Slack, Microsoft Teams, Telegram, PagerDuty and SMTP email configuration states, where present. |
| Threat Intelligence | Configured providers such as VirusTotal, AbuseIPDB, MISP, OpenCTI, AlienVault OTX, GreyNoise, Shodan and URLScan. |
| Endpoint Response | Native endpoint enrollment/recent presence, where configured. This does not establish that every endpoint collection method works. |
| Core Services | Database connection indicator and application server time. |

**READY** generally means the relevant configuration is enabled/present or a registration/presence condition is satisfied. It is not an end-to-end live connection or transaction test. **SETUP**, Needs setup, Disabled, Configured in UI and Using .env fallback explain configuration state or its source.

For example, an AI provider can be READY but lack balance, and an enabled Wazuh manager can still have a failing Indexer connection. Validate ingestion, Manager API, Indexer and endpoint callbacks independently. A database CONNECTED badge does not validate backups, all queries or replication health.

The shared dashboard may display a Universal SIEM Sources status row. That display does not enable other SIEM integrations in Community; Community remains Wazuh-only. Use supported Community configuration paths.

## 12. Dialogs and drill-downs

| Dialog / action | What it helps you inspect |
|---|---|
| Country alerts | Related alert records behind a geographic hotspot. |
| Critical Alerts Older Than 4h | Aged critical alerts requiring triage. |
| Unassigned Cases | Active cases needing ownership. |
| Online Analysts | Recently active user records. |
| Workload-count links | Analyst-related alert SLA or case views. Check destination filters before comparing counts. |

Open the underlying alert or case to perform the actual investigation, assignment or review. A dialog is a supporting view and may show a limited result set. Refresh if a record has changed since the dashboard was loaded.

## Suggested daily workflow

1. Check freshness, scope and any platform-health warnings.
2. Review Critical / New and Critical Alerts Older Than 4h.
3. Assign unowned cases and inspect alert acknowledgement/SLA risk.
4. Open relevant alerts and use [Investigation Center](INVESTIGATION_CENTER.md) for evidence and analyst review.
5. Review IOC matches, busy rules and unusual severity trends in context.
6. Update case documentation, artifacts, evidence and decisions.
7. Review persistent backlog or resource warnings with the administrator.

For a client update, report the relevant time window, alert and case workload, verified conclusions, unresolved evidence gaps and next actions. Do not present mapped source countries, IOC scores or green operational badges as confirmed incident outcomes.

## Common interpretation mistakes

| Mistake | Correct interpretation |
|---|---|
| All dashboard numbers refer to today | Each section has its own window or all-scoped population. |
| Open Cases equals all unfinished cases | The KPI is Open status only; In Progress and Pending appear in wider case monitoring. |
| MTTD measures time since compromise began | It is the linked alert-created to case-created timing proxy here. |
| 100% SLA compliance means all incidents are resolved | It reflects no recorded applicable breach in qualifying cases, including potentially active cases. |
| Audit Completeness proves a complete audit trail | It checks for at least one case-history entry. |
| False Positive Rate measures AI accuracy | It uses case outcome statuses, not a ground-truth model evaluation. |
| No mapped sources or IOC matches means no threats | Coverage, enrichment and ingestion prerequisites may be missing. |
| A green READY integration is fully tested | It is primarily a configuration/presence signal. |
| Live sync means an investigation reran | It refreshes dashboard data; processing occurs through separate workers and workflows. |

See [Configuration](../docs/CONFIGURATION.md), [Wazuh setup](../docs/WAZUH_SETUP.md), [Investigation Settings](INVESTIGATION_SETTINGS.md), [AI privacy](../docs/AI_PRIVACY.md) and [Troubleshooting](../docs/TROUBLESHOOTING.md) for next steps.
