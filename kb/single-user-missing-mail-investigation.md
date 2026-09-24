# Single-User Missing Mail — Message Trace and Inbox Rule Investigation

## Purpose

Use this workflow when one Microsoft 365 user reports that an expected email is missing.

The goal is to determine whether the problem is:

- a wider Microsoft 365 service issue;
- an Exchange Online transport problem; or
- mailbox-side behaviour affecting only the user.

## 1. Establish scope

Ask:

- Who sent the message?
- Who was the recipient?
- What was the subject?
- Approximately when was it sent?
- Is one user affected or are multiple users affected?
- Are other messages arriving normally?

A single-user symptom should not automatically be treated as a Microsoft service outage.

## 2. Check Microsoft 365 Service Health

Review Exchange Online advisories and incidents.

Do not stop at seeing that an advisory exists.

Read:

- user impact;
- affected feature;
- scope;
- current status.

An Exchange Online advisory may be unrelated to the reported mail problem.

## 3. Trace the message

Use Exchange Online message trace to determine what happened to the message.

Example:

```powershell
Get-MessageTraceV2 `
    -SenderAddress sender@tenant.example `
    -RecipientAddress recipient@tenant.example `
    -Subject "Expected Subject" `
    -SubjectFilterType StartsWith
```

Important statuses include:

* `Delivered`
* `Failed`
* `Pending`
* `Quarantined`

If the message is recorded as `Delivered`, shift the investigation from transport to the recipient mailbox.

## 4. Check mailbox folders

Check locations including:

* Inbox
* Deleted Items
* Junk Email
* Archive

A message can be delivered successfully by Exchange and then moved by mailbox processing.

## 5. Inspect inbox rules

Example:

```powershell
Get-InboxRule -Mailbox recipient@tenant.example |
    Format-List Name,Enabled,Priority,SubjectContainsWords,DeleteMessage,Description
```

Look for rules that:

* move messages;
* delete messages;
* redirect or forward messages;
* match the sender or subject;
* stop processing additional rules.

Do not disable every rule without first identifying the one that explains the symptom.

## 6. Remediate the specific cause

If an incorrect rule is responsible, disable or correct that rule according to the required outcome.

Example:

```powershell
Disable-InboxRule `
    -Mailbox recipient@tenant.example `
    -Identity "Problem Rule" `
    -Confirm:$false
```

## 7. Verify

Do not close the ticket immediately after changing the rule.

Send a new test message and verify from the affected user's mailbox that:

* the message arrives;
* it appears in the expected folder;
* the unwanted rule behaviour no longer occurs.

## Key Troubleshooting Principle

`Delivered` in message trace means Exchange successfully delivered the message to the mailbox.

It does not necessarily mean the message remained visible in the user's Inbox.

Mailbox rules and other mailbox-side behaviour must be investigated separately.

## Related Lab Case

[HSP-1010 — Single-User Missing-Mail / Inbox-Rule Investigation](../tickets/HSP-1010-single-user-missing-mail.md)

This article was produced from a staged personal Microsoft 365 lab using synthetic users and messages. It does not describe a real customer or production environment.
