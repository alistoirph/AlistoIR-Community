# Analyst Chat — team collaboration guide

Page: `/chat/index` (also accessible through `/chat`). Applies to AlistoIR Community 3.0.3. This is **human-to-human collaboration**, separate from the AI Investigation Assistant.

## What is Analyst Chat for?

Use Chat to coordinate triage, ask colleagues for context, discuss indicators and hand over work. It supports channels and one-to-one rooms. Important conclusions, evidence, ownership changes and approvals still belong in the relevant alert, investigation, case or response workflow.

Posting “reviewed,” “acknowledged” or “approved” in chat does not update an alert's status, stop an SLA timer, record the Investigation Center disposition or approve a containment task.

| Conversation | Purpose | Access model |
|---|---|---|
| Public team channel | Discussion visible to the allowed team/account scope. | Public means an application channel, not an anonymous Internet page. |
| Private channel | Discussion restricted by configured channel membership. | A manager controls membership; do not assume an @mention grants access. |
| Direct conversation | One-to-one coordination between eligible users. | Subject to tenant and allowed group pairing. See [Direct Messages](DIRECT_MESSAGES.md). |

## 1. Page layout

| Area | What it shows |
|---|---|
| Conversation header | Selected room name, optional Read-only/1:1 Chat badge, sync status and typing context where available. |
| Message count | Messages currently loaded by the browser, not a guaranteed lifetime total. |
| Search button/panel | Message search with bounded results. |
| Message area | Author, time, text, optional attachment/reply context and available message actions. |
| Composer | Message text, attachment control, emoji picker and Send. |
| Channels | Available channels, selected-channel highlight, icon/color and a lock for read-only channels. |
| Online / Active | Recently active users and a DM shortcut where eligible. |
| Manage channels gear | Channel management for permitted managers. |

The initial full-page load retrieves a bounded recent message set (normally 80). Live updates add new messages; this is not a full archive export. Use search to locate older context and do not assume the browser count represents every stored message.

## 2. Channels and colors

Default channel definitions include:

| Channel | Purpose | Default visual identity |
|---|---|---|
| SOC General | General security-operations discussion. | Blue comments icon. |
| Incident Response | Coordination during incidents. | Red fire icon. |
| Threat Intelligence | IOC/threat context discussion. | Amber crosshairs icon. |
| Alert Triage | Alert-review discussion. | Purple bell icon. |
| Announcements | Team announcements; read-only for ordinary posters. | Green announcement icon. |

Channels can differ by deployment and permitted visibility. Colors are organizational labels, **not severity or verdict scores**. The active-channel highlight identifies your current room. A lock/Read-only badge concerns posting policy; it is not an encryption indicator.

Select a channel before composing, then verify its name. Group restrictions used for direct conversations do not automatically create private groups inside a public channel.

## 3. Send a message

1. Select the intended channel or direct room.
2. Confirm the room and its audience.
3. Enter a message of up to **2,000 characters**.
4. Optionally add a supported file, screenshot, emoji or reply context.
5. Select the paper-plane Send button, or press **Enter**.
6. Confirm that the sent message appears with your author/time context.

**Shift+Enter** adds a line break. **Ctrl+V** can paste a screenshot on supported browsers. The composer also supports a file picker and supported drag/drop attachment handling. Check the chosen filename before sending.

A spinner or upload percentage describes transfer/request progress, not proof that another person read the message. If sending fails, inspect the error and current conversation before retrying to avoid duplicates.

## 4. Replies, mentions and reactions

| Feature | How to use it | What it does not mean |
|---|---|---|
| Reply | Select Reply on a visible message, check the Replying To banner, compose and send. Cancel the banner to send normally. | Does not create a separate case/task or necessarily a separate threaded room. |
| @mention | Type `@` and select an available analyst suggestion. | Does not grant channel access, assign an alert or guarantee acknowledgement. |
| Emoji picker | Inserts an emoji into your composed text. | Does not change incident priority. |
| Reaction | Select an offered reaction; an existing reaction can be toggled where supported. | A reaction is not a formal incident disposition or response approval. |

Mentions, replies, reactions and DMs can produce in-application notifications when supported and eligible. They do not guarantee email, SMS, push delivery or immediate action by the recipient. Use the organization's escalation route for time-critical work.

## 5. Attachments and screenshot sharing

The standard upload allowlist includes **JPEG, PNG, GIF, WebP, PDF, TXT, CSV and JSON**, with a **10 MiB per-file maximum**. Server MIME/content/extension checks are authoritative; browser selection is not final acceptance. Executable/script and other blocked types are rejected.

Images can have thumbnails/preview controls. Supported attachment links allow authorized viewing/downloading. A preview or accepted file is not proof of malware scanning, authenticity or forensic chain of custody.

If a Direct Messages file picker offers ZIP, do not assume ZIP upload is accepted: the current standard upload allowlist is narrower than some picker/download labels. Read the server outcome; do not rename executable files to bypass validation.

