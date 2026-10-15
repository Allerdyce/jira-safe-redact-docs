# Privacy, Data Residency & Security Architecture

**App Title:** PII, Secret & AI Redaction Guard for Jira  
**App Key:** `com.evidencepair.jira.safe.redact`  
**Vendor:** EvidencePair LLC  
**Security Contact:** `support@titlecovenant.com`

---

## 1. 100% Runs on Atlassian — Zero External Data Egress

**PII, Secret & AI Redaction Guard for Jira** runs exclusively inside Atlassian's managed Forge runtime (`nodejs24.x`).
- **Zero `remotes:` declarations** in `manifest.yml`.
- **Zero external HTTP/HTTPS calls**, webhooks, analytics trackers, or third-party servers.
- All data remains strictly within your Atlassian Cloud tenant boundary and inherits your Jira Cloud data residency controls.

---

## 2. Critical Zero-Leak Security Rule & Host-Native Entity Property Storage (<32 KB)

This application does **not** use external databases, Forge SQL, or Forge KVS. Furthermore, the application strictly enforces a **Zero-Leak Rule**:
> **Never store raw unredacted PII or secrets inside Jira Entity Properties or logs.**
> Store only the pattern type (`AWS_KEY`, `BEARER_TOKEN`, `SSN`, `CREDIT_CARD`, `EMAIL_PII`), match count, field path, and deterministic Web Crypto SHA-256 redaction certificate (`accountId|issueKey|redactedCount|patternTypes|timestamp`) inside Issue Property `safeRedactAudit` (`<32 KB`).

| Property Key | REST v3 Endpoint | Purpose | Size Guard |
| :--- | :--- | :--- | :--- |
| `safeRedactPolicy` | `/rest/api/3/project/{projectId}/properties/safeRedactPolicy` | Space-level enabled compliance policy packs and Rovo AI label settings | `<32 KB` (`32,768` bytes UTF-8) |
| `safeRedactAudit` | `/rest/api/3/issue/{issueKey}/properties/safeRedactAudit` | Zero-leak SHA-256 sanitization certificate, pattern counts, and field paths | `<32 KB` (`32,768` bytes UTF-8) |

Every write operation passes through `assertZeroRawSecretsInAuditPayload(payload)` and `assertPropertyPayloadUnder32KB(payload)` prior to persistence.

---

## 3. Personal Data & Privacy Policy

- **No Personal Data Collection:** The app does not collect, transmit, or store personal data outside Atlassian Cloud.
- **In-Place Redaction of Exposed PII:** The app is specifically engineered to *remove* accidentally pasted PII (`SSN`, `CREDIT_CARD`, `EMAIL_PII`) and cloud credentials (`AWS_KEY`, `BEARER_TOKEN`) from Jira issues and comments.
- **Account ID Attribution:** When an operator sanitizes an issue, their opaque Atlassian `accountId` (e.g., `712020:...`) and UTC timestamp are recorded inside the issue's `safeRedactAudit` property solely for internal compliance attribution within your Jira site.

---

## 4. Least-Privilege Forge Scopes

- `read:jira-work` — Read project issues, comments, and entity properties (`safeRedactPolicy`, `safeRedactAudit`).
- `write:jira-work` — Scrub exposed secrets/PII in-place inside issue descriptions/summaries/comments, apply the `sanitized-for-ai` label, and write entity properties (`safeRedactPolicy`, `safeRedactAudit`).
- `read:app-data:jira` / `write:app-data:jira` — Read/write app-scoped Jira metadata.

---

## 5. Vulnerability Disclosure & Security Contact

To report a security issue or request compliance documentation (SOC 2 / ISO 27001 / HIPAA / PCI-DSS / CAIQ), email `support@titlecovenant.com`. Security inquiries receive an initial response within 24 hours.
