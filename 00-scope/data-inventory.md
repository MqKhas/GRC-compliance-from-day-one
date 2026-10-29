# Data Inventory & Classification

As of 2026-10-29, system version `tracker@9124656`. The machine-readable version is [data-inventory.csv](data-inventory.csv).

## Classification scheme

| Level | Meaning | Example here |
|---|---|---|
| **Restricted** | Exposure would seriously harm a person or fully compromise the system. Strongest protection | User content, passphrase, keys, admin credentials |
| **Confidential** | Personal or internal data whose exposure causes real but limited harm | Email addresses, record metadata, source code |
| **Internal** | Not meant to be public, low harm if exposed | App preferences, logs without personal data |
| **Public** | Meant to be public | App files served to browsers, demo sample data |

Separate from the level, a **sensitivity flag** marks data that many privacy laws treat as special: `HEALTH`, `MENTAL-HEALTH`, `RELIGION`, `THIRD-PARTY` (about someone who isn't the user), `CREDENTIAL`.

## Inventory

### A. User content (always stored as ciphertext outside the device)

| ID | Data | Flags | Level | Stored where | Admin can read? | Retention |
|---|---|---|---|---|---|---|
| D-01 | Weight, blood pressure, pulse, waist, sleep | HEALTH | Restricted | Device; DynamoDB and backups as ciphertext | No | Until the user deletes it or the account is deleted; backups up to 3 years |
| D-02 | Calories, protein, meals | HEALTH | Restricted | Same as D-01 | No | Same as D-01 |
| D-03 | Training programs, exercise logs, phase test results | HEALTH | Restricted | Same as D-01 | No | Same as D-01 |
| D-04 | Wellbeing check-ins about **the user** (emotion, feelings, notes) | MENTAL-HEALTH | Restricted | Same as D-01 | No | Same as D-01 |
| D-05 | Wellbeing check-ins about **other named people** (for example a spouse), with "she told me / I noticed" and notes | MENTAL-HEALTH, THIRD-PARTY | Restricted | Same as D-01 | No | Same as D-01 |
| D-06 | Qadha (make-up) prayer and fast counts | RELIGION | Restricted | Same as D-01 | No | Same as D-01 |
| D-07 | Home & Marriage tasks, chores, notes, plans | THIRD-PARTY | Restricted | Same as D-01 | No | Same as D-01 |
| D-08 | Friends & Family list: names, relation, last contact, conversation notes | THIRD-PARTY | Restricted | Same as D-01 | No | Same as D-01 |
| D-09 | Goals, weekly reviews, sick and travel days, follow-ups | HEALTH (sick days) | Restricted | Same as D-01 | No | Same as D-01 |
| D-10 | Settings and lists (sections on/off, order, targets, custom exercises) | — | Internal | Same as D-01 | No | Same as D-01 |

### B. Keys and secrets

| ID | Data | Flags | Level | Stored where | Admin can read? | Retention |
|---|---|---|---|---|---|---|
| D-11 | Data passphrase | CREDENTIAL | Restricted | User's memory only. Never stored or sent | No | n/a |
| D-12 | Recovery code | CREDENTIAL | Restricted | Shown once on the device. Optionally emailed by the user to themselves through their own mail app | No | Until replaced |
| D-13 | Keyring (data key wrapped by passphrase and by recovery code) | CREDENTIAL | Restricted | Device; DynamoDB and backups | Can read the wrapped form only, useless without the passphrase or code | Life of the account; backups up to 3 years |
| D-14 | Unlocked data key | CREDENTIAL | Restricted | Device only, as a non-extractable key in IndexedDB while unlocked | No | Cleared on lock |

### C. Account and access data

| ID | Data | Flags | Level | Stored where | Admin can read? | Retention |
|---|---|---|---|---|---|---|
| D-15 | User email address | — | Confidential | Cognito | Yes | Life of the account |
| D-16 | Login password | CREDENTIAL | Restricted | Cognito (managed by AWS, not readable) | No | Life of the account |
| D-17 | TOTP secret | CREDENTIAL | Restricted | Cognito; user's authenticator app | No | Life of the account |
| D-18 | Account status, created and last-modified times | — | Confidential | Cognito | Yes | Life of the account |
| D-19 | Session tokens (ID, access, refresh) | CREDENTIAL | Restricted | Device | No | 1 hour (ID/access); 30 days (refresh) |
| D-20 | Temporary password in the invitation email | CREDENTIAL | Restricted | Sent through SES to the user's mailbox | Not shown to the admin | Valid 7 days; stays in the user's mailbox |

### D. Metadata and operational data

| ID | Data | Flags | Level | Stored where | Admin can read? | Retention |
|---|---|---|---|---|---|---|
| D-21 | Record names (`day#YYYY-MM-DD`, `week#…`, `month#…`, `config#<name>`), timestamps, version, size, deleted flag | — | Confidential | DynamoDB; backups | **Yes**. Shows which days a user logged something | Same as the record |
| D-22 | Lambda logs (errors) | — | Internal | CloudWatch Logs | Yes | 14 days |
| D-23 | Billing and budget alerts | — | Internal | AWS; administrator's email | Yes | Per AWS and the mailbox |
| D-24 | User support requests (texts, emails) | — | Confidential | Administrator's phone and email. Not logged | Yes | Not defined |

### E. System and administrative data

| ID | Data | Flags | Level | Stored where | Admin can read? | Retention |
|---|---|---|---|---|---|---|
| D-25 | Administrator credentials and MFA (AWS root, AWS admin, GitHub, Google, Apple) | CREDENTIAL | Restricted | Administrator's password manager and identity providers | Yes (owner) | Ongoing |
| D-26 | App source code and infrastructure code | — | Confidential | Private GitHub repo; administrator's computer | Yes | Ongoing |
| D-27 | Deployed app files | — | Public | S3 behind CloudFront | Yes | Replaced each deploy |
| D-28 | Stack identifiers (account ID, user pool ID, client ID, table name) | — | Confidential | AWS. The pool and client IDs are also in `config.json`, which every browser downloads by design, so they aren't secret, but they're kept out of this repo anyway | Yes | Ongoing |
| D-29 | Demo sample data | — | Public | Generated in the browser tab; never stored or sent | n/a | Until the tab is closed |

## Key observations

1. **Most of the data is special-category.** Health, mental health and religious practice each get extra protection under several privacy laws. That raises the stakes for Phases 1 and 6, even with only 10 users.
2. **People who never signed up are data subjects.** D-05, D-07 and D-08 describe spouses, family and friends who have no account and never agreed to anything (D-006). Phase 6 has to deal with this.
3. **End-to-end encryption changes who can see what.** The administrator can't read any of section A. But metadata (D-21) and account data (D-15, D-18) are readable, so the privacy analysis can't say "we see nothing."
4. **Retention is defined by technology, not by policy.** The longest retention (3 years of backups) comes from a setting in the stack, not from a decision about how long data should be kept. Phase 6 sets a retention schedule.
5. **Support requests aren't recorded (D-24).** There's no way to show that a deletion request was received and handled.
