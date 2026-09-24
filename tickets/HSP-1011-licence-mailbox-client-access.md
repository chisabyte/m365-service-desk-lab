# HSP-1011 — Outlook on the Web Client Access Failure

Lab case for fictional Harbour Street Partners. Maya Chen is a synthetic user. This is not a real customer incident.

**Problem.** Maya's Outlook on the web mailbox stopped loading after a staged change to her mailbox's email app settings.

**Finding.** Microsoft 365 Business Basic and its Exchange Online (Plan 1) service remained assigned, and Maya's mailbox was present. The admin centre showed Outlook on the web blocked. In Maya's browser session, a fresh Outlook load failed twice with `Error 440` and `StartupData` in the visible diagnostics.

**Action.** Re-enabled only Outlook on the web for Maya in the Microsoft 365 admin centre.

**Result.** The admin centre confirmed the setting was enabled, and Maya's Outlook mailbox loaded again with her message list visible.

**Evidence.** The linked text files are analyst notes of observed admin and user screens, not raw command output. A cropped screenshot shows Maya's restored Outlook session. The first sign-in briefly displayed her mailbox before a fresh load reproduced the failure; that sequence is retained below.

## User

Maya Chen

## Category

Incident — Exchange Online client access

## User Report

In this staged lab case, Maya's Outlook on the web mailbox failed to load on a fresh application load. No live user interview took place.

## Business Impact

One synthetic user could not reliably open her mailbox through Outlook on the web. Maya's Microsoft 365 account and Exchange Online service plan remained assigned. The case did not test whether other email clients continued working, and no wider outage was observed.

## Priority

P3 — One user's web mailbox access was affected. The available evidence did not show a wider service outage or loss of the mailbox itself. This is a lab triage judgement, not a production SLA.

## Questions Asked

No live interview was conducted. The investigation checked these questions in the lab:

- Is Maya still licensed for Exchange Online?
- Is her mailbox visible in the admin centre?
- Is Outlook on the web enabled for her mailbox?
- What happens in Maya's own Outlook session after a fresh load?
- Does access return when the specific client access setting is restored?

## Initial Hypothesis

The staged change disabled Maya's Outlook on the web setting while leaving her licence and other visible email app controls alone. The working hypothesis was a mailbox client access restriction. I checked the licence and mailbox state and reproduced the user-visible failure before treating that setting as the cause.

## Evidence Collected

- [01-maya-email-apps-baseline.txt](../evidence/HSP-1011/01-maya-email-apps-baseline.txt) — analyst note of the detailed pre-change email app controls, including Outlook on the web enabled.
- [02-maya-owa-block-staged.txt](../evidence/HSP-1011/02-maya-owa-block-staged.txt) — analyst note of the targeted change and admin confirmation.
- [03-maya-licence-mailbox-check.txt](../evidence/HSP-1011/03-maya-licence-mailbox-check.txt) — analyst note of the assigned Business Basic licence, checked Exchange Online service plan, and mailbox panel.
- [04-maya-owa-user-failure.txt](../evidence/HSP-1011/04-maya-owa-user-failure.txt) — analyst note of Maya's first load, failed fresh load, and failed retry. Session and request identifiers were omitted.
- [05-maya-owa-restored.txt](../evidence/HSP-1011/05-maya-owa-restored.txt) — analyst note of the narrow configuration correction and admin confirmation.
- [06-maya-owa-user-verification.txt](../evidence/HSP-1011/06-maya-owa-user-verification.txt) — analyst note of Maya's restored Outlook mailbox view.
- [06-maya-outlook-restored.png](../evidence/HSP-1011/06-maya-outlook-restored.png) — cropped Windows screen capture of the restored Outlook session, with Maya identified in the account menu and the message list visible.

## Troubleshooting

1. Confirmed Maya appeared in the admin centre with Microsoft 365 Business Basic and a mailbox storage panel.
2. Opened **Manage email apps** and recorded that Outlook on the web was checked before the staged change.
3. Unchecked only Outlook on the web and saved. The admin centre displayed `Mailbox email apps info updated` and then `OWA blocked`.
4. Maya completed an interactive sign-in. Outlook initially displayed her mailbox despite the saved block.
5. On a fresh application reload, Outlook displayed `Something went wrong`, `BootResult: fail`, `Error 440`, `StartupData`, and `ServerError`. The page's **Refresh the application** action returned to the same failure once.
6. Rechecked Maya's assigned Business Basic licence, checked Exchange Online (Plan 1) app, mailbox panel, and blocked Outlook on the web setting.
7. Restored Outlook on the web to checked and saved. The admin centre again displayed `Mailbox email apps info updated`.
8. Refreshed Outlook in Maya's authenticated browser session. Her mailbox and message list loaded.

## Root Cause

The staged mailbox client access change disabled Outlook on the web for Maya. The assigned Exchange Online licence and mailbox remained visible during the incident. The repeated fresh-load failure while the setting was blocked, followed by a successful Maya-context load after only that setting was restored, supports the client access restriction as the cause of this lab incident.

`Error 440` is recorded as the observed Outlook diagnostic. The UI did not establish that this code uniquely identifies a disabled Outlook on the web setting.

## Resolution

Re-enabled **Outlook on the web** for Maya in **Mail → Manage email apps**. No licence, mailbox, other email app control, or tenant-wide setting was changed for the remediation.

## Verification

The admin centre showed Outlook on the web checked after saving. In Maya's authenticated Outlook session, the application loaded a page titled `Mail - Maya Chen - Outlook`; the account control identified Maya, **New mail** was available, and her existing message list appeared.

This verifies web mailbox access. It does not claim a send-and-receive test or access through another client.

## User Communication

Simulated update — **not sent to a real user**:

> Your Exchange Online licence and mailbox were still present. Outlook on the web access had been disabled for your mailbox. I restored that specific setting and confirmed your mailbox opens again in your Outlook session.

## Escalation

Not escalated. The staged issue was isolated to one mailbox's Outlook on the web setting and was resolved through the available admin controls.

## Closure Notes

The failure was reproduced after a fresh load and one retry, investigated against the licence, service plan, mailbox and client access controls, then verified as Maya after the targeted correction. The initial successful post-change load is documented as observed behaviour, not omitted.

Ticket resolved within the demonstrated Outlook on the web scope.

## Related KB / SOP

[Outlook on the Web Access — Check Licence, Mailbox and Client Setting](../kb/outlook-web-client-access-investigation.md)

## Linked tickets / assets

[HSP-1003 — New Starter Onboarding](HSP-1003-new-starter-onboarding.md) documents Maya's earlier provisioning. No physical device asset was required for this browser-based lab case.
