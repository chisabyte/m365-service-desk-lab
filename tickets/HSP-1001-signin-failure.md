# HSP-1001 — Microsoft 365 Sign-In Failure

> **Lab case**  
> Fictional Harbour Street Partners environment. Synthetic users. Staged personal-lab incident, not a customer incident or production employment.

## 30-Second Summary

**Problem.** Priya Nair's staged report was that Microsoft 365 sign-in failed; she suspected the password.

**Finding.** The account showed **Sign-in blocked**. The relevant Microsoft Entra failure detail recorded `50057`: “The user account is disabled.”

**Action.** The sign-in block was removed. The password was not reset as the incident resolution.

**Result.** A later Priya **OfficeHome** sign-in row shows **Success** and error `0`.

**Evidence.** The symptom, account state, failure detail and later log row are shown below.

## Primary Evidence

### User-visible symptom

[![Priya's Microsoft sign-in window saying the account has been locked and support should be contacted.](../evidence/HSP-1001/02-signin-error.png)](../evidence/HSP-1001/02-signin-error.png)

The sign-in window says the account has been locked. That wording alone does not identify the Entra failure reason.

### Account state found

[![Microsoft 365 user panel for Priya showing Sign-in blocked and an Unblock sign-in action.](../evidence/HSP-1001/03-account-status-blocked.png)](../evidence/HSP-1001/03-account-status-blocked.png)

Priya's account panel shows **Sign-in blocked**. The visible **Unblock sign-in** control is a possible action, not proof of a click.

### Diagnostic record

[![Microsoft Entra sign-in detail for Priya showing failure code 50057 and The user account is disabled.](../evidence/HSP-1001/05-failed-signin-details.png)](../evidence/HSP-1001/05-failed-signin-details.png)

The failed event records `50057` and “The user account is disabled.” This changed the diagnosis from a suspected password issue to account access state.

### Verification in the later log

[![Microsoft Entra sign-in list with the top Priya OfficeHome row showing Success and error 0 after earlier failed rows.](../evidence/HSP-1001/07-signin-restored-log.png)](../evidence/HSP-1001/07-signin-restored-log.png)

The **top OfficeHome row at 1:22:48 PM on 22 September 2026** shows **Success / 0**. Earlier rows in the same image include failures and interruptions; they are not the verification event.

<details>
<summary>Additional staging and investigation evidence</summary>

### Staged block

[![Microsoft 365 admin confirmation saying Priya is now blocked from signing in.](../evidence/HSP-1001/01-signin-blocked.png)](../evidence/HSP-1001/01-signin-blocked.png)

This is the staged fault, not the repair.

### Wider sign-in timeline

[![Microsoft Entra sign-in list for Priya with failures, interruptions and earlier OfficeHome success rows.](../evidence/HSP-1001/04-signin-logs-overview.png)](../evidence/HSP-1001/04-signin-logs-overview.png)

The broader view mixes events. The failure detail above is decisive for diagnosis; the later OfficeHome row above is the clearest post-change verification.

</details>

## User

Priya Nair — synthetic Finance user.

## Category

Incident — identity and Microsoft 365 sign-in.

## User Report

Staged lab report: Priya could not sign in and thought the password might be wrong. No live customer report was received.

## Business Impact

The synthetic user could not access Microsoft 365 through the attempted sign-in. No wider outage is evidenced.

## Priority

**P3, lab triage judgement:** one affected user, with no evidence of a broader service failure.

## Questions Asked

No live interview occurred. The lab scenario supplied the attempted sign-in and suspected password issue; no further answers are recorded.

## Initial Hypothesis

Incorrect password was plausible from the staged report. The analyst checked account state and sign-in information before considering a reset.

## Evidence Collected

The user-visible error, blocked account panel, Entra `50057` failure detail, and subsequent OfficeHome **Success / 0** event. Staging and wider timeline images are in the collapsed section.

## Troubleshooting

The sign-in window showed a lock message. Microsoft 365 administration showed **Sign-in blocked**. The Entra event identified a disabled account rather than a bad-password code.

## Root Cause

A sign-in block staged on Priya's synthetic account.

## Resolution

The lab incident record states that the sign-in block was removed. No screenshot captures the unblock click itself; the later successful sign-in is the outcome evidence. No password reset was used as the resolution.

## Verification

The later, top OfficeHome row in `07-signin-restored-log.png` records **Success** with error `0`. This supports restored sign-in for that event, not a claim about every Microsoft 365 workload.

## User Communication

**Simulated lab communication — not sent to a real user:** “Your account sign-in was blocked. The block has been removed and a later Microsoft 365 sign-in succeeded. Your password was not reset for this incident.”

## Escalation

Not escalated in the lab record; the account-state fault was identified and a successful later sign-in was observed.

## Closure Notes

Staged account block identified through `50057`; access restored and checked against the later OfficeHome success row.

## Related KB / SOP

[Ticket and evidence standard](../docs/ticket-standard.md). No case-specific KB article is claimed.

## Linked Tickets / Assets

[HSP-1002 — MFA Recovery](HSP-1002-mfa-recovery.md) uses the same synthetic user. No asset is linked.
