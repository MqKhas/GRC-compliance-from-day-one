# Interview Prep

Questions from the end-of-phase quizzes. For each one: my answer first, then feedback and a model answer. Practice saying these out loud.

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

### Q4. "You only have 10 users who are friends and family. Why bother classifying data as health, religious or third-party?"

**My answer:** Because that IS what the data is. And because I want to practice increasing my security and GRC posture.

**Feedback:** The first sentence is the right instinct. Drop the second in an interview, because it makes the work sound like a hobby instead of a judgment. Add *why* it matters: harm to a person doesn't shrink because the user count is small, classification decides how strong the controls must be, some health-data laws have no size threshold, and third-party people never agreed to be recorded.

**Model answer:** "Because classification follows the data, not the user count. Mood, blood pressure and religious practice are special-category data, and if they leaked, the harm to one person is the same whether I have 10 users or 10,000. Classification is also what decides control strength. It's why I treat metadata as confidential and backups as part of retention. And some of it is about people who never signed up, like a spouse in a wellbeing check-in, which raises the bar further."

### Q5. "You're the only admin. Isn't SOC 2 kind of pointless for a one-person company?"

**My answer:** Yes and no. You're right that there's a big gap because, for example, I am the one publishing all changes. But this is for the routine/muscle memory and just because something isn't perfect, doesn't mean we shouldn't strive at all. Just because I don't have one more person, that doesn't mean that I just give up and leave all the data for the wolves.

**Feedback:** Right instinct, and good that you named the gap yourself. Make it more professional: skip "muscle memory" and "wolves," and show *how* you handle the gap (compensating controls and documented risk acceptance). That's the GRC answer.

**Model answer:** "Not pointless, but it has to be scaled. Plenty of companies go through SOC 2 with only a handful of people. The framework asks whether controls fit the risk, not whether you have a big team. The real gap for me is separation of duties: I write, approve and deploy every change. I can't fix that with one person, so I document it as an accepted risk and add compensating controls: tests that must pass before deploy, a change history that can't be quietly rewritten, and a record of every release. The goal isn't zero gaps. It's that every gap is known, owned and managed."
