# Security: PII, Secret & AI Redaction Guard for Jira

Updated 10 November 2026

How the Atlassian Marketplace app **PII, Secret & AI Redaction Guard for Jira**, made by EvidencePair LLC, handles data, what protects it, how we handle vulnerabilities, and which certifications apply.

## Data handling

- **Forge only.** The app runs on Atlassian Forge (`nodejs24.x`) on Atlassian infrastructure. Atlassian lists it as eligible for the Runs on Atlassian program.
- **No egress.** The manifest declares no external permissions and no remote services. The app makes requests only to your site's Jira APIs, and it loads no analytics, error-reporting services or external assets.
- **What it reads.** To find personal data and secrets, the app reads an issue's summary, description, labels, status and all its comments, page by page, and the project's redaction policy from the project property `safeRedactPolicy`. On the project page it lists the project's 25 newest issues. If Jira refuses any read, the app shows the error and does not scan or change the issue; it never treats a refused read as "no comments" or "no certificate".
- **What it writes.** When a user runs a redaction, the app writes the masked summary, description and comments back to the issue, and adds the `sanitized-for-ai` label if the project's policy says to. It records a certificate in the issue property `safeRedactAudit`: who ran it, when, which kinds of data this run found, how many, in which fields, and a SHA-256 checksum. The certificate never holds the original values. If Jira refuses any of these writes, the issue panel shows which one and Jira's reason, and no certificate is written. A redaction that finds nothing changes nothing and writes no certificate. When a project admin saves the project's redaction policy (which kinds of data to look for, and whether to label redacted issues), the app stores it in the project property `safeRedactPolicy`. If Jira refuses a save, the page shows Jira's reason and nothing is changed. The app changes an issue only when someone runs a redaction on it.
- **No storage of its own.** Everything the app keeps is in Jira itself, and EvidencePair has no copy. It uses no Forge storage or Forge SQL.
- **Requests run as the user.** Every Jira call is made as the signed-in user, so Jira applies that person's own permissions. Saving the redaction policy needs the Administer Projects permission on the project. The app works only on the issue or project Jira says the user is viewing.
- **Logs.** The app writes no log entries of its own. When a request fails, Forge records the error in the app's logs, which EvidencePair can see in Atlassian's developer console. The app's own error messages contain only HTTP status codes, issue keys, project IDs and keys, and Jira's error text, never issue content.
- **Permissions it asks for.**
  - `read:jira-work`: read issues, their comments, the project's policy, the certificate and the recent-issues list.
  - `write:jira-work`: write masked text back to issues and comments, and save the certificate.
  - `manage:jira-project`: save the project's redaction policy. Jira allows an app to set a project property only with this scope. The app uses it for nothing else.

  Version 4.0.0 added `manage:jira-project` and removed `read:app-data:jira` and `write:app-data:jira`, which earlier versions declared but never used.
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
