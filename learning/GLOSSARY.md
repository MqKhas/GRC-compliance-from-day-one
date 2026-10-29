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
| Subservice organization | A vendor that performs part of your service, such as AWS hosting your app | "My subservice organizations are vendors I depend on to deliver the service, like AWS and GitHub." |
| Carve-out vs. inclusive method | Carve-out: you rely on a vendor's controls but don't test them (you rely on their own report). Inclusive: their controls are tested as part of your audit | "I carve out AWS, so I rely on their SOC 2 and list the controls I'm assuming they perform." |
| Service commitments | The promises a company makes to its users about its service, which SOC 2 tests controls against | "My main commitment is that nobody, not even me, can read user data, so my encryption controls trace back to that promise." |
| Record metadata | Data about data: record names, timestamps, sizes. Often left unencrypted even when content is encrypted | "End-to-end encryption hides content, not patterns. Metadata still says when someone used the app." |
| CUEC | Complementary User Entity Controls: things the customer must do for the vendor's controls to work (for example, protect their own device and passphrase) | "A SOC 2 report lists what the customer is responsible for. For my app, that's protecting the passphrase and recovery code." |
| CSOC | Complementary Subservice Organization Controls: things you rely on a carved-out vendor (like AWS) to do | "I carve out AWS and rely on their SOC 2 for physical security, but I list exactly what I'm assuming they do." |
| Change management | The controlled process for getting a change into production: request, review, test, approve, deploy, and record it | "Every production change should be traceable to who made it, who approved it, and how it was tested." |
| Branch protection | GitHub rules on a branch like `main`: require a pull request, passing checks, or approval before code can land | "Branch protection turns change management from a policy into something the platform enforces." |
| Single point of compromise | One account or device that, if taken, gives an attacker everything | "If all the MFA factors sit in one vault, that vault becomes the real security boundary." |
| Compliance trigger | A future event that changes what rules apply, written down in advance with the review it requires | "I can't comply with everything up front, so I document triggers and review them before they happen." |
| Assumption vs. fact | A fact is verified with evidence; an assumption is recorded as one, with the reason | "In scoping I label assumptions explicitly so nobody mistakes them for verified facts." |
| POA&M | Plan of Action and Milestones: the list of known gaps, each with an owner, fix and due date | "A POA&M turns a gap from a weakness into a managed, tracked commitment." |
