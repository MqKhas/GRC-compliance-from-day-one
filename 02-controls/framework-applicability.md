# Framework Applicability

| | |
|---|---|
| **Document owner** | Muhammad Khas |
| **Version** | 1.1 (D-019 approved; revised by D-025, D-026 and D-027) |
| **As of** | 2026-11-05 |
| **Inputs** | [System description](../00-scope/system-description.md), [compliance triggers](../00-scope/compliance-triggers.md), decision D-009 in [PROGRESS.md](../PROGRESS.md) |

Every framework and law considered for Khas Tracker, whether it applies, why, and what would change the answer. Facts used:
- 10 invited users, all friends and family, living in four US states. None of those states has a consumer health data law.
- No users under 18 and none in the EU or UK.
- No payments, ads or revenue today. Paid access is being considered (trigger T-05).
- Hosted in AWS `us-east-1` (Virginia), with backup copies in `us-west-2` (Oregon).
- User content is end-to-end encrypted.

The legal rows are my own analysis for planning controls, not legal advice. Phase 6 does the full privacy-law analysis.

## Summary

| Framework or law | Status | Trigger that changes it |
|---|---|---|
| SOC 2 (Security, Availability, Confidentiality, Privacy) | **Applies: primary target** (Type I readiness) | Processing integrity would be added if the service starts processing transactions or results for users |
| ISO/IEC 27001:2022 | **Applies: second target** (ISMS readiness) | None; already applied |
| NIST CSF 2.0 | **Reference: mapping and maturity** | None |
| NIST SP 800-53 Rev 5 | **Reference: mapping**, AC family tailored from the moderate baseline (D-017) | Federal customer (T-09) |
| NIST SP 800-171 Rev 3 | Does not apply. Mapping column kept | CUI or a defense contract (T-09) |
| CMMC 2.0 | Does not apply | A defense contract handling FCI or CUI (T-09) |
| FedRAMP | Does not apply | A US federal agency as a customer (T-09) |
| HIPAA | Does not apply | Offered to or for a healthcare provider or health plan (T-10) |
| FTC Health Breach Notification Rule | Likely does not apply today. **Treated as applying once access is paid** | Paid access (T-05) |
| FTC Act section 5 (unfair or deceptive practices) | Reference today. Applies to a commercial service | Paid access (T-05) |
| GDPR / UK GDPR | Does not apply today. **Readiness prepared in Phase 6** (D-026) | EU or UK user, or processing there (T-08) |
| COPPA | Does not apply | Account for someone under 13 (T-01) |
| Comprehensive state privacy laws (users' states) | Does not apply | Growth past a state's threshold, revenue from selling data, or no longer a small business (T-04, T-05) |
| Consumer health data laws (WA MHMDA, NV, CT) | Does not apply | A user in, or data processed in, one of those states (T-03) |
| State breach notification laws (users' states) | Reference: Phase 7 decision tree | An unencrypted exposure of covered personal information |

## Reasons

Four groups, in the same order as the table: **Applies**, **Reference only**, **Does not apply today, prepared for**, and **Does not apply**.

### Applies

**SOC 2 (AICPA Trust Services Criteria).** It's voluntary, but it's the report a US customer would ask for first. It's the primary target under D-009. Categories in scope:
- **Security** (CC1–CC9): required in every SOC 2 report.
- **Availability** (A1.1–A1.3): added by D-025, because paying users expect the service to be there and to come back after a failure. It covers capacity, backup and recovery infrastructure, and recovery testing. The app keeps working offline (SC-4), which helps, but sync and new-device setup need the service.
- **Confidentiality** (C1): the core promise SC-1 ("nobody else can read it").
- **Privacy** (P1–P8): most of the data is health, mood and religious data, some of it about people without accounts.

One category is left out:
- **Processing integrity** (PI1): it covers systems whose job is to process something correctly for the customer, such as payments, orders, payroll or calculated results. Khas Tracker stores encrypted records the user's own device produced, and the server can't read them, so it can't check or change their meaning. Charging for access doesn't change that: payments would be taken by a payment provider, which would be a vendor (Phase 5), not part of this system. It gets added if the service ever produces results for users on the server.

**ISO/IEC 27001:2022.** The second target under D-009. Clauses 4–10 are built into the existing phases, and the [Statement of Applicability](statement-of-applicability.md) covers all 93 Annex A controls.

### Reference only

**NIST CSF 2.0.** Every control maps to a CSF subcategory. Phase 9 scores maturity against the CSF tiers.

**NIST SP 800-53 Rev 5.** Every control maps to 800-53. There's no federal system, so no baseline is required. The Access Control (AC) family is tailored in full from the SP 800-53B moderate baseline to show how tailoring works (D-017).

**FTC Health Breach Notification Rule (16 CFR Part 318, amended 2024).** The 2024 amendment brought many health and fitness apps under the rule. Today it likely doesn't reach Khas Tracker, because a free, non-commercial service for friends and family is outside the FTC Act's "in or affecting commerce" reach.

**Once access is paid, that reason goes away,** so the plan is to treat the rule as applying from then on. Two parts of the rule still narrow it:
- **The "personal health record" definition.** The rule covers apps with the *technical capacity* to draw health data from more than one source. Khas Tracker only holds what users type in, so whether it qualifies is a close call. Treating it as covered is the safe choice.
- **Only *unsecured* health information counts.** Content encrypted on the user's device, with keys the operator never holds, isn't unsecured. So a server-side breach exposing only ciphertext wouldn't trigger notice. A changed app that captured passphrases (R-02, R-04) would, because it defeats the encryption.

If it applies, affected users must be told within 60 days of discovering a breach, and the FTC too when 500 or more people are affected. Phase 7 builds this into the breach-notification decision tree.

**FTC Act section 5.** Once the service is commercial, its public promises ("nobody else can read it", "no ads, trackers or analytics") become claims the FTC can hold it to. The controls behind SC-1 and SC-2 matter more, not less.

**State breach notification laws (the states where users live).** These cover personal information such as names combined with ID or account numbers, and in some states health information. Each has a safe harbor for encrypted data when the key isn't compromised. User content is end-to-end encrypted, and the readable account data is mostly email addresses. So most incidents would not trigger notice, but some could. Phase 7 builds the decision tree from these laws.

### Does not apply today, prepared for

**GDPR and UK GDPR.** No users are in the EU or UK, and no data is processed there. But inviting even one person in the EU or UK would bring the GDPR in (Article 3(2): offering a service to people in the EU, paid or free), and the UK GDPR works the same way. Most of the data is special-category data (health and religion, Article 9). So the work has to be ready *before* that invitation, not after. Under D-026, Phase 6 prepares a readiness pack (control PRV-04):
- **Lawful basis and explicit consent:** Article 6, plus Article 9(2)(a) explicit consent for health and religious data, with consent text in the setup screens.
- **A data protection impact assessment** (Article 35), because special-category data is processed.
- **A record of processing** (Article 30).
- **Rights handling:** access, correction, erasure and data portability (Articles 15–20). The encrypted backup download (BCK-05) helps with portability.
- **Breach notice:** to the supervisory authority within 72 hours unless the breach is unlikely to put people at risk (Article 33). Encryption usually removes the need to tell users (Article 34(3)(a)).
- **The vendor terms with AWS:** its data processing addendum, and AWS's EU-US Data Privacy Framework certification and standard contractual clauses.
- **An EU (and UK) representative** (Article 27). The exemption for occasional processing is unlikely to fit an ongoing service holding health data, so this would probably mean a paid representative service. That's a cost to decide on before the first EU or UK invitation.

Until the pack is ready, the account-creation check (GOV-05) stops an EU or UK invitation.

### Does not apply

**NIST SP 800-171 Rev 3 and CMMC 2.0.** Both protect Controlled Unclassified Information (and, for CMMC Level 1, Federal Contract Information) held by federal contractors. Khas Tracker holds neither. The 800-171 mapping column is kept because it costs little once 800-53 is mapped.

**FedRAMP.** It applies to cloud services used by US federal agencies. There are none.

**HIPAA.** It applies to covered entities (healthcare providers, health plans, clearinghouses) and their business associates. Khas Tracker is neither: users record their own health data for themselves, not for or from a healthcare provider.

**COPPA.** It applies to collecting personal information *from* children under 13 through a service aimed at children or with actual knowledge. There are no users under 18 (D-005). The Parenting section lets adult users record information *about* their children, which COPPA doesn't cover. That data is still treated as sensitive data about people without accounts (D-006).

**Comprehensive state privacy laws.** Charging for the app doesn't by itself bring these laws in. What they look at is how many residents' data you hold, whether you make money from *selling personal data*, and, in a few states, whether you are a small business.
- **Most use thresholds.** The lowest per-resident threshold in the users' states is 35,000 residents in a year, or 10,000 plus more than 20% of revenue from selling personal data. With 10 users, Khas Tracker is far below every threshold, paid or not.
- **A few use the SBA small-business definition instead of a number.** Their law reaches anyone whose service residents use, unless they are a small business. A one-person project, even with some subscription revenue, stays a small business, so it's exempt.
- **Some duties apply even to small businesses or at low thresholds.** Examples are consent before selling sensitive data, and in the strictest state, collecting sensitive data only when strictly necessary, with no sale of it at all. Khas Tracker sells no data, has no ads or trackers (SC-2), and encrypts content so the operator can't use it, so it would already meet these. Designing to the strictest of these laws now keeps growth from becoming a rebuild.

**Consumer health data laws (Washington My Health My Data Act, Nevada SB 370, Connecticut).** These protect residents of those states, or data processed there, and Washington's has no size threshold. No user lives in Washington, Nevada or Connecticut, and the AWS regions used are in Virginia and Oregon. Trigger T-03 stays live, because one new user in Washington would bring the Washington act into scope.

## Review

This table is reviewed whenever a compliance trigger fires, when a new user is invited from a new state or country, and at the quarterly review (Phase 9).
