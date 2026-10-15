# PII, Secret & AI Redaction Guard for Jira

**Marketplace Title:** PII, Secret & AI Redaction Guard for Jira  
**App Key:** `com.evidencepair.jira.safe.redact`  
**App ID:** `ari:cloud:ecosystem::app/719ce0d1-4562-4b7f-9856-1101074101a3`  
**Developer / Vendor:** EvidencePair LLC  
**Support & Security Contact:** `support@titlecovenant.com`  
**Runtime:** Atlassian Forge `nodejs24.x` (100% *Runs on Atlassian* — Zero External Egress, Zero Forge SQL/KVS)

---

## Overview

**PII, Secret & AI Redaction Guard for Jira** is a 100% *Runs on Atlassian* data loss prevention (DLP), secret scrubber, and **Atlassian Rovo AI readiness guard** for Jira Cloud. It recursively traverses Jira **Atlassian Document Format (ADF)** JSON AST nodes across issue summaries, descriptions, and comments to detect and redact exposed cloud API keys, bearer tokens, Social Security Numbers (HIPAA), payment card PANs (PCI-DSS), and personal emails (GDPR PII) in-place—without breaking rich-text formatting—and issues a deterministic **Web Crypto SHA-256 Sanitization Certificate** while applying the `sanitized-for-ai` label.

### Key Capabilities

1. **Space Redaction Policy Engine (`jira:projectPage` — `jira-safe-redact-admin`)**
   - Enable and customize compliance regex policy packs: **Cloud Secrets & API Keys** (`AWS_KEY`, `BEARER_TOKEN`), **HIPAA** (`SSN`), **PCI-DSS** (`CREDIT_CARD`), and **GDPR PII** (`EMAIL_PII`).
   - Stored in native Jira Project Property `safeRedactPolicy` (`<32 KB` enforced).

2. **ADF-Safe Issue & Comment Scrubber + Zero-Leak Dry-Run Preview (`jira:issuePanel` — `jira-safe-redact-panel`)**
   - Recursively walks Jira ADF AST nodes (`paragraph`, `text`, `codeBlock`, `bulletList`, `table`, `panel`, inline `marks`) in issue `description`, `summary`, and `comment` bodies.
   - Displays a masked dry-run preview (`sk_live_...9f2a -> [REDACTED_API_KEY]`, `AKIA...MPLE -> [REDACTED_AWS_KEY]`, `***-**-6789 -> [REDACTED_SSN]`) without exposing full raw tokens.
   - Executes 1-click in-place ADF sanitization without breaking rich-text formatting.

3. **Zero-Leak SHA-256 Sanitization Certificate & Rovo AI Safe Labeling (`safeRedactAudit`)**
   - **Critical Zero-Leak Rule**: Never stores raw unredacted PII or secrets inside Jira Entity Properties or logs.
   - Stores only the pattern type, match count, field path, and deterministic SHA-256 redaction certificate (`accountId|issueKey|redactedCount|patternTypes|timestamp`) inside Jira Issue Property `safeRedactAudit` (`<32 KB`).
   - Applies the `sanitized-for-ai` label and provides a space-wide JQL batch scanner with 1-click compliance CSV export.

---

## Documentation Index

- [User & Administrator Guide](./USER_GUIDE.md)
- [Privacy, Data Residency & Security Architecture](./PRIVACY_AND_SECURITY.md)
- [End-User License Agreement (EULA)](./EULA.md)
- [Atlassian Marketplace Listing Copy](./LISTING_COPY.md)
- [Cloud Per-User Pricing Tiers](./PRICING_TABLE.md)
