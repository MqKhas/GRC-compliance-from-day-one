# Asset & Threat Register

| | |
|---|---|
| **Document owner** | Muhammad Khas |
| **As of** | 2026-10-31, system version `tracker@9124656` |
| **Method** | [methodology.md](methodology.md), following NIST SP 800-30 Rev 1 Tasks 2-1 to 2-3 |

The building blocks of the [risk register](risk-register.md): what is being protected (assets), who or what could cause harm (threat sources), and the weaknesses and conditions that make harm possible (vulnerabilities and predisposing conditions).

## Assets

Data IDs (D-xx) refer to the [data inventory](../00-scope/data-inventory.md).

| ID | Asset | Includes | C | I | A | Why it matters |
|---|---|---|---|---|---|---|
| A-01 | User content and keyrings | D-01 to D-10, D-13, stored as ciphertext in DynamoDB | High | High | Moderate | Health, mental health, religious and third-party data. Promise SC-1 depends on it |
| A-02 | Backups | Point-in-time recovery (35 days), daily backup vault (35 days), copy vault in a second region (3 years) | High | High | High | The only way back from data loss. Also holds deleted users' data |
| A-03 | Record metadata | D-21: record names, timestamps, sizes | Moderate | Low | Low | Readable by the administrator and AWS. Shows when users log, not what |
| A-04 | User accounts | D-15 to D-20 in Cognito | Moderate | High | Moderate | Gate to each user's ciphertext and the ability to delete it |
| A-05 | Web app as delivered | D-27: files in S3 served by CloudFront | Low | **Very high** | Moderate | Whoever controls these files controls the code that handles passphrases |
| A-06 | Source code and deploy pipeline | D-26: private GitHub repository and Actions workflow | Moderate | **Very high** | Low | A push to `main` deploys to production |
| A-07 | Deploy identity | GitHub OIDC deploy role and the CDK bootstrap roles it uses | n/a | **Very high** | Low | Can change any resource in the stack |
| A-08 | AWS account | Root user, administrator IAM user, billing, account-level settings | n/a | **Very high** | High | Full control of everything above |
| A-09 | Administrator identities and credential store | D-25: password manager and personal identity accounts used to reach AWS and GitHub (D-008) | **Very high** | **Very high** | High | The real perimeter for a one-person operation |
| A-10 | Domain, DNS and email identity | Domain registration, DNS zone, TLS certificate, SES sending domain | n/a | **Very high** | High | Controls where users' browsers go and who can send mail as the app |
| A-11 | Sync API | API Gateway and Lambda | n/a | High | Moderate | Enforces per-user access to records |
| A-12 | Logs and audit trail | Lambda logs (14 days); account-level audit logging **(unverified)** | Low | High | Moderate | Needed to detect and investigate an incident |
| A-13 | The operator | One person who builds, runs and supports the service | n/a | n/a | High | Nobody else can operate the service |
| A-14 | Privacy commitments and request handling | Commitments SC-1 to SC-5; D-24 support requests | n/a | n/a | n/a | How users' rights and expectations are met |

C, I, A = how much confidentiality, integrity and availability matter for that asset.

## Threat sources

Types follow SP 800-30 Table D-2. Capability, intent and targeting (Tables D-3 to D-5) apply to adversarial sources only.

| ID | Threat source | SP 800-30 type | Capability | Intent | Targeting | Notes |
|---|---|---|---|---|---|---|
| TS-01 | Opportunistic outsider (phishing kits, credential stuffing, bots) | Adversarial: individual or group, outsider | Moderate | Money: resell accounts, mine crypto, ransom | Low. Hits anyone | The most common real-world source for small cloud accounts |
| TS-02 | Targeted outsider | Adversarial: individual, outsider | Moderate | Personal: someone who knows the operator or a user | Moderate | Unlikely, but the users are a small, known group |
| TS-03 | Software supply-chain attacker | Adversarial: group, established | High | Broad credential and code theft | Low. Hits whoever installs the package | Compromised npm packages or GitHub Actions |
| TS-04 | Trusted insider: a user, or someone close to one | Adversarial: individual, trusted insider | Low | Curiosity, relationship conflict | High. One specific person | For example, a partner with access to a user's unlocked phone |
| TS-05 | Privileged insider: the operator, acting deliberately or under pressure | Adversarial: individual, privileged insider | Very high | None today. Included because the trust model depends on it | n/a | See R-04 |
| TS-06 | The operator's own mistakes | Accidental: privileged user or administrator | n/a | n/a | n/a | Wrong command, bad deploy, misconfiguration |
| TS-07 | Software and service faults | Structural: software, IT equipment | n/a | n/a | n/a | Bugs in the app or in AWS services |
| TS-08 | Infrastructure outage or disaster | Environmental: infrastructure failure | n/a | n/a | n/a | AWS region outage; loss of the operator's phone or computer |
| TS-09 | Operator unavailable | Added: not in Table D-2 (key person) | n/a | n/a | n/a | Illness, travel, life events |
| TS-10 | Legal and regulatory | Added: not in Table D-2 | n/a | n/a | n/a | A complaint, regulator inquiry or new law. ISO 27001 clause 4.2 brings these in |

