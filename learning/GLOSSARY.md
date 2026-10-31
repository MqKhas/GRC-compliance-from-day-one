# Glossary

Plain-language definitions, plus how I'd explain each one in a sentence. Added as terms come up.

| Term | What it means | In one sentence |
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
| NIST SP 800-30 | NIST's guide to conducting risk assessments: a risk model, a process, and five-level scales for likelihood, impact and risk | "800-30 gave me the engine for my risk assessment, and I wrote its scale definitions for my own system." |
| Threat source / threat event | The source is who or what causes harm (an attacker, a mistake, an outage). The event is what actually happens (phishing, a bad deploy) | "The source is the actor; the event is what they do, and that's where ATT&CK technique IDs fit." |
| Vulnerability vs. predisposing condition | A vulnerability is a fixable weakness. A predisposing condition is a fact about the system that makes some risks likelier or worse, usually accepted as part of the design | "No branch protection is a vulnerability; being a one-person operation is a predisposing condition." |
| Likelihood | How likely an event is to happen and cause harm, here within 12 months | "I score likelihood as the chance of harm in the next year, with a written reason for every score." |
| Impact | How bad the harm would be, rated by the worst effect on people, the service or the operator | "Impact is about the worst realistic harm, not the average one." |
| Risk matrix (SP 800-30 Table I-2) | A lookup table that turns likelihood and impact into a risk level | "I use 800-30's matrix rather than multiplying, because it keeps rare but catastrophic risks visible." |
| Inherent risk | The risk without the system's own controls | "Inherent risk shows how much my controls are actually doing." |
| Current (residual) risk | The risk with the controls that exist today. Unverified controls don't count | "I only give credit for controls I can show working." |
| Target risk | The risk expected after the planned treatment is done | "Target risk is what I'm committing to get to, and by when." |
| Risk appetite | A plain statement of how much and what kind of risk you're willing to take | "My appetite is very low for anyone reading user data, and moderate for a day of downtime." |
| Risk acceptance criteria | The measurable rules for when a risk may be accepted, and by whom | "Low risks can be accepted by default; High ones must be treated or signed off as an exception with a review date." |
| Risk treatment (mitigate, accept, transfer, avoid) | The four choices for a risk: reduce it, keep it on purpose, share it with someone else, or stop the risky activity | "Accepting a risk is a decision with an owner, a reason and a review date, not a shrug." |
| Risk owner | The person accountable for a risk and allowed to decide its treatment | "Every risk needs a named owner; in my program that's me for all of them." |
| Risk treatment plan | ISO 27001's required plan of treatment actions, controls, dates, and the risk owner's approval of the residual risk | "The treatment plan turns risk decisions into dated actions someone has signed off." |
| Heat map | A grid of likelihood against impact showing where risks sit | "One glance at the heat map shows my top risks are all low-likelihood, high-impact." |
| MITRE ATT&CK | A public catalogue of real attacker techniques, each with an ID | "Mapping risks to ATT&CK links the risk register to how attacks actually happen in a SOC." |
| Supply-chain attack | Compromising software you depend on, like a package or a CI action, to reach you | "My deploy pipeline runs other people's code with production access, so it's a supply-chain risk." |
| Trusted Types | A browser security feature, set in the Content Security Policy, that blocks unsafe ways of turning text into HTML or script | "Trusted Types turns 'we never use innerHTML' from a coding habit into something the browser enforces." |
