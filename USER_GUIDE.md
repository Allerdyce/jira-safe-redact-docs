# PII, Secret & AI Redaction Guard for Jira — User & Administrator Guide

## 1. Installation & Quick Start

1. Install **PII, Secret & AI Redaction Guard for Jira** from the Atlassian Marketplace into your Jira Cloud site.
2. Open any Jira project (e.g., **IT Helpdesk**) and select **PII & Secret Redaction Radar** in the left project sidebar (`jira:projectPage` — `jira-safe-redact-admin`).
3. Review and toggle the active Space Redaction Policy Packs:
   - **Cloud Secrets & API Keys**: `AWS_KEY` (`AKIA...`, `ASIA...`), `BEARER_TOKEN` (`sk_live_...`, `ghp_...`, `xoxb-...`, `Bearer ...`).
   - **HIPAA**: `SSN` (`\b\d{3}-\d{2}-\d{4}\b`).
   - **PCI-DSS**: `CREDIT_CARD` (13–16 digit payment card PANs).
   - **GDPR PII**: `EMAIL_PII` (exposed personal/customer email addresses).
4. Click **Save Space Redaction Policy (<32 KB)** to persist the policy directly to `/rest/api/3/project/{projectId}/properties/safeRedactPolicy`.

---

## 2. Dry-Run Scanning & 1-Click In-Place ADF Redaction on an Issue

1. Open any Jira issue and click the **PII & Secret Sanitizer (Rovo AI Guard)** issue panel (`jira:issuePanel` — `jira-safe-redact-panel`).
2. The scanner recursively traverses the issue's `summary`, `description` (Atlassian Document Format AST), and `comment` bodies (`body` ADF AST).
3. Review the **Zero-Leak Dry-Run Masked Diff Preview** table:
   - Displays each finding's pattern type (`AWS_KEY`, `BEARER_TOKEN`, `SSN`, `CREDIT_CARD`, `EMAIL_PII`), policy pack, ADF AST field path, and partially masked preview (`sk_live_...9f2a -> [REDACTED_API_KEY]`).
4. Click **1-Click Sanitize ADF In-Place & Issue SHA-256 Certificate**:
   - Replaces matched secrets and PII inside the ADF AST text nodes (`[REDACTED_API_KEY]`, `[REDACTED_AWS_KEY]`, `[REDACTED_SSN]`, `[REDACTED_CREDIT_CARD]`, `[REDACTED_EMAIL_PII]`) while preserving all rich-text formatting, marks, tables, and lists.
   - Applies the Jira issue label **`sanitized-for-ai`** so Atlassian Rovo AI agents and JQL filters can target verified-clean issues.
   - Computes the deterministic SHA-256 certificate `SHA-256(accountId|issueKey|redactedCount|patternTypes|timestamp)` and writes only zero-leak metadata to `/rest/api/3/issue/{issueKey}/properties/safeRedactAudit`.

---

## 3. Space-Wide JQL Batch Scanner & 1-Click Compliance CSV Export

1. Open **PII & Secret Redaction Radar** from the project sidebar.
2. Click **Run Space-Wide JQL Batch Scanner** to inspect issues across the space.
3. Click **1-Click Export Compliance Audit CSV** to generate an RFC-4180 compliant CSV containing issue keys, summaries, sanitization status, redacted counts, detected pattern types, scrubbed field paths, `sanitized-for-ai` label status, operator Account IDs, UTC timestamps, and 64-character SHA-256 certificates.

---

## 4. Support & SLA

For technical support, feature requests, or security inquiries, contact **EvidencePair LLC** at `support@titlecovenant.com`.
