# Risk Assessment Methodology

| | |
|---|---|
| **Document owner** | Muhammad Khas |
| **Version** | 1.0 |
| **As of** | 2026-10-31 |
| **Based on** | NIST SP 800-30 Rev 1 (*Guide for Conducting Risk Assessments*) |
| **Also meets** | ISO/IEC 27001:2022 clause 6.1.2 (risk assessment) and 6.1.3 (risk treatment) |
| **Decisions** | D-010, D-011, D-012 in [PROGRESS.md](../PROGRESS.md) |

## 1. Purpose and scope

This document defines how risks to Khas Tracker are identified, scored, evaluated and treated, so that repeating the assessment gives consistent, comparable results (ISO 27001 6.1.2 b).

The scope is the ISMS scope in [isms-scope.md](../00-scope/isms-scope.md). The assessment is done at the information system level (SP 800-30 Tier 3), because the organization, the business process and the system are effectively the same thing here.

## 2. Process

Following SP 800-30 Chapter 3, Step 2:

| Step | SP 800-30 task | Output |
|---|---|---|
| 1. Identify assets | (preparation, 3.1) | [Asset register](asset-threat-register.md#assets) |
| 2. Identify threat sources | Task 2-1, Appendix D | [Threat sources](asset-threat-register.md#threat-sources) |
| 3. Identify threat events | Task 2-2, Appendix E | Risk register: threat event, with MITRE ATT&CK technique IDs where an adversary is involved |
| 4. Identify vulnerabilities and predisposing conditions | Task 2-3, Appendix F | [Vulnerabilities](asset-threat-register.md#vulnerabilities-and-predisposing-conditions) |
| 5. Determine likelihood | Task 2-4, Appendix G | Section 3.1 |
| 6. Determine impact | Task 2-5, Appendix H | Section 3.2 |
| 7. Determine risk | Task 2-6, Appendix I | Section 3.3 |
| 8. Evaluate and treat | ISO 27001 6.1.2 e, 6.1.3 | Section 4, [risk treatment plan](risk-treatment-plan.md) |

Each risk is scored three times:

| Score | Meaning |
|---|---|
| **Inherent** | The risk without any of Khas Tracker's own controls (no end-to-end encryption, MFA, backups, CI checks and so on). Controls provided by AWS and other vendors still count, because they are not this system's choice |
| **Current** | The risk with the controls that exist today. **Only verified controls count.** A control marked *(unverified)* in the system description does not lower the score until Phase 4 shows it is in place |
| **Target** | The risk expected after the planned treatment in the risk treatment plan is complete |

## 3. Scales

Five levels, as in SP 800-30 Appendices G–I. The numbers 1–5 are labels used for sorting, not measurements.

### 3.1 Likelihood (SP 800-30 Appendix G, adapted)

One question for every risk, whether an attacker or an accident is involved: **how likely is this event to happen and cause the stated harm in the next 12 months?** This combines SP 800-30's likelihood of initiation or occurrence with the likelihood of adverse impact (Table G-5) into one judgment.

| Level | Value | Definition |
|---|---|---|
| Very High | 5 | Almost certain. Expected to happen more than once a year |
| High | 4 | Likely. Expected about once a year |
| Moderate | 3 | Could happen. About once every 1–3 years |
| Low | 2 | Unlikely. About once every 3–10 years |
| Very Low | 1 | Highly unlikely. Less than once in 10 years |

For adversarial threats, consider how capable and motivated the threat source is and how easy the vulnerability is to use (SP 800-30 Tables D-3 to D-5 and F-2). Write the reason in the register.

### 3.2 Impact (SP 800-30 Appendix H, adapted)

Rate the **worst** effect across three kinds of harm. SP 800-30 Table H-2 groups harm to operations, assets and individuals. Here "people" covers individuals, and "service" and "operator" cover operations and assets.

| Level | Value | People (users and the people they record) | Service | Operator |
|---|---|---|---|---|
| Very High | 5 | Plaintext content of many users readable, or anyone's data readable from now on | All user data permanently lost, or an attacker controls production | Over $5,000; regulator action; service shut down |
| High | 4 | Plaintext content of one user, or of a person a user records, exposed | One user's data lost for good; outage longer than a week | $500–$5,000; formal complaint; users' trust broken |
| Moderate | 3 | An account taken over without content read; or email or metadata of several users exposed | Outage of 1–7 days; data restored from backup with some loss | $100–$500; users upset, some leave |
| Low | 2 | One user's email or activity dates seen | Outage under a day; full restore | $10–$100; minor embarrassment |
| Very Low | 1 | No personal data involved | No noticeable effect | Under $10; nobody notices |

### 3.3 Risk level (SP 800-30 Appendix I, Table I-2)

The risk level is read from the SP 800-30 matrix. Table I-2 deliberately weights impact, so a very high impact stays at least Low even when it is very unlikely.

| Likelihood ↓ / Impact → | Very Low | Low | Moderate | High | Very High |
|---|---|---|---|---|---|
| **Very High** | Very Low | Low | Moderate | High | Very High |
| **High** | Very Low | Low | Moderate | High | Very High |
| **Moderate** | Very Low | Low | Moderate | Moderate | High |
| **Low** | Very Low | Low | Low | Low | Moderate |
| **Very Low** | Very Low | Very Low | Very Low | Low | Low |

A **score** (likelihood × impact, 1–25) is shown alongside, only to sort risks within the same level. The level, not the score, drives decisions.

## 4. Risk appetite and acceptance criteria

### 4.1 Risk appetite statement

> Khas Tracker exists to give a small group of people a private place to track their lives. The operator has a **very low appetite** for any risk that could expose what users record, or that could let anyone other than the operator change the code users run. There is a **low appetite** for permanently losing user data. There is a **moderate appetite** for outages, because the app works offline and is a free service for friends and family, so a day of downtime is acceptable. Free or low-cost controls are preferred. Money is spent only when a High or Very High risk needs it.

### 4.2 Acceptance criteria (ISO 27001 6.1.2 a)

| Current risk level | Rule | Who decides | Review |
|---|---|---|---|
| Very Low, Low | May be accepted by default | Risk owner | Yearly |
| Moderate | May be accepted only with a written reason; otherwise treated | Risk owner | Every 6 months |
| High, Very High | Must be treated. Accepting one needs a signed exception with a reason, compensating controls and a review date | Risk owner, recorded as a decision in PROGRESS.md | Every 90 days while accepted |

A risk that touches **confidentiality of user content** or **code integrity** is judged against the very low appetite in 4.1: if a free or cheap treatment exists, it is treated even when the level is Low.

### 4.3 Treatment options (ISO 27001 6.1.3, SP 800-39 risk response)

| Option | ISO 27005 term | Meaning |
|---|---|---|
| Mitigate | Modify | Add or improve controls to lower likelihood or impact |
| Accept | Retain | Keep the risk on purpose, under the criteria in 4.2 |
| Transfer | Share | Move part of the risk to another party (insurance, a vendor) |
| Avoid | Avoid | Stop the activity that creates the risk |

## 5. Roles

| Role | Who | Responsibility |
|---|---|---|
| Risk owner | Muhammad Khas, for every risk | Decides scores, treatment and acceptance; signs the treatment plan |
| Risk assessor | Muhammad Khas | Runs the assessment under this method |

With one person in every role, there is no independent challenge of the scores. This is part of the separation-of-duties limitation in the system description, section 7. The internal audit in Phase 8 is the planned independent check.

## 6. When the assessment is repeated (ISO 27001 8.2)

- At least once a year.
- When a [compliance trigger](../00-scope/compliance-triggers.md) fires.
- After a security incident, or a tabletop exercise that finds something new (Phase 7).
- Before a significant change to the architecture, such as a new AWS service or a feature that touches encryption.

## 7. Outputs

| File | Contents |
|---|---|
| [asset-threat-register.md](asset-threat-register.md) | Assets, threat sources, vulnerabilities and predisposing conditions |
| [risk-register.md](risk-register.md) and [risk-register.csv](risk-register.csv) | Every risk with inherent, current and target scores, controls, owner and treatment |
| [risk-treatment-plan.md](risk-treatment-plan.md) | Treatment actions, due dates and the risk owner's sign-off (ISO 27001 6.1.3) |
