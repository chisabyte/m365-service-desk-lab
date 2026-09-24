# Outlook on the Web Access — Check Licence, Mailbox and Client Setting

## Purpose

Use this workflow when one Microsoft 365 user can sign in but Outlook on the web will not open their mailbox. Check licensing, mailbox presence and the user's client access setting separately before changing anything.

## Establish the symptom

- Identify the affected user and the exact client: Outlook on the web, classic Outlook, new Outlook for Windows, or a mobile app.
- Ask whether the issue affects one person or others.
- Record the visible error without assuming a particular error code proves the cause.
- If a page loaded from an existing session, retry a fresh application load before declaring the access check successful.

## Check the assigned service

In the Microsoft 365 admin centre, open the affected user under **Active users → Licenses and apps**. Confirm an appropriate Microsoft 365 licence and its **Exchange Online** service plan are assigned. In the user's **Mail** tab, confirm the mailbox is present.

An assigned licence and visible mailbox do not prove that every email client is allowed to open it.

## Check mailbox client access

Open the user's **Mail → Manage email apps** panel and inspect **Outlook on the web**. The administrative summary may show **OWA blocked** when this setting is disabled.

Exchange Online PowerShell provides a separate configuration check when an authenticated admin session is available:

```powershell
Get-CASMailbox -Identity user@tenant.example |
    Select-Object DisplayName,OWAEnabled,MAPIEnabled,EwsEnabled,ActiveSyncEnabled
```

The `OWAEnabled` value is the mailbox client access setting to investigate for Outlook on the web. Preserve the current value before changing it.

## Correct the specific setting

If the user's licence and mailbox are intact and Outlook on the web should be allowed, enable **Outlook on the web** for that mailbox in **Manage email apps** and save. An equivalent targeted Exchange Online PowerShell change is:

```powershell
Set-CASMailbox -Identity user@tenant.example -OWAEnabled $true
```

Avoid changing other email app controls or tenant-wide policies unless separate evidence requires it.

## Verify as the affected user

Check that the admin setting saved, then load Outlook again in the affected user's session. Confirm the account context and that the mailbox view actually opens. State the scope of the check: opening a message list does not by itself prove sending, receiving, or other client protocols work.

If an existing session appears to work immediately after a change, use a fresh application load before drawing a conclusion. Record any delay or inconsistent state that occurred.

## Evidence handling

Capture the baseline setting, user-visible failure, relevant licence and mailbox checks, the targeted correction, and a user-context success check. A written analyst note must be labelled as a note; it is not screenshot or raw command proof. Remove passwords, MFA codes, tokens, session identifiers and unrelated personal data before publishing evidence.

## Related lab case

[HSP-1011 — Outlook on the Web Client Access Failure](../tickets/HSP-1011-licence-mailbox-client-access.md) staged this setting for synthetic user Maya Chen. Her Outlook first loaded, then a fresh load failed while Outlook on the web was blocked. Access returned after the targeted setting was restored.

## Microsoft references

- [Managing email apps for user mailboxes](https://learn.microsoft.com/en-us/exchange/recipients-in-exchange-online/manage-user-mailboxes/managing-email-apps-for-user-mailboxes)
- [Enable or disable POP3, IMAP, MAPI, Outlook Web App or ActiveSync in Microsoft 365](https://learn.microsoft.com/en-us/troubleshoot/exchange/user-and-shared-mailboxes/pop3-imap-owa-activesync-office-365)
