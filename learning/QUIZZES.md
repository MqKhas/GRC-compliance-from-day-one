# Quizzes

At the end of each phase I quiz myself on what I just built. For each question: my answer first, then feedback and a model answer.

## Phase 0 — Scope & system description (2026-10-29)

### Q1. "Walk me through how you decided what was in scope for your app's assessment."

**My answer:** I tried to think about what was truly involved with the tracker app. If it directly related in any way, it was in scope. I decided that the rest of my portfolio site would be carved out because although it uses the same domain, it is just static info that doesn't have anything to do with the tracker app.

**Feedback:** Good. Clear rule and a concrete carve-out with a reason. Missing the most important nuance: the personal site was carved out, but the things it *shares* with the tracker (the AWS account, admin access, billing, DNS) stayed in scope. That's what shows you understand that scope is about what can affect the system, not just what it's labeled.

**Model answer:** "My rule was: anything that stores or processes user data, or can change the system, is in scope. That pulled in the obvious AWS services, but also GitHub, because a push to main deploys production, and my own Apple and Google accounts, because they protect admin access. I carved out my personal website, which is static and holds no user data. It lives in the same AWS account, though, so the account-level controls it shares, like root access, billing and DNS, stayed in scope. Every one of those calls is in a decisions log with the reason."

### Q2. "Your app is end-to-end encrypted. Does that mean you, as the admin, can't see anything about your users?"

**My answer:** Well I can see their email and if they've created a new password. But no other data.

**Feedback:** Partly right, and the gap is exactly what this question tests. You can see more than that: account status and timestamps, and **record metadata**. Record names like `day#2026-10-26`, plus when each was updated, show which days someone used the app and how often. Small correction: you can see the account *status* (for example, whether a temporary password has been changed), not anything about the password itself.

**Model answer:** "No, and I'm careful not to claim that. I can't read any content: health, mood, notes, all of it is encrypted on the phone. But I can see each user's email and account status, and the metadata on their records, like which dates have entries and when they were updated. That reveals activity patterns, so the privacy analysis treats metadata as personal data, not as nothing."

### Q3. "What's the difference between a carved-out subservice organization and a complementary user entity control? Give an example of each."

**My answer:** A carved-out subservice org would be like the rest of my website, while a complementary user entity control would be like my Apple or Google accounts.

**Feedback:** Both examples are off. This is the one to re-learn.
- A **subservice organization** is a *vendor* that performs part of your service. **Carve-out** means you rely on their controls without testing them yourself, usually by relying on their own SOC 2. Examples: AWS, GitHub. Your personal website is a carved-out *part of scope*, not a subservice organization.
- A **CUEC** is something your *users* (customers) must do for your controls to work. Example: users must keep their passphrase and recovery code safe. Your Apple and Google accounts are *your* admin identities, so they're in scope (D-008). Apple and Google *as companies* are subservice organizations.
- Memory hook: **subservice = vendors I depend on. CUEC = what my customers must do.**

**Model answer:** "A subservice organization is a vendor that runs part of my service. For my app that's AWS. I use the carve-out method, so I don't test AWS's data center controls; I rely on their SOC 2 report and list the controls I'm assuming they perform, which are the CSOCs. A complementary user entity control is something the customer has to do. For me, the big one is that users must protect their passphrase and recovery code, because if they lose both, no one can recover their data."

**Retry (2026-10-29):** A subservice org is a service that is being utilized under you. In this case, it's AWS, GitHub. While a complementary user entity control is what a person/user USING my service (the app) needs to do to continue using the app, which is saving the password/mfa/passphrase/username, making sure not to leave their phone open and laying around, etc.

**Feedback on retry:** Correct now, with the right examples for both. Two refinements:
- Say "a vendor that performs part of my service," not "under you." Then add the carve-out: "I don't test AWS's controls myself; I rely on their SOC 2 report."
- A CUEC isn't what users do "to continue using the app." It's what they must do **for my controls to work**. If a user leaks their passphrase, my encryption worked perfectly and their data is still exposed. That's why the duty gets written down.

### Q4. "You only have 10 users who are friends and family. Why bother classifying data as health, religious or third-party?"

**My answer:** Because that IS what the data is. And because I want to practice increasing my security and GRC posture.

