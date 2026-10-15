# Atlassian Marketplace Listing Copy

- **App Name:** PII, Secret & AI Redaction Guard for Jira
- **App Key:** `com.evidencepair.jira.safe.redact`
- **Tagline (Short Description):** Scrub exposed PII & cloud secrets from Jira ADF descriptions and comments with SHA-256 certificates and Rovo AI safe labels.
- **Categories:** Compliance & Security, Admin Tools, IT & Helpdesk

---

## Full Marketplace Description

Prevent accidentally pasted API keys, cloud credentials, Social Security Numbers, credit card PANs, and personal email addresses from lingering in Jira tickets or leaking into AI grounding contexts. **PII, Secret & AI Redaction Guard for Jira** combines an Atlassian Document Format (ADF) AST recursive scrubber, a zero-leak dry-run diff preview, and deterministic Web Crypto SHA-256 sanitization certificates into a 100% *Runs on Atlassian* security guard.

### Why Security & Compliance Teams Choose PII, Secret & AI Redaction Guard

1. **ADF-Safe Issue & Comment Scrubber + Masked Dry-Run Preview (`jira-safe-redact-panel`)**
   - Recursively walks Jira ADF JSON AST nodes across issue `description`, `summary`, and `comment` bodies.
   - Displays a zero-leak masked dry-run preview (`sk_live_...9f2a -> [REDACTED_API_KEY]`, `AKIA...MPLE -> [REDACTED_AWS_KEY]`, `***-**-6789 -> [REDACTED_SSN]`) and executes 1-click in-place redaction without breaking rich-text formatting, tables, code blocks, or marks.

2. **Zero-Leak SHA-256 Sanitization Certificate & Rovo AI Safe Labeling (`safeRedactAudit`)**
   - Never stores raw unredacted PII or secrets inside Jira Entity Properties or logs.
   - Records only the pattern type (`AWS_KEY`, `BEARER_TOKEN`, `SSN`, `CREDIT_CARD`, `EMAIL_PII`), match count, field path, and deterministic Web Crypto SHA-256 certificate (`accountId|issueKey|redactedCount|patternTypes|timestamp`) in Issue Property `safeRedactAudit` (`<32 KB`) while applying the `sanitized-for-ai` label.

3. **Space Policy Packs & JQL Batch Radar with 1-Click CSV Export (`jira-safe-redact-admin`)**
   - Space admins toggle compliance policy packs (*Cloud Secrets & API Keys*, *HIPAA*, *PCI-DSS*, *GDPR PII*) in `safeRedactPolicy` (`<32 KB`), run space-wide JQL batch scans, and export an RFC-4180 compliance CSV in 1 click.

---

## Version Highlights

### Highlight 1: ADF-Safe Issue & Comment Scrubber with Zero-Leak Dry-Run Preview
Recursively traverse Jira Atlassian Document Format (ADF) descriptions, summaries, and comments to preview masked matches (`sk_live_...9f2a -> [REDACTED_API_KEY]`) and scrub exposed secrets and PII in-place without breaking rich-text formatting.

### Highlight 2: Zero-Leak Web Crypto SHA-256 Certificate & Rovo AI Safe Label
Record a tamper-evident SHA-256 sanitization certificate (`accountId|issueKey|redactedCount|patternTypes|timestamp`) in Jira Issue Property `safeRedactAudit` (`<32 KB`, zero raw PII stored) and automatically apply the `sanitized-for-ai` label.

### Highlight 3: Space Policy Packs (HIPAA, PCI-DSS, Cloud Secrets, GDPR) & CSV Export
Configure space-level detection packs in `safeRedactPolicy`, execute space-wide JQL batch scans in `jira:projectPage`, and export your compliance audit ledger to RFC-4180 CSV in 1 click.
