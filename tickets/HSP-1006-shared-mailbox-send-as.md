# HSP-1006 — Shared Mailbox: Full Access Works but Send As Is Missing

Lab case for fictional Harbour Street Partners. Synthetic user. Not a real customer.

## User
Priya Nair

## Category
Incident

## User Report
Priya Nair reported that she could open and read the Accounts shared mailbox, but attempts to send messages from the Accounts address failed with a permission error.

## Business Impact
The issue affected one user and one shared mailbox. Priya could still read the Accounts mailbox, but she could not send messages using the Accounts address, preventing normal outbound communication from the shared mailbox.

## Priority
P3 — One user was affected. The shared mailbox remained accessible for reading, but Priya could not send from the shared address. There was no organisation-wide outage or complete loss of mailbox access.

## Questions Asked
- Can you open the Accounts shared mailbox?
- Can you read messages in the mailbox?
- What happens when you try to send from the Accounts address?
- Does the error occur only when sending as Accounts?

## Initial Hypothesis
Because this was a staged lab incident, the suspected cause was incomplete shared-mailbox delegation: Priya appeared to have Full Access but might not have Send As permission. I verified the two permissions separately before making any change.

## Evidence Collected
- `01-full-access-works.png` — Priya successfully opened the Accounts shared mailbox.
- `02-send-as-failure.png` — initial Send As attempt failed.
- `03-full-access-before.txt` and `03-full-access-before.png` — `Get-MailboxPermission` confirmed Priya had `FullAccess`.
- `04-send-as-before.txt` and `04-send-as-before.png` — `Get-RecipientPermission` showed no `SendAs` entry for Priya.
- `05-send-as-grant.png` — PowerShell output from granting Priya `SendAs`.
- `06-send-as-after.txt` and `06-send-as-after.png` — post-change permission check confirmed Priya had `SendAs`.
- `07-post-change-send-failure-details.png` — Outlook still reported that Priya did not have permission immediately after the change.
- `08-post-grant-propagation-delay.png` — a later retry still failed while the permission change was propagating.
- `09-send-as-success.png` — successful verification message received from the Accounts shared mailbox.

## Troubleshooting
1. Confirmed that Priya could open the Accounts shared mailbox successfully in Outlook on the web.
2. Attempted to send a message using the Accounts address and reproduced the error: "Couldn't send this message."
3. Used `Get-MailboxPermission` to check mailbox access permissions.
4. Confirmed that Priya had `FullAccess` to the Accounts shared mailbox.
5. Used `Get-RecipientPermission` to check Send As permissions separately.
6. Confirmed that Priya did not have a `SendAs` permission entry. Only `NT AUTHORITY\SELF` was listed with `SendAs`.
7. Granted Priya `SendAs` using Exchange Online PowerShell.
8. Used `Get-RecipientPermission` again and confirmed that Priya was now listed with `SendAs`.
9. Retested sending from the Accounts mailbox immediately after the change. Outlook still reported that Priya did not have permission to send from the mailbox.
10. Allowed time for the Exchange Online permission change to propagate and started a fresh Outlook session.
11. Retested sending from the Accounts address.
12. The verification message was successfully delivered to Priya with Accounts shown as the sender.

## Root Cause
Priya had Full Access to the Accounts shared mailbox but did not have the separate Send As permission.

Full Access allowed her to open and read the shared mailbox, but it did not authorise her to send messages using the Accounts address.

## Resolution
Granted Priya the Send As permission on the Accounts shared mailbox using Exchange Online PowerShell:

`Add-RecipientPermission -Identity accounts -Trustee priya.nair@DanielChisasuraTech.onmicrosoft.com -AccessRights SendAs -Confirm:$false`

No additional mailbox permissions were added.

After the change, the Send As permission was verified with `Get-RecipientPermission`.

The permission did not become usable immediately in Outlook, so no additional configuration changes were made. After allowing time for the Exchange Online permission change to propagate and starting a fresh Outlook session, sending as Accounts succeeded.

## Verification
Verification was performed from both the administrative and affected-user perspectives.

### Administrative verification
`Get-RecipientPermission` showed:

- Trustee: `priya.nair@DanielChisasuraTech.onmicrosoft.com`
- Access right: `SendAs`
- Inherited: `False`

### User verification
Priya opened the Accounts shared mailbox and sent a verification message using:

`accounts@DanielChisasuraTech.onmicrosoft.com`

The message was successfully delivered to Priya's inbox with Accounts shown as the sender.

This confirmed that:
- Full Access remained functional.
- Send As was now functional.
- The issue was resolved without granting unnecessary additional permissions.

## User Communication
Simulated user update:

The Accounts mailbox access was working correctly, but the permission required to send messages using the Accounts address was missing. Send As access was added and verified. After the permission change propagated, a test message sent from the Accounts address was delivered successfully.

## Escalation
Not escalated.

The issue was isolated to Exchange Online shared-mailbox permissions and was resolved within the lab using the available Exchange Online administrative tools.

## Closure Notes
Priya could open the Accounts shared mailbox but could not send messages from the Accounts address.

Investigation confirmed:
- `FullAccess` was assigned to Priya.
- `SendAs` was not assigned to Priya.

`SendAs` was granted using `Add-RecipientPermission`.

The permission was confirmed with `Get-RecipientPermission`. The first post-change Outlook tests continued to fail while the permission change was propagating. After allowing time for propagation and starting a fresh Outlook session, a message sent from Accounts was successfully delivered.

Ticket resolved and verified.

## Related KB / SOP
KB article to be created from this case:

`Shared Mailbox — Full Access vs Send As`

## Linked tickets / assets
Related context:
- `HSP-1005` — Multi-User Email Delivery Incident

No physical device asset was required for this case.
