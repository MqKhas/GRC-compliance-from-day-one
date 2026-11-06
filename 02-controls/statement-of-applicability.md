# Statement of Applicability

| | |
|---|---|
| **Document owner** | Muhammad Khas |
| **Version** | 1.0 (approved by M. Khas, D-030) |
| **As of** | 2026-11-05, app version `tracker@4af3f74` |
| **Standard** | ISO/IEC 27001:2022, clause 6.1.3 d, Annex A |
| **Full statement** | [statement-of-applicability.csv](statement-of-applicability.csv), and the *Statement of Applicability* sheet in [control-framework.xlsx](control-framework.xlsx) |

For each of the 93 Annex A controls, the full statement says whether it applies, why, whether it's implemented, who provides it, which matrix controls and risks it ties to, and when open work is due.

## Status at a glance

| Status | Annex A controls | Meaning |
|---|---|---|
| Implemented | 29 | Every mapped matrix control is implemented |
| Partly implemented | 15 | Some mapped controls are implemented, others are open |
| Planned | 26 | Applies, with the work still open and a due date |
| Unverified | 1 | Believed to be in place, no evidence yet |
| Relied on, unverified | 14 | Provided by AWS. Phase 5 checks AWS's audit reports |
| Excluded | 8 | Doesn't apply, for the reasons below |
| **Total** | **93** | |

So 85 controls apply and 8 are excluded.

## The 8 exclusions

Each one depends on people or a network that don't exist here. If the trigger happens, the control comes back in.

- **5.11 Return of assets:** no staff or contractors hold assets.
- **6.1 Screening:** no one to screen.
- **6.2 Terms and conditions of employment:** no employment.
- **6.4 Disciplinary process:** no staff.
- **6.5 Responsibilities after termination or change of employment:** no employment to end.
- **6.6 Confidentiality or non-disclosure agreements:** no staff, contractors or partners receive confidential information.
- **8.23 Web filtering:** no organization network or staff browsing to filter. The owner's computer has malware protection (END-01).
- **8.30 Outsourced development:** the owner does all development.

The first seven come back if a second person gets administrator access (trigger T-06). Control 8.30 comes back if a developer or contractor is hired.

## Choices worth explaining

- **Data center controls aren't excluded.** Physical controls such as 7.1 and 7.2 still protect the data, but AWS provides them. Excluding them would suggest they don't matter. They're marked as provided by AWS and stay unverified until Phase 5 reviews AWS's audit reports.
- **A control can be in place without being fully credited.** "Partly implemented" means at least one mapped control has evidence and another is still open.
- **Risk isn't the only reason a control applies.** Some are here because of a legal duty or a commitment to users, such as 5.31 and 5.34.

## Review

Reviewed at each management review (Phase 9), when a compliance trigger fires, and when a matrix control changes status.