**Feedback:** The first sentence is the right instinct. The second is about me, not the data, so it doesn't answer the question. Add *why* it matters: harm to a person doesn't shrink because the user count is small, classification decides how strong the controls must be, some health-data laws have no size threshold, and third-party people never agreed to be recorded.

**Model answer:** "Because classification follows the data, not the user count. Mood, blood pressure and religious practice are special-category data, and if they leaked, the harm to one person is the same whether I have 10 users or 10,000. Classification is also what decides control strength. It's why I treat metadata as confidential and backups as part of retention. And some of it is about people who never signed up, like a spouse in a wellbeing check-in, which raises the bar further."

**Retry (2026-10-29):** Classifying the data, firstly, because that is what the data is. Secondly, it helps us decide what type of controls we need to construct AROUND the data.

**Feedback on retry:** Much stronger. "Classification drives the controls" is the core idea, and it's said like a practitioner now. To make it complete, add one concrete example: "Health and mood data are Restricted, which is why they're encrypted on the device and why backups count toward retention."

### Q5. "You're the only admin. Isn't SOC 2 kind of pointless for a one-person company?"

**My answer:** Yes and no. You're right that there's a big gap because, for example, I am the one publishing all changes. But this is for the routine/muscle memory and just because something isn't perfect, doesn't mean we shouldn't strive at all. Just because I don't have one more person, that doesn't mean that I just give up and leave all the data for the wolves.

**Feedback:** Right instinct, and good that you named the gap yourself. Make it more professional: skip "muscle memory" and "wolves," and show *how* you handle the gap (compensating controls and documented risk acceptance). That's the GRC answer.

**Model answer:** "Not pointless, but it has to be scaled. Plenty of companies go through SOC 2 with only a handful of people. The framework asks whether controls fit the risk, not whether you have a big team. The real gap for me is separation of duties: I write, approve and deploy every change. I can't fix that with one person, so I document it as an accepted risk and add compensating controls: tests that must pass before deploy, a change history that can't be quietly rewritten, and a record of every release. The goal isn't zero gaps. It's that every gap is known, owned and managed."

## Phase 1 — Risk assessment (2026-10-31)

### Q1. "Walk me through how you scored R-01, the admin identity risk. Why is it Moderate and not High?"

**My answer:** The way I thought about this was about Likelihood vs Impact. The likelihood of this happening is low because I have strong controls around my admin identity. I have a strong password as well as 2fa. The impact if it does happen could be devastating. They could completely wipe out the code for the app. If one of my other accounts gets compromised, then it takes out the password/2fa, because the attacker will have both. To say it simply, the likelihood is low but the impact is Very High.

**Feedback:** Right structure and right scores (Low × Very High). Three fixes:
- Name the method. It's Moderate because SP 800-30 Table I-2 says Low × Very High = Moderate. It would be High only if likelihood were Moderate. That's the actual answer to "why not High."
- "Strong controls" oversells it. The weakness *is* the control: the second factor isn't independent of the password, which is exactly why likelihood is Low and not Very Low. You said this, but last, as an afterthought. Lead with it.
- The impact is wrong in the detail. The code lives in GitHub, not AWS. The damage from an AWS takeover is control of production: deleting the data *and its backups*, deploying a capturing build, running up the bill.

**Model answer:** "I score likelihood and impact separately, then read the level from the SP 800-30 matrix. Impact is Very High: whoever holds the AWS account controls production, can delete the data and the backups, and could serve a build that captures passphrases. Likelihood is Low, not Very Low, because MFA is on but the second factor isn't independent of the password store, so one compromise can give both. Low times Very High is Moderate on Table I-2. It isn't High because I don't see it as likely within a year. The treatment, phishing-resistant keys held separately plus locked backups, takes likelihood to Very Low, which makes it Low."

### Q2. "What's the difference between inherent and current risk? And why didn't you give yourself credit for CloudTrail when scoring?"

**My answer:** Inherent risk is the risk without any controls. Current risk is taking into account the controls I already have in place. Because there is a very basic CloudTrail implementation. I'm not collecting logs properly, in the sense that, I could not say what was exposed (if anything was) or explain any security alerting, as there is no alerting.

