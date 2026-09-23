# Lab SOP — Identity Verification Before MFA Recovery

This is a reusable procedure for the fictional Harbour Street Partners lab, derived from the [HSP-1002 identity-verification note](../evidence/HSP-1002/03-identity-verification-note.txt). It is not a real organisation's policy or proof that any external approval occurred.

1. Match the synthetic requester to the intended Microsoft 365 account before changing an authentication method.
2. Obtain and record approval from the fictional manager in the lab scenario before the recovery action. HSP-1002 names Alex Romero, Office Manager, as the approver; the note does not specify an approval channel or independent audit record.
3. Record completion of the identity check before removing the unavailable method.
4. Remove only the affected authentication method, allow registration of the replacement method, and inspect the resulting security-info state.
5. Verify a subsequent sign-in record and state exactly what it proves. If the record says MFA was satisfied by a token claim, do not describe it as a captured fresh Authenticator approval.

The administrator's access to the tenant is not a substitute for the identity check.
