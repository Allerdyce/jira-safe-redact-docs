# Privacy, Data Residency & Security Architecture

> The app's privacy policy is [PRIVACY_POLICY.md](./PRIVACY_POLICY.md). Security documentation (data handling, encryption, vulnerability management and certifications) is in [SECURITY.md](./SECURITY.md).

**App Title:** PII, Secret & AI Redaction Guard for Jira  
**App Key:** `com.evidencepair.jira.safe.redact`  
**Vendor:** EvidencePair LLC  
**Privacy Contact:** `hello@evidencepair.com`  
**Security Contact:** `security@evidencepair.com`  
**Support:** <https://evidencepair.com/support/> (`support@evidencepair.com`)

---

## 1. Runs on Atlassian, no external data egress

**PII, Secret & AI Redaction Guard for Jira** runs only inside Atlassian's managed Forge runtime (`nodejs24.x`).
- **No `remotes:` declarations** and no `permissions.external` in `manifest.yml`.
- **No external HTTP/HTTPS calls**, webhooks, analytics trackers, or third-party servers. Every Jira call is made as the signed-in user (`api.asUser()`).
- The data the app keeps stays in your Jira site, which Atlassian stores in your site's data residency location.

---

## 2. Zero-Leak Rule & Jira entity property storage (<32 KB)

This application does **not** use external databases, Forge SQL, or Forge KVS. The application follows a **Zero-Leak Rule**:
> **Never store raw unredacted PII or secrets inside Jira Entity Properties or logs.**
> Store only the pattern type (`AWS_KEY`, `BEARER_TOKEN`, `SSN`, `CREDIT_CARD`, `EMAIL_PII`), match count, field path, and deterministic Web Crypto SHA-256 redaction certificate (`accountId|issueKey|redactedCount|patternTypes|timestamp`) inside Issue Property `safeRedactAudit` (`<32 KB`).

| Property Key | REST v3 Endpoint | Purpose | Size Guard |
| :--- | :--- | :--- | :--- |
| `safeRedactPolicy` | `/rest/api/3/project/{projectId}/properties/safeRedactPolicy` | Project redaction policy: enabled policy packs and label settings. Settings only, no personal data. Written when a project admin saves it, which needs the Administer Projects permission. | `<32 KB` (`32,768` bytes UTF-8) |
| `safeRedactAudit` | `/rest/api/3/issue/{issueKey}/properties/safeRedactAudit` | Zero-leak SHA-256 sanitization certificate: the account ID of the person who ran the redaction, the time, pattern counts, and field paths | `<32 KB` (`32,768` bytes UTF-8) |

The certificate passes through `assertZeroRawSecretsInAuditPayload(payload)` and `assertPropertyPayloadUnder32KB(payload)` before it is saved, and the policy through `assertPropertyPayloadUnder32KB(payload)`. A redaction that finds nothing writes no certificate. The SHA-256 checksum is not a digital signature: someone with permission to edit the issue property could change the certificate and recompute it.

---

## 3. Personal data

The full privacy policy is [PRIVACY_POLICY.md](./PRIVACY_POLICY.md).

- **Nothing leaves Atlassian:** The app does not transmit or store personal data outside your Jira site.
- **In-Place Redaction of Exposed PII:** The app is built to *remove* accidentally pasted PII (`SSN`, `CREDIT_CARD`, `EMAIL_PII`) and cloud credentials (`AWS_KEY`, `BEARER_TOKEN`) from Jira issue summaries, descriptions and comments. It keeps no copy of the original values. It does not edit the issue's history.
- **Account ID Attribution:** When a user sanitizes an issue, their Atlassian `accountId` and the UTC timestamp are recorded in the issue's `safeRedactAudit` property, to show who ran the redaction. Anyone who can view the issue can read it.
- **Personal data reporting:** The app does not take part in Atlassian's personal data reporting. A closed account's ID stays in a certificate until the certificate is replaced or deleted, or the issue is deleted.

---

## 4. Forge scopes

- `read:jira-work`: Read the issue's summary, description, labels, status and all its comments, the project property `safeRedactPolicy`, the issue property `safeRedactAudit`, and the project's 25 newest issues.
- `write:jira-work`: Scrub exposed secrets/PII in place inside issue summaries, descriptions and comments, apply the `sanitized-for-ai` label when the project's policy says to, and write the issue property `safeRedactAudit`.
- `manage:jira-project`: Write the project property `safeRedactPolicy` when a project admin saves the redaction policy. Jira requires this scope to set a project property, and still requires the user to have the Administer Projects permission. Not used for anything else.

Version 4.0.0 added `manage:jira-project` and removed `read:app-data:jira` and `write:app-data:jira`, which earlier versions declared but never used.

---

## 5. Vulnerability Disclosure & Security Contact

To report a security issue, email `security@evidencepair.com`. We acknowledge every report within one business day. EvidencePair holds no security certifications; see [SECURITY.md](./SECURITY.md).
