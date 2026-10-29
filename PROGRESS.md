# Progress

## Phase checklist

| # | Phase | Started | Finished | Post | Release |
|---|---|---|---|---|---|
| 0 | Scope & system description | 2026-10-27 | | | |
| 1 | Risk assessment | | | | |
| 2 | Control framework & mapping | | | | |
| 3 | Policies | | | | |
| 4 | Evidence automation | | | | |
| 5 | Third-party risk | | | | |
| 6 | Privacy | | | | |
| 7 | Incident response | | | | |
| 8 | Internal audit / SOC 2 readiness | | | | |
| 9 | Continuous monitoring & maturity | | | | |

## Decisions log

Every scoping, risk and treatment decision, with the options I considered and why I chose.

| ID | Date | Decision | Options considered | Why | Decided by |
|---|---|---|---|---|---|
| D-001 | 2026-10-27 | Run the program in a separate public repo; the app repo stays private | Same repo as app; separate public repo | Keeps app source and live configuration private while the GRC work stays auditable in public | M. Khas |
| D-002 | 2026-10-27 | One phase per LinkedIn post, about every 2–3 weeks | One launch post; weekly posts | Ten phases match ten lessons; the pace allows real work between posts | M. Khas |
| D-003 | 2026-10-27 | Evidence from the private app repo is cited as a quoted excerpt plus commit hash | Link to private repo; screenshots only | Verifiable against a fixed version without exposing the source | M. Khas |
| D-004 | 2026-10-27 (revised 2026-10-29) | Demo **sample data** is out of scope. The demo **feature** is in scope as part of the production web app | Include demo fully; exclude demo fully; split data from feature | Sample data is fake, lives only in the browser tab, and is never sent or stored. But the demo runs on the production site from the same deployed code as real accounts, so a defect in it could affect the real app | M. Khas |
| D-005 | 2026-10-28 | Treat a future minor user as a **compliance trigger**: keep COPPA and teen-privacy duties documented and ready, and re-review scope before any account is made for someone under 18 | Ignore until it happens; design for minors now | No minors today, but likely later, and easy to forget. A written trigger with a checklist makes sure it gets reviewed at the right moment without adding work now | M. Khas |
| D-006 | 2026-10-28 | Assume other users record data about non-users ("Her wellbeing", Friends & Family names) and scope it as third-party personal data | Assume only I do; ask each user | Safer assumption. The data is encrypted and unreadable to the admin, so asking users isn't needed to scope it | M. Khas |
| D-007 | 2026-10-29 | Carve out the personal website hosted in the same AWS account. Keep everything it shares with the tracker in scope: the AWS account, root and admin identities, billing, the domain registration and DNS | Include it fully; carve it out with shared parts in scope | The personal site is static and holds no user data. The shared account and domain can still affect the tracker, so those stay in scope | M. Khas |
| D-008 | 2026-10-29 | Treat the Apple Account and Google account as in-scope identity providers for administration | Leave personal accounts out of scope | Both protect administrative access to AWS or GitHub, and GitHub deploys production. Compromise of either could reach production. Details in private notes | Proposed, awaiting M. Khas |

## Session notes

- 2026-10-27: Repo set up. Phase 0 started.
