# Common Control Matrix

| | |
|---|---|
| **Document owner** | Muhammad Khas |
| **Version** | 1.0 (approved by M. Khas, D-028) |
| **As of** | 2026-11-05, app version `tracker@4af3f74` |
| **Full matrix** | [control-matrix.csv](control-matrix.csv) (source of truth) and [control-framework.xlsx](control-framework.xlsx) (same data, with summary, risk trace and coverage sheets) |

One set of controls for Khas Tracker, each mapped to SOC 2, ISO 27001:2022 Annex A, NIST CSF 2.0, NIST SP 800-53 Rev 5 and NIST SP 800-171 Rev 3, and traced to the risks it treats. Decisions D-009 and D-017 to D-029 in [PROGRESS.md](../PROGRESS.md).

## Status at a glance

| Status | Controls | Meaning (D-018) |
|---|---|---|
| Implemented | 31 | Evidence exists: an excerpt from the app repo at a fixed commit, or a document in this repo |
| Unverified | 3 | Believed to be in place, no evidence yet. Not credited until evidence exists, by Phase 5 at the latest |
| Gap | 32 | Planned, with a due date from the risk treatment plan or the phase plan (D-021) |
| Not applicable | 1 | Personnel security: there are no staff (trigger T-06) |
| **Total** | **67** | |

Most gaps are due soon: 9 by 2026-11-30, 6 by 2026-12-31 and 8 by 2027-01-31. The rest are documents produced by later phases (privacy, vendors, incident response, audit), at about one phase a month.

SOC 2 categories in scope: Security, Availability, Confidentiality and Privacy (D-019, D-025).

## Where the controls are strong and where they're thin

The controls are grouped into 15 domains, shown by the prefix of the control ID. Implemented count / total in each domain:

- **Fully in place:** Secure development (SDL) 6 / 6, Risk management (RSK) 2 / 2, Availability (AVL) 1 / 1, Endpoints (END) 1 / 1.
- **Mostly in place:** Change management (CHG) 4 / 6, Identity and access (IAM) 6 / 13 (plus 2 unverified).
- **Mostly gaps, closed by later phases:** Governance (GOV) 3 / 8, Data protection (DAT) 3 / 6, Vulnerability and supply chain (VUL) 2 / 4, Logging and monitoring (LOG) 1 / 5, Backup and continuity (BCK) 1 / 5, Privacy (PRV) 1 / 4.
- **Nothing in place yet:** Vendors (VND) 0 / 2, Incident response (IR) 0 / 2, People (HR) 0 / 2, of which 1 is not applicable.

The *Summary* sheet in the workbook has the same breakdown with every status.

## What stands out

- **The app is stronger than the operations around it.** Encryption, access enforcement, the Content Security Policy and the pipeline are implemented with evidence in code. Logging, backup protection, incident response and written policies are mostly gaps. That's normal for a project built by an engineer before any GRC program existed.
- **End-to-end encryption is the main compensating control** (DAT-02). Because stored data is ciphertext made on the user's device, customer-managed encryption keys on the server would add little, so AWS-managed keys are used (DAT-04). Encryption doesn't protect against a changed app, so the code-integrity controls (CHG-01, CHG-05, CHG-06, IAM-07) carry that weight.
- **Separation of duties is compensated, not met** (GOV-03). With one operator, a pull request with a self-review checklist, CI, a deploy notice and a deploy changelog users can see take its place.
- **Some controls are honest about their limits.** CI and the pull-request process aren't enforced by branch protection (a paid feature for private repositories). Internal audit can't be independent. There is no staging environment. Each limit is written in the control's notes.
- **Change management works, and the paperwork caught up.** A new Parenting section shipped on 2026-11-04 through the pull-request process (CHG-01), with its own security review in the app repo. The data inventory was updated the next day to cover it (DAT-01, D-029), and risks R-09 and R-11 were re-checked: their wording widened, their scores didn't change.
- **Every risk traces to at least one control.** The *Risk trace* sheet shows which controls treat each risk and how many of them are still gaps.

## How the mappings were made

- **NIST CSF 2.0, SP 800-53 and SP 800-171:** based on NIST's published relationships (the CSF 2.0 informative references, and 800-171 Rev 3's derivation from the 800-53 moderate baseline).
- **SOC 2 and ISO 27001:** mapped by judgment against the criteria and control text.

A mapping means the control helps meet that requirement, not that it meets it alone. The *SOC 2 coverage* and *ISO coverage* sheets show the mapping from the other direction, so an auditor can start from a criterion and find the controls.
