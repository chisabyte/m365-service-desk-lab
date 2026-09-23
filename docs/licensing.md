# Licensing and Feature Limits

## Available / Used

- **Microsoft 365 Business Basic:** shown assigned to Maya in HSP-1003 and removed from Owen in HSP-1004.
- **Microsoft 365 admin centre and Microsoft Entra ID:** used for account status, groups, authentication methods and sign-in records shown in HSP-1001 to HSP-1004.
- **Exchange Online, Outlook, Teams, SharePoint and OneDrive:** used in the onboarding and offboarding cases. The screenshots establish the particular checks described in those tickets.
- **Exchange Online mail-flow rules and message trace; Microsoft 365 Service Health:** used in HSP-1005 to investigate, remediate and verify a staged two-user delivery failure. The visible advisories were not treated as proof of a Microsoft delivery outage.
- **Microsoft Authenticator / MFA:** a method was present, removed and registered again in HSP-1002. The later successful sign-in record says the MFA requirement was satisfied by a token claim; it does not independently show a fresh app approval.

## Unavailable / Not Tested

- **Microsoft Intune endpoint management:** not available or configured in this Business Basic lab. No retire, remote wipe or device ownership transfer was performed for HSP-1004. See the [device offboarding note](../evidence/HSP-1004/12-device-offboarding-note.txt).
- **Conditional Access policy administration:** no policy configuration or change is evidenced. Sign-in log rows display `Not Applied`; this is not a claim of a configured Conditional Access control.
- **Session revocation, device wipe and address-list hiding for HSP-1004:** no evidence of completion is included, so these are not recorded as performed.
- **Self-service password reset and other advanced identity features:** outside the completed case evidence; no completion claim is made.

Availability here describes the demonstrated lab scope, not a general statement of Microsoft licensing entitlements.