**Feedback:** The definitions are right. The CloudTrail part misses the point.
- Small refinement: inherent here means without *my system's own* controls. What AWS does (like physical security) still counts, and the methodology says so.
- The reason for no credit isn't that CloudTrail is weak. It's that it's **unverified**. You *believe* a trail is on, but you haven't shown it. The rule is "unverified controls don't lower the score until Phase 4 proves them." "There is a very basic CloudTrail implementation" is itself an unverified claim. An auditor would stop you right there and ask for evidence.
- What you described (can't tell what was exposed, no alerting) is the *impact* of the gap. That belongs in R-06's scoring reason, not in this answer.

**Model answer:** "Inherent risk is the risk without my system's own controls; current risk counts the controls that exist today. But I only count controls I can show working. I believe a CloudTrail trail is on, but I haven't verified it, so I gave it no credit. The only logging I counted is AWS's default 90-day event history, which exists in every account. Phase 4 checks the trail with evidence, and only then would the score move. Giving credit for unverified controls is how registers end up full of green that doesn't hold up in an audit."

### Q3. "R-06 is still Moderate after your treatment plan. Isn't that a failure? Defend it to an auditor."

**My answer:** No this is not a failure because, once I put in my treatment plan, then that is the risk that I'm willing to accept. Just because my plan didn't change the level of risk, doesn't mean I shouldnt put in the effort to implement it. Sometimes with risks, they will always stay consistent. Example, if the likelihood is 3/4/5 but the impact is 3/4, then even if you put in controls to get the likelihood from very high to moderate and the impact from High to moderate, it still remains moderate because Likelihood vs impact risk matrix.

**Feedback:** The core idea is right: residual risk that's within your criteria and accepted in writing is not a failure. Two problems:
- The matrix example is wrong. Very High × High is **High** on Table I-2, so moving to Moderate × Moderate *does* change the level, from High to Moderate. The real point is narrower: the matrix is coarse, so a treatment can improve things without crossing a band.
- The strongest defence is missing: it was a **deliberate, documented decision**. You chose free controls over paid GuardDuty, under an appetite that prefers free controls. The acceptance criteria allow a Moderate residual with a written reason and a 6-month review, and you named what would make you reconsider. That's what an auditor needs to hear.

**Model answer:** "No. Risk management doesn't mean driving everything to Low; it means every residual risk is known, within the criteria, and owned. For R-06, I chose free controls: a verified audit trail and sign-in alerts. They make an incident far easier to investigate, but without a threat-detection service the likelihood stays where it is, so it's still Moderate. Skipping GuardDuty was a cost decision under my risk appetite, and my acceptance criteria allow a Moderate residual with a written reason. It's in the treatment plan, I signed it, and it's reviewed every six months. If there's an incident or the free controls prove too thin, GuardDuty is the first thing I revisit."

### Q4. "Your app is end-to-end encrypted. So why aren't your top risks about the database?"

**My answer:** Because it's encrypted, this greatly decreases the impact as it lowers the compromises to metadata/availability, but the data itself remains confidential.

**Feedback:** Correct, but it only answers half the question. You said why the database dropped, not what rose to the top instead, or why. The insight is that encryption happens in code you deliver. Whoever controls that code can capture the passphrase *before* encryption happens. So the risk moves from where the data sits to where the code comes from.

**Model answer:** "Because a stolen table is ciphertext. Encryption cuts a database breach down to metadata and availability. But encryption runs in code I deliver to the browser, and whoever controls that code can capture the passphrase before anything is encrypted. So my top risks are about who can change the code or the account it runs in: my admin identity, my GitHub account, the deploy pipeline's dependencies, and unreviewed changes. Encryption didn't remove the risk; it moved it from the data to the code path."

## Phase 2 — Control framework & mapping (2026-11-05)

### Q1. "Why can one control map to SOC 2, ISO 27001 and NIST at the same time, and what does that save you?"

**My answer:** One control can map to these different standards because the frameworks have different end goals/for different situations, but the way you get to the end goal is sometimes similar. Having a 12 character password, or 2fa are all controls around passwords and you need to have proper controls around access to reach your end goal, no matter the different framework.

**Feedback:** The reasoning is right: different frameworks, same underlying safeguards. Two things to add:
- You answered why it works, but not what it saves. You build and test the control once, and collect one set of evidence that answers every framework. Without that, you'd run a separate program for each.
- Use your real numbers. Passwords here are at least 10 characters, plus an authenticator code. Quoting a number from your own system shows you know it.

**Model answer:** "Frameworks ask different questions for different audiences, but they want many of the same safeguards. Multi-factor sign-in satisfies SOC 2 CC6.1, ISO 8.5, CSF PR.AA-03 and 800-53 IA-2 at once. So I write each control once and map it, which means one control to run and one set of evidence for every framework. A new framework becomes a new mapping column, not a new program. A mapping only means the control contributes to a requirement, not that it meets it alone."

### Q2. "Why is ISO control 6.1 (Screening) excluded, but 7.1 (Physical security perimeters) isn't, when you own no data center?"

**My answer:** Because we explained why it's not relevant, but 7.1 is still there cuz there is still physical security around the users device they are using to access the trackere.

**Feedback:** The 6.1 half needs the actual reason, and the 7.1 half is mistaken.
- "We explained why" is what an exclusion needs, but an auditor wants the explanation itself: there are no staff or contractors to screen.
- 7.1 isn't about users' devices. It's about the data centers that hold the data, and AWS provides that protection. Devices are 8.1 and 7.9, and users' own phones are outside what this system manages.
- The key idea is the difference between "doesn't exist" and "someone else does it". No staff means screening has nothing to act on. AWS's physical security still protects the data, so it stays in, marked as provided by AWS.

**Model answer:** "Screening has nothing to act on, because I'm the only operator. It comes back if a second person gets admin access, under trigger T-06. Physical security is different: the data sits in real data centers, and those controls still protect it. AWS provides them rather than me, so they stay applicable and are marked as provided by AWS. I don't take them on trust either: they stay unverified until I review AWS's audit reports in Phase 5."

### Q3. "What's the difference between a compensating control and scoping a control out? Give one example of each."

**My answer:** Compensating control is when an alternative safeguard is in place when the primary control cant be applied, scoping out is when you say this control isn't in scope of this framework. We scoped out the rest of my muhammad-khas.com site because it had nothing to do with the tracker even though it was on the same domain. A compensating control we had was we increased our monitoring of accounts, fi accounts are inactive we will review them every x days.

**Feedback:** The compensating definition is right. The rest needs adjusting:
- Scoping a control out means the control has nothing to act on in this system. The framework stays the same.
- Your website example is a real scoping decision, but it's about the **system boundary** (Phase 0), not about a control.
- The inactive account review isn't a compensating control. AC-2(3) is met directly, and 180 days is a **parameter value**. The real examples are below.
- The test: if the need exists but the control can't be done, you compensate. If the need doesn't exist, you scope out.

**Model answer:** "A compensating control gives similar protection when the real control can't be done, so the need is still there. AC-5 separation of duties is impossible with one person, so a pull request with a self-review checklist, automated checks and a deploy notice stand in. Scoping out means the need isn't there. AC-18 wireless access has nothing to act on, because the service runs no wireless network. Both need a written reason, and a scoped-out control needs a way back in if the system changes."

### Q4. "End-to-end encryption is your main compensating control. Name one thing it doesn't protect against, and which controls cover that."

**My answer:** E2EE doesn't protect against phishing attacks. If someone uploaded malicious code, then they could see the data.

**Feedback:** Your second sentence is the answer. The first is only partly right.
- Malicious code is the key weakness: a changed app could capture the passphrase before anything is encrypted.
- Phishing is more of a route to that. Phishing a user's login alone gets only ciphertext, because the passphrase is separate. Phishing an admin could lead to a changed app.
- You didn't name the controls, and that was half the question.

**Model answer:** "It doesn't protect against a changed app. Encryption runs in code I deliver, so a malicious build could capture the passphrase before anything is encrypted. That's why the code path carries the weight: phishing-resistant admin sign-in (IAM-07) protects who can deploy, pull requests and CI (CHG-01, CHG-02) control what gets deployed, and the deploy notice and user-visible changelog (CHG-05, CHG-06) make an unexpected change visible."