## Vulnerabilities and predisposing conditions

SP 800-30 Appendix F. A **vulnerability** is a weakness that could be fixed. A **predisposing condition** is a fact about the system that makes some risks more likely or more severe. It is usually accepted as part of the design.

Open weaknesses are described at the level an auditor would write them, without detail that would help an attacker.

### Vulnerabilities

| ID | Vulnerability | Assets | Source | Status |
|---|---|---|---|---|
| V-01 | Administrator sign-in factors are not fully independent of each other | A-08, A-09 | Phase 0 interview | Open |
| V-02 | A personal identity account can sign in to the code host, and the code host deploys production with no approval step | A-06, A-09 | Phase 0 interview; `deploy.yml` | Open |
| V-03 | No branch protection or second reviewer on the app repository | A-06 | Phase 0 interview | Open |
| V-04 | The deploy role can use the CDK bootstrap roles, which have broad (administrator-equivalent) permissions | A-07, A-08 | `tracker@9124656:infra/lib/tracker-stack.ts:359-362` | Open |
| V-05 | The CI pipeline doesn't pin dependencies to a lockfile, pins GitHub Actions to version tags rather than commits, and has no dependency scanning | A-06, A-07 | `tracker@9124656:.github/workflows/deploy.yml:25-33` | Open |
| V-06 | Account-level audit logging and threat detection are unverified; app logs are kept 14 days; no API or CDN access logs | A-12 | System description 3.1; `tracker-stack.ts:190-193` | Open (unverified) |
| V-07 | The backup copy vault was created by hand outside infrastructure code; backups are not locked against deletion; no restore test is on record | A-02 | `tracker-stack.ts:306-316`; DEPLOY.md Part 6 | Open |
| V-08 | Recovery of administrator access after losing a device has not been tested | A-09, A-08 | Phase 0 interview | Open |
| V-09 | Record metadata is stored unencrypted | A-03 | `tracker@9124656:infra/lambda/api.mjs`; data inventory D-21 | By design |
| V-10 | Backup retention (up to 3 years) is set by a technical setting, not a retention decision, and is longer than a user would expect after deletion | A-02, A-14 | `tracker-stack.ts:322` | Open |
| V-11 | No privacy notice, terms, or log of user requests | A-14 | System description 2, 3.4 | Open (Phase 6) |
| V-12 | User sign-in uses the Cognito Lite plan (no compromised-credential or risk-based checks); password policy is length-only, 10 characters | A-04 | `tracker-stack.ts:132-141` | Open |
| V-13 | Invitations carry a temporary password by email; users may email themselves their recovery code | A-04, A-01 | `tracker-stack.ts:87-94`; data flow 9, 10 | By design |
| V-14 | Domain registration and DNS live in the same AWS account as the app and an unrelated personal site; registrar transfer lock unverified | A-10 | System description 3.1; D-007 | Open (unverified) |
| V-15 | Data passphrase strength is checked by length only (12 characters) | A-01 | `tracker@9124656:web/js/app.js:32` | Open |

### Predisposing conditions

| ID | Condition | Effect on risk |
|---|---|---|
| PC-01 | One operator does everything | No separation of duties or independent review; key-person dependency |
| PC-02 | The operator controls the code that handles users' passphrases | End-to-end encryption protects stored data from the operator, but not from a changed app |
| PC-03 | Most data is special-category, and some is about people without accounts | Raises the impact of any confidentiality breach |
| PC-04 | Users are friends and family | Informal communication; lower likelihood of complaints; higher personal cost if trust breaks |
| PC-05 | Near-zero budget | Favours free controls; paid tools need a clear risk case |
| PC-06 | The app works offline and keeps an encrypted copy on each device | **Lowers** the impact of outages and of some data loss |
| PC-07 | All user content is end-to-end encrypted | **Lowers** the impact of most server-side compromises to metadata and availability |
