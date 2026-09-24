# Shared Mailbox — Full Access vs Send As

## Purpose

This article explains the difference between Full Access and Send As permissions for an Exchange Online shared mailbox and provides a basic troubleshooting workflow when a user can open a shared mailbox but cannot send messages from its address.

## Symptoms

A user may report that:

- The shared mailbox opens successfully.
- Messages and folders can be read.
- The user can work inside the mailbox.
- Sending from the shared mailbox address fails.
- Outlook reports that the user does not have permission to send from the mailbox.

This usually means mailbox access and sending permissions need to be checked separately.

## Permission Types

### Full Access

Full Access allows a delegated user to open the shared mailbox and work with its contents.

Typical capabilities include:

- Opening the mailbox.
- Reading messages.
- Managing folders and messages.

Full Access does **not** automatically grant permission to send using the shared mailbox address.

### Send As

Send As allows a delegated user to send a message that appears to come directly from the shared mailbox.

For example:

`From: accounts@company.example`

The recipient sees the shared mailbox as the sender.

### Send on Behalf

Send on Behalf is different from Send As.

A message sent using Send on Behalf identifies both the user and the mailbox, for example:

`Priya Nair on behalf of Accounts`

Only the permission required by the business request should be granted.

## Troubleshooting Workflow

### 1. Confirm mailbox access

First determine whether the user can open the shared mailbox.

If the mailbox opens successfully, Full Access may already be working.

### 2. Check Full Access

Exchange Online PowerShell:

```powershell
Get-MailboxPermission -Identity accounts
```

For a specific user:

```powershell
Get-MailboxPermission -Identity accounts |
    Where-Object {$_.User -eq "user@tenant.example"} |
    Select-Object User,AccessRights,IsInherited,Deny
```

Confirm that the expected user has:

`FullAccess`

### 3. Check Send As separately

Run:

```powershell
Get-RecipientPermission -Identity accounts
```

Confirm whether the affected user has:

`SendAs`

Do not assume that Full Access includes Send As.

### 4. Grant Send As when authorised

If Send As is required and approved:

```powershell
Add-RecipientPermission `
    -Identity accounts `
    -Trustee user@tenant.example `
    -AccessRights SendAs `
    -Confirm:$false
```

Grant only the permission required for the request.

### 5. Verify the permission

Run:

```powershell
Get-RecipientPermission -Identity accounts |
    Where-Object {$_.Trustee -eq "user@tenant.example"} |
    Select-Object Trustee,AccessRights,IsInherited
```

The user should appear with:

`SendAs`

### 6. Verify as the affected user

Administrative configuration alone is not sufficient verification.

Sign in as the affected user and:

1. Open the shared mailbox.
2. Compose a new message.
3. Select the shared mailbox address in the From field.
4. Send a test message.
5. Confirm that the delivered message shows the shared mailbox as the sender.

## Propagation

Exchange Online permission changes may not become usable immediately.

If the permission is visible in PowerShell but Outlook still reports a permission error:

* Do not repeatedly remove and re-add the permission.
* Allow time for the change to propagate.
* Start a fresh Outlook or browser session.
* Retest the operation.

Only make additional configuration changes if evidence shows the original permission change did not resolve the issue.

## Key Troubleshooting Principle

**Opening a shared mailbox and sending as that mailbox are separate capabilities.**

A user who can successfully open and read a shared mailbox may still require an additional Send As permission before they can send messages using the shared address.

## Related Lab Case

[HSP-1006 — Shared Mailbox: Full Access Works but Send As Is Missing](../tickets/HSP-1006-shared-mailbox-send-as.md)

This article was produced from a staged personal Microsoft 365 lab incident using synthetic users and mailboxes. It does not describe a real customer or production environment.
