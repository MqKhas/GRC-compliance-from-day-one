# Glossary

Plain-language definitions, plus how I'd say each one in an interview. Added as terms come up.

| Term | What it means | How I'd say it in an interview |
|---|---|---|
| GRC | Governance (who decides and how), Risk (what could go wrong and how much it matters), Compliance (proving you meet a standard or law) | "GRC is how a company decides what security it needs, and then proves it's actually doing it." |
| SOC 2 | An attestation report by a CPA firm on a service company's controls, judged against the AICPA Trust Services Criteria | "SOC 2 is how a SaaS company shows customers its controls are real, through an independent auditor." |
| Type I vs. Type II | Type I: are controls designed well at one point in time. Type II: did they operate effectively over a period, usually 3–12 months | "Type I is a snapshot of design. Type II proves the controls actually worked over time." |
| System boundary | The line around what's in scope: the people, processes, technology and data being assessed | "Scoping is the first decision, because everything after it depends on what's inside the boundary." |
| Separation of duties (SoD) | No single person can both make and approve a sensitive change, so one mistake or bad actor can't act alone | "With one admin I can't have true SoD, so I document it as an accepted risk and add compensating controls like logging and change history." |
| Key-person risk | The business depends on one person; if they're unavailable, things stop | "Bus factor of one is a real risk, even if the data is safe." |
| Compensating control | A different control that reduces the same risk when the ideal control isn't possible | "When the standard control doesn't fit, I show what else covers the risk and why it's enough." |
| CUEC | Complementary User Entity Controls: things the customer must do for the vendor's controls to work (for example, protect their own device and passphrase) | "A SOC 2 report lists what the customer is responsible for. For my app, that's protecting the passphrase and recovery code." |
| CSOC | Complementary Subservice Organization Controls: things you rely on a carved-out vendor (like AWS) to do | "I carve out AWS and rely on their SOC 2 for physical security, but I list exactly what I'm assuming they do." |
| POA&M | Plan of Action and Milestones: the list of known gaps, each with an owner, fix and due date | "A POA&M turns a gap from a weakness into a managed, tracked commitment." |
