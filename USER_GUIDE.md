# PII, Secret & AI Redaction Guard for Jira — User & Administrator Guide

Updated 10 November 2026

## 1. Installation & Quick Start

1. Install **PII, Secret & AI Redaction Guard for Jira** from the Atlassian Marketplace into your Jira Cloud site.
2. Open any Jira project (e.g., **IT Helpdesk**) and select **PII, Secret & AI Redaction Guard for Jira** in the left project sidebar (`jira:projectPage` — `jira-safe-redact-admin`).
3. Review and toggle the active Space Redaction Policy Packs:
   - **Cloud Secrets & API Keys**: `AWS_KEY` (`AKIA...`, `ASIA...`), `BEARER_TOKEN` (`sk_live_...`, `ghp_...`, `xoxb-...`, `Bearer ...`).
   - **HIPAA**: `SSN` (`\b\d{3}-\d{2}-\d{4}\b`).
   - **PCI-DSS**: `CREDIT_CARD` (13–16 digit payment card PANs).
   - **GDPR PII**: `EMAIL_PII` (exposed personal/customer email addresses).
4. Click **Save Space Redaction Policy (<32 KB)** to save the policy to the project property `/rest/api/3/project/{projectId}/properties/safeRedactPolicy`.
   - Saving needs the **Administer Projects** permission on the project.
   - The page confirms the save and shows the policy as Jira now holds it. If Jira refuses the save, the page shows Jira's reason and nothing is saved.
   - Switching a pack off stops the app looking for that pack's kinds of data. Switching **Rovo AI Safe Labeling** off stops it adding the `sanitized-for-ai` label.

---

## 2. Dry-Run Scanning & 1-Click In-Place ADF Redaction on an Issue

1. Open any Jira issue and click the **PII, Secret & AI Redaction Guard for Jira** issue panel (`jira:issuePanel` — `jira-safe-redact-panel`).
2. The scanner recursively traverses the issue's `summary`, `description` (Atlassian Document Format AST), and every `comment` body (`body` ADF AST), however many comments the issue has. If Jira refuses to return any of them, the panel shows the error instead of scanning.
3. Review the **Zero-Leak Dry-Run Masked Diff Preview** table:
   - Displays each finding's pattern type (`AWS_KEY`, `BEARER_TOKEN`, `SSN`, `CREDIT_CARD`, `EMAIL_PII`), policy pack, ADF AST field path, and partially masked preview (`sk_live_...9f2a -> [REDACTED_API_KEY]`).
4. Click **1-Click Sanitize ADF In-Place & Issue SHA-256 Certificate**:
   - Replaces matched secrets and PII inside the ADF AST text nodes (`[REDACTED_API_KEY]`, `[REDACTED_AWS_KEY]`, `[REDACTED_SSN]`, `[REDACTED_CREDIT_CARD]`, `[REDACTED_EMAIL_PII]`) while preserving all rich-text formatting, marks, tables, and lists.
   - Applies the Jira issue label **`sanitized-for-ai`** so Atlassian Rovo AI agents and JQL filters can target verified-clean issues, unless the project's policy has labelling switched off.
   - Computes the deterministic SHA-256 certificate `SHA-256(accountId|issueKey|redactedCount|patternTypes|timestamp)` and writes only zero-leak metadata to `/rest/api/3/issue/{issueKey}/properties/safeRedactAudit`. The certificate records only what this run redacted.
   - If Jira refuses any change (for example, an edit to someone else's comment you're not allowed to edit), the panel says which change and what was already saved, and no certificate is written. Fix the cause and run it again.
   - If nothing is found, nothing is changed and no certificate is written. Any certificate shown is from an earlier redaction.

---

## 3. Space-Wide JQL Batch Scanner & 1-Click Compliance CSV Export

1. Open **PII, Secret & AI Redaction Guard for Jira** from the project sidebar.
2. Click **Run Space-Wide JQL Batch Scanner** to inspect issues across the space.
3. Click **1-Click Export Compliance Audit CSV** to generate an RFC-4180 compliant CSV containing issue keys, summaries, sanitization status, redacted counts, detected pattern types, scrubbed field paths, `sanitized-for-ai` label status, operator Account IDs, UTC timestamps, and 64-character SHA-256 certificates.

---

## 4. Support

For technical support or feature requests, contact **EvidencePair LLC** at <https://evidencepair.com/support/> (`support@evidencepair.com`). We reply within one business day. Report security issues to `security@evidencepair.com`.
