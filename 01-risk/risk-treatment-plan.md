# Risk Treatment Plan

| | |
|---|---|
| **Risk owner** | Muhammad Khas |
| **Version** | 1.0 |
| **As of** | 2026-10-31 |
| **Standard** | ISO/IEC 27001:2022 clause 6.1.3 (information security risk treatment) |
| **Inputs** | [Risk register](risk-register.md), decisions D-013 to D-015 in [PROGRESS.md](../PROGRESS.md) |

ISO 27001 6.1.3 asks for four things, and this plan covers each:
1. A treatment option for every risk (c), shown in section 1 and 2.
2. The controls needed, checked against Annex A so nothing needed is missed (b, c). Annex A references are listed for each action. The full Statement of Applicability comes in Phase 2.
3. A plan for carrying out the treatment (e): actions, owners and due dates.
4. The risk owner's approval of the plan and acceptance of the residual risk (f), in section 4.

Each action becomes a tracked GitHub issue (label `phase-1`, plus `gap`) so progress is visible.

## 1. Risks to mitigate

Due dates are targets for a one-person, spare-time project. Actions on the live AWS account or the app repo are done in the private app repository and reported here without exploitable detail.

| Risk | Action | Annex A (2022) | Due | Current → target |
|---|---|---|---|---|
| R-01 | Move AWS root and admin MFA to phishing-resistant factors kept outside the password store; lock backups against deletion | A.5.17, A.8.2, A.8.5, A.8.13 | 2026-11-30 | Moderate → Low |
| R-02 | Phishing-resistant MFA on the identity account and code host; a separate code-host sign-in; email on every production deploy; monthly review of the code host security log | A.5.17, A.8.5, A.8.16, A.8.32 | 2026-11-30 | Moderate → Low |
| R-03 | Branch and pull request for every change, with a self-review checklist; CI on pull requests before merge | A.8.25, A.8.29, A.8.32 | 2026-11-30 | Moderate → Low |
| R-05 | Lockfile and `npm ci`; deploy credentials only for the deploy step; Actions pinned to commit SHAs; Dependabot alerts; later, narrower CDK deploy permissions | A.5.21, A.8.2, A.8.8 | 2026-11-30 (narrower permissions: 2027-01-31) | Moderate → Low |
| R-06 | Verified audit trail with log file validation; free alerts on root and admin sign-in and on logging changes; longer app log retention (Phase 4) | A.8.15, A.8.16 | 2027-01-31 | Moderate → Moderate (see section 3) |
| R-07 | Cognito Essentials plan with passkey sign-in; user guidance (Phase 6) | A.5.17, A.8.5 | 2027-01-31 | Low → Very Low |
| R-08 | Self-hosted passphrase strength check, no third-party scripts | A.5.17, A.8.24 | 2027-01-31 | Low → Low |
| R-09 | Record names that don't reveal dates | A.5.34, A.8.24 | 2027-03-31 | Moderate → Low |
| R-10 | Retention schedule and disclosure; decide on the 3-year backup copy (Phase 6) | A.5.34, A.8.10 | 2027-03-31 | Low → Low |
| R-11 | Privacy notice and terms, request log, applicable-law analysis (Phase 6) | A.5.31, A.5.34 | 2027-03-31 | Low → Low |
| R-12 | Restore test, recorded; backup copy vault brought into code or documented | A.5.30, A.8.13 | 2026-11-30 | Low → Low |
| R-13 | Continuity note for a trusted person; user backup download turned back on | A.5.29, A.5.30 | 2026-12-31 | Low → Low |
| R-15 | Verify domain transfer lock, auto-renew, contacts and renewal payment; add to Phase 4 checks | A.5.23, A.8.9 | 2026-11-30 | Low → Low |
| R-17 | Trusted Types in the Content Security Policy | A.8.26, A.8.28 | 2026-11-30 | Low → Low |
| R-18 | Cross-user tests for the two API routes not yet covered | A.8.3, A.8.29 | 2026-11-30 | Very Low → Very Low |
| R-19 | Test recovery of administrator access from a new device; offline break-glass codes | A.5.17, A.5.29 | 2026-11-30, together with R-01 | Low → Low |

**Order matters for R-01 and R-19.** New sign-in factors must be set up and recovery tested in the same session, or fixing R-01 could lock the owner out (R-19).

## 2. Risks accepted

| Risk | Level | Reason | Compensating or detective controls kept | Review |
|---|---|---|---|---|
| R-04 Operator ships a passphrase-capturing build | Low | Cannot reach zero while one person serves the code, even with a second person. Accepted at design time | Git history; a deploy changelog users can see (due 2026-12-31); triggers T-04 and T-06 | 2027-10-31 |
| R-14 Outage or cost spike | Low | Within the moderate appetite for outages; the app works offline | Throttling, CloudFront, AWS Shield Standard, budget alerts | 2027-10-31 |
| R-16 User mailbox exposes invitation password or recovery code | Low | The authenticator is still needed to reach any data | Temporary password expiry; TOTP; user guidance in Phase 6 | 2027-10-31 |

## 3. Residual risk after treatment

When the actions in section 1 are done, 16 risks are Low, 2 are Very Low and 1 remains Moderate.

**R-06 stays Moderate.** The owner chose free logging and alerting controls over a paid threat-detection service (GuardDuty), a cost decision under the risk appetite (D-011, D-013). Under the acceptance criteria (methodology 4.2), the remaining Moderate risk is accepted with this written reason, and reviewed every 6 months. If an incident happens, or the free controls prove not enough, GuardDuty is the first thing to reconsider.

## 4. Risk owner approval (ISO 27001 6.1.3 f)

I have reviewed this plan and the [risk register](risk-register.md). I approve the treatment actions and due dates above. I accept the residual risks described in sections 2 and 3, including R-06 at Moderate, until the review dates shown.

| Risk owner | Decision | Date |
|---|---|---|
| Muhammad Khas | **Approved** | 2026-10-31 |

The approval record is the merge of the pull request that adds this plan.

## 5. Tracking

Progress on each action is tracked in GitHub issues and reviewed at the quarterly review (Phase 9). When an action is complete, the risk is re-scored in the register with the evidence that the control now works.