Attachments are not automatically inserted into a case's Evidence Files, registered as artifacts or analyzed by AI. Add investigation-relevant evidence through the proper case/alert workflow and preserve references there.

Review screenshots for credentials, customer identifiers, personal information and unrelated browser content before sending. Chat does not automatically apply the external-AI sanitizer to text or attachments.

## 6. Edit and delete

| Action | Current behavior |
|---|---|
| Edit | Edit your own accessible message within **15 minutes** of sending. The server enforces ownership and time. |
| Delete | Normal users delete their own accessible messages through the available action. Another person's message is not generally editable/deletable through your normal controls. |

Controls can differ between the full page and compact DM window. Missing actions can reflect ownership, permissions or server validation. Follow the result/error instead of assuming a visible button guarantees authorization.

Deletion is a recorded removal from the application conversation, not a promise to erase all copies, notification previews, forwards, screenshots or backups. Editing a message does not correct an already-copied alert/case conclusion; update the authoritative record separately.

## 7. Search messages

Select the magnifying glass, enter at least **two characters**, and inspect the returned room/author/time context. The full-page search requests matches across accessible conversations, with bounded results; the same input can also filter loaded messages in the current room.

Search uses the configured text-search capability, with a substring fallback where needed. It is not a raw attachment-content search or guaranteed complete transcript. Results are limited (normally up to 30, with a server maximum of 50 per request). No results can reflect scope, deleted content, search semantics or limits.

Select a result to open its conversation. Close the search panel or clear the term to return to normal viewing. Check room name and any remaining filter before assuming a message is missing.

## 8. Presence, typing and live synchronization

The page uses adaptive polling: normally about **3 seconds** while active, increasing to about 6/10 seconds with inactivity and about 20 seconds when hidden, with delay after failures. It is not a guaranteed real-time delivery service.

Online / Active is a recent-activity list, normally considering activity within about ten minutes. It is not a full directory or a duty roster. A missing user may be offline, inactive, outside the allowed scope or outside the bounded list.

Typing/replying indicators, where available, are short-lived application signals. They do not prove a message will be sent or that the other person has accepted an investigation. See [Direct Messages](DIRECT_MESSAGES.md) for unread/seen behavior.

## 9. Manage channels

Permitted managers can open the gear / `/chat/channels` to manage team channels. Account/tenant context and role checks apply.

| Option | Purpose |
|---|---|
| Channel name / description | Human-readable purpose and room label. |
| Icon / color | Visual organization; not a security classification. |
| Public / Private | Team-visible channel versus membership-restricted channel. |
| Read-only | Ordinary users can read but not post; permitted manager/admin roles can post. |
| Private membership | Add/remove eligible members. Creating a private channel adds its creator as owner. |
| Convert to private | Available for eligible tenant-specific channels; review membership afterward. |
| Delete channel | A destructive management action where allowed. Default channels cannot be removed using the normal delete action. |

Review the audience before creating or converting a channel. A lock can indicate read-only posting rather than private membership, so use the actual channel settings to verify privacy. Do not treat deletion or conversion as a recall of previously shared content.

## 10. Security and investigation handover

Chat messages/files are stored through AlistoIR's server-side application. One-to-one or Private describes application access, not an end-to-end encryption guarantee. Administrators, server storage and backups remain part of the deployment's trust boundary.

Chat is not an AI assistant and normal chat sending does not invoke the model-analysis workflow. The AI privacy boundary does not automatically redact chat content, prevent recipients copying it or sanitize downloaded files.

For a useful handover, include the alert/case reference, verified observation, remaining gap, requested next step and responsible person. Record the formal conclusion in the alert, Investigation Center or case, rather than leaving it only in chat.

## 11. Troubleshooting

| Symptom | What to check |
|---|---|
| Chat storage unavailable | Administrator should check database/schema and application logs. |
| Cannot post | Confirm the room, membership, read-only state and permissions. |
| No active analysts listed | Recent presence and access scope; use the header DM directory for eligible offline recipients. |
| @mention unavailable | Eligible-user list, room access and browser script loading. |
| Upload rejected | Type, MIME/content validation, size and server upload limits. |
| Cannot edit | Ownership and 15-minute limit. |
| Cannot delete another message | Normal deletion is restricted; do not assume managerial controls override every backend ownership check. |
| Messages delayed | Connectivity, polling/backoff, browser visibility and server health. |
| Security token expired | Refresh the signed-in page and retry the intended send/action. Do not disable CSRF. |
| Message count/search looks incomplete | Recent-load and search limits; verify room, search term and scope. |

Continue with [Direct Messages](DIRECT_MESSAGES.md), [Alerts](ALERTS.md), [Alert View](ALERT_VIEW.md) and [Investigation Center](INVESTIGATION_CENTER.md).
