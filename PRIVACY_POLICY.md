# Privacy policy: PII, Secret & AI Redaction Guard for Jira

Updated 9 November 2026

This policy describes the Atlassian Marketplace app **PII, Secret & AI Redaction Guard for Jira** ("the app"), made by EvidencePair LLC ("EvidencePair", "we"). EvidencePair's company privacy policy, which covers our website and email you send us, is at <https://evidencepair.com/privacy/>.

## In short

- The app runs on Atlassian Forge, inside your Jira site. It declares no external egress and no remote services, and it makes requests only to your site's Jira APIs. Every request is made as the signed-in user.
- It looks for personal data and secrets in an issue's summary, description and comments. When a user asks, it replaces what it finds with placeholders in the issue itself. It keeps no copy of the original values.
- It keeps its own data in Jira, as one issue property on each issue where someone ran a redaction. That data is never sent to EvidencePair.
- The only personal data the app stores itself is the Atlassian account ID of the person who ran a redaction.

## What the app stores

**One issue property, `safeRedactAudit`, per issue where someone ran a redaction (the redaction certificate):**

- the issue key and the project ID;
- the Atlassian account ID of the person who ran the redaction, and when;
- how many matches it found, which kinds (for example `SSN` or `EMAIL_PII`), how many of each, and the fields they were in, such as `summary`, a position in the description, or a comment's ID;
- the label it applied;
- a SHA-256 checksum of the account ID, issue key, total count, kinds and time, which lets the app tell whether those values have changed since. The checksum adds no information of its own.

The certificate never holds the values the app found. Before saving it, the app checks it for anything that looks like one of the kinds of data it finds, and stops without saving if it sees one. Each run replaces the issue's previous certificate. A run that finds nothing still writes a certificate: it repeats the previous certificate's counts, or, on a first run, records zero matches.

**Changes to the issue itself, when a user runs a redaction:**

- each match in the summary, description and comments is replaced with a placeholder such as `[REDACTED_SSN]`, and the rest of the text and its formatting stay as they were;
- the label `sanitized-for-ai`, or the label the policy names, is added to the issue.

The app makes these changes as the user who runs it, so Jira's permissions decide what that person can change. The app does not check Jira's reply to each change. If Jira refuses one, for example an edit to a comment that person may not edit, the app still writes the certificate.

**The sample button.** The issue panel has a button for trying the app. It replaces the issue's current description with a synthetic example: a made-up email address, API key, AWS key, Social Security number and card number. It works on any issue the user can edit, and the app keeps no copy of the description it replaces.

The app does not look up names, email addresses or profiles. Issue text can contain personal data, and the app reads it as entered.

## What the app reads from Jira

- On the issue panel: the issue's summary, description, labels and status, its comments, the project's redaction policy and the issue's certificate.
- On the project page: the policy, and the 25 most recently created issues in the project (key, summary, description, labels and status) with each one's certificate. It scans their summaries and descriptions. It does not read their comments.

It reads no attachments, custom fields or work logs.

The policy is a project property, `safeRedactPolicy`. It holds settings only: the project key and ID, the kinds of data to find, which fields to scan, the label, and when it was last updated. The project page has a button to save a policy there. In this version the save does not work: the app raises an error before it sends anything to Jira, so the app does not write this property. The app reads the property if one exists. Otherwise it uses its defaults: all five kinds of data, in summaries, descriptions and comments.

It looks for five kinds of data: AWS access key IDs; API keys and bearer tokens (`sk_live_`, `sk_test_`, `ghp_`, `xox` tokens and `Bearer` values); US Social Security numbers; payment card numbers; and email addresses. It finds them by their format alone. It can therefore miss data written another way, and replace numbers or text that only look like one of these.

While it scans, the issue panel shows each match in masked form, for example `***-**-6789 -> [REDACTED_SSN]`. The masked form keeps part of the value: the last four digits of a Social Security or card number, the first and last four characters of an AWS key, the start and last four characters of a token, and the first and last letters of an email address's name with its full domain. The app shows these on screen and does not store them.

## Where the data lives

Entity properties are part of your Jira site's data. Atlassian stores them with the rest of your site, in your site's data residency location, under Atlassian's own security and privacy commitments. The app has no storage of its own: it uses no Forge storage and no Forge SQL, and EvidencePair runs no servers in its path. The app's Forge function holds issue text only while a request runs. The masked previews and the CSV it builds go to the user's browser and are not kept.

## Who can see it

Jira shows an entity property to anyone who can view the issue it belongs to, through Jira's REST API. Anyone who can see an issue can therefore read its certificate, including the account ID of the person who ran the redaction. The issue panel shows the certificate too.

The project page shows a table and a CSV of the recent issues to anyone who opens it. The CSV includes each issue's summary, its certificate and the account ID in it. Because the app calls Jira as that person, the list holds only issues they can see. The app does not save the CSV, and copies people make of it are outside the app.

Jira's permissions also decide who can change a certificate: changing an issue property needs permission to edit the issue.

## Retention and deletion

The app keeps each certificate until someone deletes it or a later redaction on that issue replaces it. It has no retention setting, runs no scheduled jobs, and has no uninstall step. Deleting an issue deletes its properties. A Jira administrator can delete a certificate at any time through Jira's REST API, and should do so before uninstalling if the data must go.

A redaction changes only the current summary, description and comments. The app keeps no copy of the original values and cannot restore them. It does not edit the issue's history, and Jira's history of the issue can still show earlier values of the summary and description.

## Account closure

The app does not take part in Atlassian's personal data reporting. When an Atlassian account is closed, its account ID stays in any certificate that person created until that certificate is replaced or deleted, or the issue is deleted.

## Platform logs

The app writes nothing to the Forge platform's logs. The errors it raises itself name the problem and contain no issue text. Atlassian's platform can still record errors it sees while the app runs, such as an unhandled error and its message ([Atlassian: view app logs](https://developer.atlassian.com/platform/forge/view-app-logs/)).

## Email and support

If you email us, our [company privacy policy](https://evidencepair.com/privacy/) describes how we handle that email, and the current restrictions on where our apps may be used.

## What we never do

We do not sell personal data or use it for unrelated advertising or AI training. The app sends nothing to any AI service. Its data stays in your Jira site and never reaches us.

## Changes to this policy

The date at the top changes when this policy does. Every version is kept in this repository's history.

## Contact

hello@evidencepair.com
