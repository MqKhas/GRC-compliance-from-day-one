# Primer: Phase 1, Risk assessment

A 5-minute read before building the risk methodology and registers in [`01-risk/`](../01-risk/).

## What this phase is and why it exists

Phase 0 answered "what are we protecting?" Phase 1 answers "what could go wrong, how bad would it be, and what are we going to do about it?"

Every later phase depends on it. Controls (Phase 2) exist to treat risks. Policies (Phase 3) say how those controls are run. Audit (Phase 8) checks whether the risks are actually being managed. A control without a risk behind it is just busywork, and an auditor will ask "why do you have this?"

**Who reads the output:**
- **An auditor** checks that the method is defined, applied the same way every time, and that every decision has an owner and a date. ISO 27001 auditors look for exactly that.
- **A customer** in a security review wants to see that you know your top risks and are doing something about them.
- **An exec** (here, the owner) reads the top 5 to decide where time and money go.
- **An engineer** (also the owner) reads the treatment plan to know what to fix next.

## The source standard: NIST SP 800-30 Rev 1

*Guide for Conducting Risk Assessments* (2012). The parts we use:

| Part | What it gives us |
|---|---|
| Ch. 2, Figure 3 (risk model) | **Threat source** → starts a **threat event** → that exploits a **vulnerability** (made easier by **predisposing conditions**) → causing an **adverse impact**. Existing controls sit in between |
| Ch. 3, Step 2 (Tasks 2-1 to 2-6) | The order of work: identify threat sources, threat events, vulnerabilities, then decide likelihood, impact and risk |
| Appendix D | Threat source types: adversarial (an attacker, an insider), accidental (the admin's own mistake), structural (hardware or software failure), environmental (an AWS region outage) |
| Appendix E | Example threat events (Table E-2 adversarial, E-3 non-adversarial) |
| Appendix F | Vulnerabilities and predisposing conditions |
| **Appendix G** | **Likelihood.** Tables G-2 to G-5: how likely an attacker is to try, how likely a non-attack event is to happen, and how likely it is to cause harm if it does |
| **Appendix H** | **Impact.** Table H-2 lists types of harm (to operations, assets, individuals); Table H-3 is the impact scale |
| **Appendix I** | **Risk.** Table I-2 combines likelihood and impact into a risk level |
| Appendix K | What a risk assessment report contains |

800-30 uses five levels everywhere: **Very Low, Low, Moderate, High, Very High**. It deliberately leaves the definitions to you, so the first job is to write them for *this* system.

**In SOC terms:** a threat source is the actor, a threat event is what they do (that's where MITRE ATT&CK technique IDs fit), a vulnerability is the weakness that lets it work, and impact is the damage you'd write up in the escalation report.

## How ISO 27001 fits in

ISO 27001:2022 doesn't say *how* to assess risk. It says what your method must do.

- **Clause 6.1.2 (risk assessment):** define risk criteria, including **risk acceptance criteria**. The method must give "consistent, valid and comparable results" if repeated. Identify risks to confidentiality, integrity and availability, give each a **risk owner**, analyse likelihood and consequence, and rank them.
- **Clause 6.1.3 (risk treatment):** choose a treatment for each risk, pick the controls, check them against Annex A so nothing needed was missed (that becomes the Statement of Applicability in Phase 2), write a **risk treatment plan**, and get the **risk owner's approval** of the plan and of the residual risk.
- **Clause 8.2 / 8.3:** actually do it at planned intervals and carry out the plan.

So the 800-30 method is the engine, and ISO is the checklist the engine has to satisfy. One method serves both, the same idea as D-009.

## Inherent, current and target risk

| Term | Meaning | For this app |
|---|---|---|
| **Inherent risk** | The risk if none of the specific controls existed | "Someone reads users' health data", if data were stored in plaintext |
| **Current (residual) risk** | The risk now, with the controls that really exist today | The same risk with end-to-end encryption in place: much lower |
| **Target risk** | The risk expected after the planned treatment is done | What's left after the POA&M items are closed |

A common auditor pushback: "inherent" gets fuzzy because nobody can picture a system with *no* controls. Define it once in the methodology and stick to it. Another pushback: claiming a control lowers residual risk when it's **unverified** (CloudTrail, for example). Unverified controls don't count until Phase 4 proves them.

## Risk appetite, tolerance and acceptance

- **Risk appetite:** a plain statement of how much risk you're willing to take to get what you want. "I'll accept some availability risk to keep the service almost free, but very little confidentiality risk to users' health data."
- **Risk tolerance / acceptance criteria:** the measurable version. "Low risks can be accepted by default. High risks must be treated, or accepted in writing by the owner with a review date."
- **Risk acceptance** is a decision, not a shrug. It needs an owner, a reason, a date and a review date.

## The four treatment options

| Option | Meaning | Example here |
|---|---|---|
| **Mitigate** (ISO: modify) | Add or improve controls | Turn on GuardDuty |
| **Accept** (ISO: retain) | Live with it, on purpose, in writing | Admin could ship a passphrase-capturing build |
| **Transfer** (ISO: share) | Move part of it to someone else | Insurance, or relying on a vendor. With no budget, rarely an option here |
| **Avoid** | Stop doing the risky thing | Drop the 3-year backup copy instead of protecting it |

## Good vs. bad

**Good:** scales written for this system; every risk has a threat, a vulnerability, a reason for the likelihood and impact, an owner and a decision; unverified controls don't reduce risk; the top risks are obvious at a glance.

**Bad:** a register of 80 generic risks ("malware", "hackers") copied from a template, every score "Medium", no reasoning, and controls claimed but never shown.
