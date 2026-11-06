# Primer: Phase 2, Control framework & mapping

A 5-minute read before building the control matrix, Statement of Applicability and crosswalk in [`02-controls/`](../02-controls/).

## What this phase is and why it exists

Phase 1 answered "what could go wrong?" Phase 2 answers "what do we actually do about it, and how does that line up with the frameworks people will judge us against?"

A **control** is a repeatable activity or setting with an owner, a frequency and evidence that it happens. For example: "MFA is required for every user, enforced by the identity service; evidence in the infrastructure code."

The approach is a **common control framework** (decision D-009): write one set of controls, then **map** each one to the requirements of every framework that matters. You don't run a separate program per framework. The mapping is many-to-many. One control can meet several framework requirements, and one requirement can need several controls.

**In SOC terms:** it works like detection coverage against MITRE ATT&CK. One detection rule covers several techniques, and one technique is covered by several rules. A coverage map shows the holes.

**Who reads the output:**
- **A SOC 2 auditor** reads the control matrix to plan testing. For a Type I report, they check that each control is designed well and in place on one date.
- **An ISO 27001 auditor** usually starts with the Statement of Applicability. They look for all 93 Annex A controls, each with a reason it's included or excluded, plus its status.
- **A customer security reviewer** uses the crosswalk to answer their own questionnaire.
- **The engineer** (the owner) gets a to-do list: every gap with a due date.

## The frameworks, in the shape we use them

| Framework | Structure | What we produce |
|---|---|---|
| SOC 2 (AICPA Trust Services Criteria, 2017, revised points of focus 2022) | Common Criteria CC1–CC9 cover Security; C1 covers Confidentiality; P1–P8 cover Privacy | A mapping column in the control matrix |
| ISO/IEC 27001:2022 Annex A | 93 controls in 4 themes: organizational (5.1–5.37), people (6.1–6.8), physical (7.1–7.14), technological (8.1–8.34) | The **Statement of Applicability**, required by clause 6.1.3 d |
| NIST CSF 2.0 | 6 functions: Govern, Identify, Protect, Detect, Respond, Recover, split into categories and subcategories | A mapping column (maturity scoring comes in Phase 9) |
| NIST SP 800-53 Rev 5 | 20 control families. Baselines (low, moderate, high) are in **SP 800-53B** | A mapping column, plus one family tailored in full |
| NIST SP 800-171 Rev 3 | 97 requirements in 17 families, derived from the 800-53 moderate baseline | A mapping column only. It doesn't apply here (no Controlled Unclassified Information) |

NIST publishes official crosswalks (800-53 to CSF 2.0, and 800-171 to 800-53), and AICPA publishes TSC mappings. Those are the starting point. Where a mapping is our own judgment, we say so.

## Statement of Applicability (ISO 27001 clause 6.1.3 d)

The SoA lists every Annex A control and records, for each one:
1. Whether it's included.
2. Why it's included (which risk, law or commitment needs it) or why it's excluded.
3. Whether it's implemented.

Excluding a control is fine **with a reason**. An auditor would push back on excluding the physical controls (7.x) without naming who carries them. Here, AWS runs the data centers, so we rely on AWS for those controls (a carve-out). That isn't the same as "not applicable."

## Tailoring a baseline (SP 800-53B §2.4, SP 800-37 Task P-12)

A baseline is a starting point, not a requirement. Tailoring changes it on purpose, and every change needs a written reason:
- **Scoping:** remove controls that don't fit the system, such as controls inherited from the cloud provider.
- **Compensating controls:** use a different control that gives similar protection when the baseline control isn't possible. Example: separation of duties (AC-5) is impossible for one person, so a pull-request self-review checklist plus a deploy notice takes its place.
- **Parameter values:** fill in the blanks the control leaves open, such as how long logs are kept.
- **Supplementing:** add controls your risk assessment calls for.

## End-to-end encryption as a compensating control

Many controls exist to stop insiders or the cloud provider from reading data. Customer-managed encryption keys (800-53 SC-12 and SC-28) and access reviews on the database are two examples. In Khas Tracker the data is already ciphertext made on the user's phone. A customer-managed key would only re-encrypt data that is already unreadable to the operator and to AWS, so it adds cost and little protection. We record it as "met by a compensating control: client-side end-to-end encryption", with evidence, rather than dropping it without a reason.

End-to-end encryption doesn't protect code integrity, availability or metadata. That's why risks R-02, R-04 and R-09 still exist.

## Good vs. bad

**Good:**
- Each control is a testable statement: who, what, how often, and where the evidence is.
- Every control traces to the risk IDs it treats.
- Gaps are labeled as gaps, with due dates.
- Every exclusion has a reason.
- Mappings come from official crosswalks where they exist.

**Bad:**
- Framework text copied in as "controls".
- One control per framework line, giving 400 near-duplicates.
- "N/A" with no reason.
- A planned action marked "implemented".
- Any claim of being "compliant" or "certified". The honest words are "readiness against" or "aligned to".
