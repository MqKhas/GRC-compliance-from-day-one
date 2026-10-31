# Compliance from Day One

**Building an auditor-grade GRC program for a real, running app.**

I built **Khas Tracker**, a private, end-to-end encrypted daily-life and fitness tracker on AWS that a few friends and family use. Then I got curious about how real companies *prove* their security, so this is me learning Governance, Risk & Compliance by running a full GRC program on it, in public. It's set up as if the app were a startup preparing for its first SOC 2 Type I, ISO 27001 certification, and customer security reviews. One set of controls is mapped to SOC 2, ISO 27001:2022, NIST CSF 2.0, NIST SP 800-53 and SP 800-171, and every framework considered is documented as adopted, used as a reference, or not applicable.

Try the app's demo (fake data, stays in your browser): https://khas.tracker.muhammad-khas.com

## Ground rules

- **No invented evidence.** If a control isn't implemented, it's a gap with a remediation item, not a pass.
- **Decisions are mine and recorded.** Risk scores, risk acceptance and scoping are logged in [PROGRESS.md](PROGRESS.md).
- **AI-assisted, human-owned.** I use AI tools to help draft and build. I make every decision and can defend every artifact.
- **Safe to be public.** No secrets, account identifiers or exploitable details of open weaknesses. The app's source repo is private, so evidence from it is quoted with a commit reference.

## Phases

| # | Phase | Status | Release |
|---|---|---|---|
| 0 | [Scope & system description](00-scope/) | Done | [v0.1-scope](https://github.com/MqKhas/GRC-compliance-from-day-one/releases/tag/v0.1-scope) |
| 1 | [Risk assessment](01-risk/) | Not started | |
| 2 | [Control framework & mapping](02-controls/) | Not started | |
| 3 | [Policies](03-policies/) | Not started | |
| 4 | [Evidence automation](04-evidence-automation/) | Not started | |
| 5 | [Third-party risk](05-third-party-risk/) | Not started | |
| 6 | [Privacy](06-privacy/) | Not started | |
| 7 | [Incident response](07-incident-response/) | Not started | |
| 8 | [Internal audit / SOC 2 readiness](08-audit/) | Not started | |
| 9 | [Continuous monitoring & maturity](09-continuous-monitoring/) | Not started | |

## How this repo works as a GRC system

| GRC concept | Where it lives |
|---|---|
| Remediation items (POA&M) and audit findings | GitHub Issues, labeled `gap` / `finding` / `risk-accepted` |
| Phases | Milestones, each closed with a tagged release |
| Policy approval and change history | Pull requests: the merge is the approval record |
| Decisions | The decisions log in [PROGRESS.md](PROGRESS.md) |

## Key results

- **Phase 0:** 29 data elements inventoried and classified, including 9 that are health, mental health, religious or about people without accounts. 10 compliance triggers documented. 9 scoping and strategy decisions logged.
