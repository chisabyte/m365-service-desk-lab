# HSP-1010 — Single-User Missing-Mail / Inbox-Rule Investigation

Lab case for fictional Harbour Street Partners. Synthetic users. Not a real customer.

## User
Priya Nair

## Category
Incident

## User Report
Priya Nair reported that an expected message from Maya Chen was missing from her Inbox.

The issue appeared to affect Priya only. Maya had sent the message successfully, but Priya could not see it in her Inbox.

## Business Impact
The issue affected one user and one message flow into Priya's mailbox.

There was no evidence of an organisation-wide Exchange Online mail-delivery outage.

Priya could continue accessing Outlook, but expected mail was not appearing in the Inbox.

## Priority
P3 — One user was affected and Outlook remained available. The issue prevented normal visibility of an expected message but did not cause a wider service outage.

## Questions Asked
- Who sent the missing message?
- Which recipient was affected?
- Was the problem limited to Priya or affecting multiple users?
- What was the subject of the expected message?
- Was there any current Exchange Online service issue that matched the symptoms?
- Did Exchange Online record the message as delivered?
- If Exchange delivered the message, where did it go inside Priya's mailbox?

## Initial Hypothesis
Because this was a staged lab incident, the investigation was designed to determine whether the apparent missing-mail problem was caused by Exchange mail flow, a Microsoft service issue, or mailbox-side behaviour.

I did not treat the missing Inbox message as proof of an Exchange Online delivery failure.

The investigation first checked service scope and message delivery before examining Priya's mailbox rules.

## Evidence Collected
- `01-inbox-rules-baseline.txt` — documentation note recording that the pre-staging `Get-InboxRule` check returned no existing inbox rules for Priya.
- `02-staged-inbox-rule.txt` — staged rule configuration used to reproduce the incident.
- `03-message-missing-from-inbox.png` — Priya's Inbox did not contain the expected test message.
- `04-service-health-check.png` — Microsoft 365 Service Health showed one Exchange Online advisory.
- `05-service-health-advisory-unrelated.png` — the Exchange advisory concerned unwanted Outlook add-ins and did not match the missing-mail symptom.
- `06-message-trace.txt` and `06-message-trace-delivered.png` — Exchange Online message trace recorded the test message as `Delivered`.
- `07-message-found-deleted-items.png` — the supposedly missing message was found in Priya's Deleted Items.
- `08-inbox-rule-root-cause.txt` and `08-inbox-rule-root-cause.png` — inbox-rule inspection showed an enabled rule matching the test subject and deleting the message.
- `09-rule-disabled.txt` and `09-rule-disabled.png` — post-change evidence showed the staged rule was disabled.
- `10-message-delivered-to-inbox.png` — a new verification message from Maya appeared normally in Priya's Inbox after remediation.

## Troubleshooting
1. Confirmed that Priya and Maya both existed as Exchange Online user mailboxes.
2. Recorded Priya's inbox-rule baseline before staging the fault.
3. Created a staged inbox rule matching the subject `HSP-1010 Missing Mail Test` and configured it to delete matching messages.
4. Maya sent a test message to Priya using the matching subject.
5. Confirmed that the message did not appear in Priya's Inbox.
6. Checked Microsoft 365 Service Health.
7. Found an active Exchange Online advisory, but reviewed its details instead of assuming it explained the incident.
8. Confirmed that the advisory concerned unwanted Outlook add-ins appearing automatically and was unrelated to message delivery.
9. Used `Get-MessageTraceV2` to trace the message from Maya to Priya.
10. Message trace showed the message status as `Delivered`.
11. Because Exchange had successfully delivered the message, shifted the investigation from mail flow to Priya's mailbox.
12. Checked Priya's Deleted Items and found the test message there.
13. Inspected the staged inbox rule using `Get-InboxRule`.
14. Confirmed that the rule:
    - was enabled;
    - matched the subject `HSP-1010 Missing Mail Test`;
    - had `DeleteMessage = True`;
    - stopped further rule processing.
15. Disabled the rule.
16. Verified with `Get-InboxRule` that the rule remained present but had `Enabled = False`.
17. Maya sent a new verification message to Priya.
18. Confirmed that the verification message appeared normally in Priya's Inbox.

## Root Cause
An enabled inbox rule in Priya's mailbox matched the test-message subject and automatically deleted the message.

Exchange Online had successfully delivered the message to Priya's mailbox. The message was then moved to Deleted Items by mailbox-side rule processing.

The issue was therefore not an Exchange Online transport failure and was not caused by the unrelated Service Health advisory.

## Resolution
Disabled the inbox rule responsible for deleting the matching messages.

The rule was disabled rather than immediately deleted so that the configuration remained available for post-change verification and evidence.

No Exchange transport configuration or Microsoft 365 service setting was changed.

## Verification
Verification was performed at both the Exchange Online and affected-user levels.

### Exchange Online verification
Message trace showed the original test message as:

`Status: Delivered`

This established that Exchange Online successfully delivered the message to Priya's mailbox.

### Rule verification
After remediation, `Get-InboxRule` showed:

- Name: `HSP-1010 Staged Missing Mail`
- Enabled: `False`
- Priority: `1`

### User verification
Maya sent a new message with the subject:

`HSP-1010 Missing Mail Verification`

The message appeared normally in Priya's Inbox.

This confirmed that normal Inbox delivery was restored after the rule was disabled.

## User Communication
Simulated user update:

The message was successfully delivered by Exchange Online but was being moved out of the Inbox by an inbox rule in your mailbox. The rule was disabled and a new test message from Maya arrived normally in the Inbox.

## Escalation
Not escalated.

The issue was isolated to a mailbox-side inbox rule and was resolved using the available Exchange Online administrative tools.

No evidence indicated a wider Microsoft 365 mail-delivery incident requiring Microsoft support escalation.

## Closure Notes
Priya reported a missing message from Maya.

Service Health showed an Exchange Online advisory, but its documented impact related to unwanted Outlook add-ins and did not match the incident symptoms.

`Get-MessageTraceV2` confirmed that Exchange Online delivered the test message successfully.

The message was found in Priya's Deleted Items.

`Get-InboxRule` identified an enabled rule matching the test subject and configured with `DeleteMessage = True`.

The rule was disabled.

A new verification message from Maya then appeared normally in Priya's Inbox.

Root cause: mailbox-side inbox rule.

Ticket resolved and verified.

## Related KB / SOP
KB article to be created from this case:

`Single-User Missing Mail — Message Trace and Inbox Rule Investigation`

## Linked tickets / assets
Related case:

- `HSP-1005` — Multi-User Email Delivery Incident

HSP-1005 demonstrates a multi-user Exchange Online mail-flow incident. HSP-1010 demonstrates a single-user mailbox-side problem after Exchange successfully delivered the message.

No physical device asset was required for this case.
