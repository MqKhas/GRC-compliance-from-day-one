# SP 800-53 Access Control Tailoring

| | |
|---|---|
| **Document owner** | Muhammad Khas |
| **Version** | 1.0 (approved by M. Khas, D-031) |
| **As of** | 2026-11-05, app version `tracker@4af3f74` |
| **Starting point** | SP 800-53 Rev 5 Access Control (AC) family, SP 800-53B moderate baseline: 39 controls and enhancements |
| **Full table** | [ac-tailoring.csv](ac-tailoring.csv), and the *AC tailoring* sheet in [control-framework.xlsx](control-framework.xlsx) |

No federal rule requires this system to follow a baseline. The AC family is tailored in full to show how tailoring works, and because access control carries most of the risk here (D-017). Every change from the baseline has a written reason.

## What changed

| Tailoring action | Controls | What it means |
|---|---|---|
| Keep | 22 | Applies as written |
| Keep, set parameter | 6 | Applies, with the blanks filled in for this system |
| Compensate | 3 | The control doesn't fit, so something else gives similar protection |
| Scope out | 8 | Nothing here for the control to act on |
| Add | 1 | Not in the baseline, added because this system needs it |
| **Total** | **40** | 39 from the baseline plus 1 added |

## Where it stands

| Status | Controls |
|---|---|
| Implemented | 15 |
| Partly implemented | 5 |
| Unverified | 5 |
| Planned, with a due date | 7 |
| Not applicable (scoped out) | 8 |

## Compensated

- **AC-5 Separation of duties:** one person can't separate duties. A pull request with a self-review checklist, automated checks, a deploy notice and a changelog users can see make changes visible instead.
- **AC-2(5) Inactivity logout and AC-11 Device lock:** the app has no idle lock by design. It relies on the device's own lock, plus a Lock now button for users. Administrator devices lock on idle (END-01).

## Scoped out

- **AC-2(2) Temporary and emergency accounts:** none exist.
- **AC-8 System use notification:** this banner is mainly a government system's monitoring warning. This private service has no legal need for one, and privacy terms come in Phase 6.
- **AC-18, AC-18(1), AC-18(3) Wireless access:** the service runs no wireless network.
- **AC-20(1), AC-20(2) Limits on external systems and portable storage:** both are written for staff, and there are none (trigger T-06).
- **AC-21 Information sharing:** users can't share records with each other. This comes back if a sharing feature is added.

## Parameter values set for this system

- **AC-2(3) Disable accounts:** accounts with no sync for 180 days are flagged for the owner at the quarterly review. Nothing is disabled or deleted automatically, and the owner checks with the person first.
- **AC-2(13) High-risk individuals:** the account is disabled within 24 hours of learning of the risk.
- **AC-6(7) Privilege review:** every quarter (D-022).
- **AC-7 Failed sign-ins:** the sign-in service's built-in behavior. After 5 failures it locks for 1 second, doubling with each further failure up to about 15 minutes, for passwords and MFA codes. It's temporary on purpose, so nobody can lock a user out for good by guessing. Documented by AWS, and confirmed in Phase 5.
- **AC-12 Session termination:** tokens last 1 hour and refresh tokens 30 days.
- **AC-1 Policy review:** every year and after a significant change.

## Added

- **AC-12(1) User-initiated logouts:** users can sign out or lock the app from Settings. It isn't in the moderate baseline. It's added because, without an idle lock, a user's own lock and sign-out matter more.

## Limits

Users' own phones aren't managed by this system. Encryption on the device before upload is what protects data once it leaves them, and the phone's own lock protects it on the phone.

**Accepted risk (D-031):** a phone that's unlocked with the app open shows everything, because the app has no idle lock. Keeping a phone locked is the user's responsibility, and the owner accepts the remaining risk. User guidance (PRV-01, Phase 6) will say so. This is reviewed if paid access starts (T-05).
