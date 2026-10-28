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
| D-004 | 2026-10-27 | Demo mode sample data is out of scope | Include demo; exclude demo | Sample data is fake, lives only in the browser tab, and is never sent or stored. Demo exists only to show features before someone gets an account. Open question: whether the demo *code path* stays in scope (see Phase 0 batch 2) | M. Khas |

## Session notes

- 2026-10-27: Repo set up. Phase 0 started.
