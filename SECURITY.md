# Security: PII, Secret & AI Redaction Guard for Jira

Updated 9 November 2026

How the Atlassian Marketplace app **PII, Secret & AI Redaction Guard for Jira**, made by EvidencePair LLC, handles data, what protects it, how we handle vulnerabilities, and which certifications apply.

## Data handling

- **Forge only.** The app runs on Atlassian Forge (`nodejs24.x`) on Atlassian infrastructure. Atlassian lists it as eligible for the Runs on Atlassian program.
- **No egress.** The manifest declares no external permissions and no remote services. The app makes requests only to your site's Jira APIs, and it loads no analytics, error-reporting services or external assets.
- **What it reads.** To find personal data and secrets, the app reads an issue's summary, description, labels, status and comments, and the project's redaction policy from the project property `safeRedactPolicy`. On the project page it lists the project's 25 newest issues.
- **What it writes.** When a user runs a redaction, the app writes the masked summary, description, labels and comments back to the issue. It records a certificate in the issue property `safeRedactAudit`: who ran it, when, which kinds of data were found, how many, in which fields, and a SHA-256 checksum. The certificate never holds the original values. A sample button, meant for trying the app, replaces the current issue's description with synthetic test data.
- **No storage of its own.** Everything the app keeps is in Jira itself, and EvidencePair has no copy. It uses no Forge storage or Forge SQL.
- **Requests run as the user.** Every Jira call is made as the signed-in user, so Jira applies that person's own permissions.
- **Logs.** The app writes nothing to Forge's platform logs.
- **Permissions it asks for.**
  - `read:jira-work`: read issues, their comments, the project's policy, the certificate and the recent-issues list.
  - `write:jira-work`: write masked text back to issues and comments, and save the certificate.
  - `read:app-data:jira`, `write:app-data:jira`: declared but not used by this version.
- **What the checksum is.** Each certificate carries a SHA-256 checksum of the account ID, issue key, total count, kinds of data found and time (not the field paths). It shows whether those values still match what was recorded. It is not a digital signature: someone with permission to edit the issue property could change the certificate and recompute the checksum.

## Encryption

The app stores nothing outside your Jira site and sends nothing outside Atlassian's platform. Encryption in transit and at rest is provided by Atlassian as part of your site, as described in the [Atlassian Trust Center](https://www.atlassian.com/trust). The app adds no encryption of its own.

## Vulnerability management

- **Reporting.** Email security@evidencepair.com. Tell us the app, what you found and how to reproduce it. Please do not test against sites you do not own. We acknowledge every report within one business day.
- **Response.** EvidencePair adopted a written security incident response plan on 12 October 2026. It covers triage, containment, investigation, notification of Atlassian and affected customers, and recovery.
- **Dependencies.** The app's npm dependencies are pinned in its lockfile. Atlassian maintains and patches the Forge runtime.
- **Bug bounty.** We do not run a bug bounty programme.

## Certifications

EvidencePair holds no security certifications, and the app has had no independent penetration test or external audit. The infrastructure the app runs on is Atlassian's, and Atlassian's own certifications cover it: see the [Atlassian Trust Center](https://www.atlassian.com/trust).
