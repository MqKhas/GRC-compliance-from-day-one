# Risk Register: Summary

| | |
|---|---|
| **Risk owner** | Muhammad Khas |
| **As of** | 2026-10-31, system version `tracker@9124656` |
| **Method** | [methodology.md](methodology.md) (NIST SP 800-30 Rev 1) |
| **Full register** | [risk-register.csv](risk-register.csv) (source of truth) and [risk-register.xlsx](risk-register.xlsx) (same data, filterable) |
| **Decisions** | D-013 to D-015 in [PROGRESS.md](../PROGRESS.md) |

This page is the short version. Every risk's threat event, ATT&CK techniques, assets, vulnerabilities, existing controls, scoring reasons and planned action are in the CSV.

## At a glance

19 risks. 16 to mitigate, 3 to accept. None is High or Very High today.

| Level | Inherent | Current | Target |
|---|---|---|---|
| Very High | 3 | 0 | 0 |
| High | 6 | 0 | 0 |
| Moderate | 6 | 6 | 1 |
| Low | 4 | 12 | 16 |
| Very Low | 0 | 1 | 2 |

**Inherent → current** shows what the existing controls already do. End-to-end encryption does most of the work: it is why no risk to user content is above Moderate today.
**Current → target** shows what the [treatment plan](risk-treatment-plan.md) is expected to achieve.

## Heat map: current risk

Rows are likelihood, columns are impact. Each cell's risk level comes from SP 800-30 Table I-2 (see [methodology](methodology.md#33-risk-level-sp-800-30-appendix-i-table-i-2)); the workbook's Scales sheet shows it in colour.

| Likelihood ↓ / Impact → | Very Low | Low | Moderate | High | Very High |
|---|---|---|---|---|---|
| **Very High** | | | | | |
| **High** | | | | | |
| **Moderate** | | R-10, R-14 | R-03, R-09 | R-06 | |
| **Low** | | R-16 | R-07 | R-11, R-12, R-13, R-19 | R-01, R-02, R-05 |
| **Very Low** | | | R-18 | R-08 | R-04, R-15, R-17 |

The right-hand column is the story of this system. The worst outcomes (someone reading users' data, or controlling production) are all unlikely today. They stay visible because Table I-2 weights impact.

## Top risks (current level Moderate)

| ID | Risk | Current | Treatment | Target |
|---|---|---|---|---|
| R-01 | Administrator identity or credential store compromised | Moderate | Mitigate | Low |
| R-02 | Malicious deploy through a compromised identity or code-host account | Moderate | Mitigate | Low |
| R-05 | Malicious dependency or Action runs in the deploy job | Moderate | Mitigate | Low |
| R-06 | Incident goes undetected or cannot be investigated | Moderate | Mitigate (free controls only) | **Moderate**, accepted with reason |
| R-03 | Unreviewed change breaks the app or corrupts data | Moderate | Mitigate | Low |
| R-09 | Record metadata shows when users log | Moderate | Mitigate | Low |

Four of the six are about **who can change the code users run, or the account it runs in**. With one operator and end-to-end encryption, that's where the real risk sits, not in the database.

## All risks

| ID | Risk | Current | Treatment | Target |
|---|---|---|---|---|
| R-01 | Administrator identity or credential store compromised | Moderate | Mitigate | Low |
| R-02 | Malicious deploy through a compromised identity or code-host account | Moderate | Mitigate | Low |
| R-03 | Unreviewed change breaks the app or corrupts data | Moderate | Mitigate | Low |
| R-04 | Operator ships a passphrase-capturing build | Low | Accept | Low |
| R-05 | Malicious dependency or Action runs in the deploy job | Moderate | Mitigate | Low |
| R-06 | Incident goes undetected or cannot be investigated | Moderate | Mitigate | Moderate |
| R-07 | User account taken over | Low | Mitigate | Very Low |
| R-08 | Passphrase cracked offline | Low | Mitigate | Low |
| R-09 | Record metadata shows when users log | Moderate | Mitigate | Low |
| R-10 | Deleted users remain in backups for years | Low | Mitigate | Low |
| R-11 | Privacy obligations not met | Low | Mitigate | Low |
| R-12 | User data permanently lost | Low | Mitigate | Low |
| R-13 | Operator unavailable for an extended period | Low | Mitigate | Low |
| R-14 | Outage or cost spike | Low | Accept | Low |
| R-15 | Domain or DNS hijacked, or domain lapses | Low | Mitigate | Low |
| R-16 | User mailbox exposes invitation password or recovery code | Low | Accept | Low |
| R-17 | Client-side code flaw exposes plaintext | Low | Mitigate | Low |
| R-18 | API authorization flaw between users | Very Low | Mitigate | Very Low |
| R-19 | Operator locked out of administration | Low | Mitigate | Low |

Several Low risks are mitigated anyway. Two reasons: the methodology's rule that free fixes are applied to any risk touching user-content confidentiality or code integrity (R-08, R-15, R-17), and cheap fixes that produce evidence for later phases (R-18).

## MITRE ATT&CK techniques referenced

| Technique | Name | Risks |
|---|---|---|
| T1056.003 | Input Capture: Web Portal Capture | R-02, R-04, R-15 |
| T1059.007 | Command and Scripting Interpreter: JavaScript | R-17 |
| T1078 / T1078.004 | Valid Accounts / Cloud Accounts | R-01, R-02, R-05, R-07 |
| T1110.002 | Brute Force: Password Cracking | R-08 |
| T1111 | Multi-Factor Authentication Interception | R-07 |
| T1114 | Email Collection | R-16 |
| T1189 | Drive-by Compromise | R-17 |
| T1190 | Exploit Public-Facing Application | R-18 |
| T1195.001 / T1195.002 | Supply Chain Compromise: dependencies and tools / software supply chain | R-05, R-02 |
| T1485 | Data Destruction | R-01, R-07 |
| T1496 | Resource Hijacking | R-01 |
| T1499 | Endpoint Denial of Service | R-14 |
| T1530 | Data from Cloud Storage | R-09 |
| T1555 | Credentials from Password Stores | R-01 |
| T1562.008 | Impair Defenses: Disable or Modify Cloud Logs | R-06 |
| T1566 / T1566.002 | Phishing / Spearphishing Link | R-01, R-07 |
| T1584.001 | Compromise Infrastructure: Domains | R-15 |

Risks caused by accidents, faults or legal duties (R-03, R-10 to R-13, R-19) have no ATT&CK mapping, since ATT&CK only describes adversary behaviour.
