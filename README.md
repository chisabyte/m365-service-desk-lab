# Microsoft 365 Service Desk Lab

Hands-on Microsoft 365 and Microsoft Entra support portfolio documenting incident investigation, identity support, access administration, verification and service-desk records.

> **Lab disclosure**  
> Harbour Street Partners is a fictional Australian professional-services firm created for a personal lab. Every user, mailbox, device and ticket is synthetic test data. This is not commercial employment, a client engagement or a production tenant.

## Environment

The cases use a personal Microsoft 365 Business Basic lab. Evidence shows the Microsoft 365 admin centre, Microsoft Entra sign-in records, Exchange Online mailboxes, mail-flow rules and message trace, Microsoft 365 Service Health, Outlook, Teams, SharePoint, OneDrive and Microsoft Authenticator. [Environment](docs/lab-environment.md) · [Licensing and limits](docs/licensing.md)

## Cases

### HSP-1001 — Microsoft 365 Sign-In Failure

Priya's staged sign-in failure looked like a password problem. The account was blocked; Microsoft Entra recorded error `50057` (disabled account). Sign-in was unblocked without a password reset, and a later OfficeHome event recorded **Success / 0**. [Open case and evidence →](tickets/HSP-1001-signin-failure.md)

### HSP-1002 — MFA Recovery

Priya could not use the Authenticator prompt after a phone replacement. A lab identity-verification note records a matching account and fictional manager approval before the old method was removed. Authenticator was registered again; a later sign-in record shows success, with the verification limit explained in the case. [Open case and evidence →](tickets/HSP-1002-mfa-recovery.md)

### HSP-1003 — New Starter Onboarding

Maya's Finance request covers Business Basic licensing, group and Team membership, mailbox provision and checks in her Outlook, Teams and SharePoint context. [Open case and evidence →](tickets/HSP-1003-new-starter-onboarding.md)

### HSP-1004 — User Offboarding

Owen's staged offboarding covers sign-in containment, mailbox conversion, a lab-administrator handover copy of a OneDrive file, removal of two access groups, licence removal and post-change checks. The evidence does not establish manager access or endpoint wipe. [Open case and evidence →](tickets/HSP-1004-user-offboarding.md)

### HSP-1005 — Multi-User Email Delivery Incident

Priya and Maya's staged messages to Alex failed. Message trace identified a shared Exchange Online mail-flow rule, while the visible Service Health advisories had different symptoms. The rule was disabled; new messages reached Alex and appeared as **Delivered** in a later trace. [Open case and evidence →](tickets/HSP-1005-multi-user-email-delivery.md)

## How to read the evidence

Each case opens with a 30-second summary, followed by clickable primary screenshots and a fuller service-desk record. Supporting baseline and staging images sit in collapsed sections. Captions describe what the image shows; written notes are identified as lab records. [Ticket standard](docs/ticket-standard.md)

## Project Status

| Case | Status |
| --- | --- |
| HSP-1001 | Complete |
| HSP-1002 | Complete, with verification scope stated |
| HSP-1003 | Complete |
| HSP-1004 | Complete within the documented M365 scope |
| HSP-1005 | Complete, with rule-deletion evidence limit stated |

**Publication milestone:** five completed, reviewed and sanitised Microsoft 365 support cases. The five cases are documented locally; final factual and screenshot review is required before public release.
