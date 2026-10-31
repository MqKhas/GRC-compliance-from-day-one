# ISMS Scope Statement

| | |
|---|---|
| **Document owner** | Muhammad Khas |
| **Version** | 1.0 |
| **As of** | 2026-10-31 |
| **Standard** | ISO/IEC 27001:2022, clauses 4.1–4.3 |
| **Source** | Derived from the [system description](system-description.md) and decisions D-004 to D-009 |

> An ISMS (information security management system) is the set of policies, processes and controls an organization uses to manage information security. ISO 27001 certifies the ISMS, so the first thing it asks is: what does this ISMS cover? This document answers that.

## 1. Scope statement (clause 4.3)

> **The ISMS covers the operation, development and administration of Khas Tracker, an invite-only, end-to-end encrypted daily-life and fitness tracking web application, including its AWS cloud infrastructure, its source code repository and deployment pipeline, the administrator identities used to manage them, and the personal data of its users and of the people they record. It is operated by one person.**

### In scope

- The Khas Tracker web app (including the demo feature) and its sync API
- The AWS account that hosts it: the services in the system description, root and administrator access, billing, and account-level settings
- Domain registration and DNS for the app
- The private GitHub repository and the GitHub Actions deploy pipeline
- Administrator identity accounts that protect access to AWS and GitHub (Apple Account, Google account)
- All data in the [data inventory](data-inventory.md)
- The processes for account management, change and deployment, backup and restore, user support and requests

### Exclusions and why

ISO 27001 allows exclusions as long as they're justified and don't affect the ISMS's ability to protect the information in scope.

| Excluded | Justification |
|---|---|
| Personal website in the same AWS account | Static site with no user data (D-007). The shared AWS account, identities and DNS remain in scope, so the exclusion doesn't weaken protection of the tracker |
| Demo sample data | Fake, held only in browser memory, never stored or sent (D-004) |
| Users' own devices, browsers, mail and authenticator apps | Controlled by users, not by the operator. Their duties are listed as complementary user entity controls in the system description |
| Physical security of data centers | Provided by AWS. Relied on through AWS's own certifications (assessed in Phase 5) |

## 2. Context of the organization (clause 4.1)

Issues that affect the ability of the ISMS to achieve its intended outcomes.

### Internal issues

| Issue | Effect on the ISMS |
|---|---|
| One-person operation | No separation of duties and a key-person dependency. Compensating controls and documented risk acceptance are needed instead |
| No budget beyond a few dollars a month | Controls must be low-cost or free. Paid tools need a clear risk case |
| The app is end-to-end encrypted by design | User content is protected even from the operator. Shifts the main risks to metadata, administrator access, code integrity and availability |
| Fully defined as code (AWS CDK) | Most configuration can be verified from code and automated checks |
| No written policies or procedures before this program | The ISMS starts from scratch |

### External issues

| Issue | Effect on the ISMS |
|---|---|
| Users are friends and family who trust the operator personally | Informal communication is the norm, which makes formal notice and request handling easy to skip |
| Data includes health, mental health, religious practice and data about non-users | Raises the potential harm and the privacy law exposure, even at small scale |
| US state privacy and consumer health data laws are changing quickly | Applicability must be re-checked regularly (compliance triggers) |
| Dependence on AWS, GitHub, Google and Apple | Their security and availability directly affect the service |

## 3. Interested parties and their requirements (clause 4.2)

| Interested party | Relevant requirements | Addressed through the ISMS? |
|---|---|---|
| **Users** | Confidentiality of everything they log; their data is available when they need it; they can get their data deleted | Yes |
| **People recorded by users** (spouses, family, friends without accounts) | Information about them is not exposed | Yes |
| **The operator** | Keep the service low-cost and maintainable by one person; avoid legal exposure | Yes |
| **Vendors** (AWS, GitHub, Google, Apple) | Compliance with their terms of service and acceptable use policies | Yes |
| **Regulators** (FTC, state attorneys general) | Compliance with applicable privacy and consumer protection laws, analyzed in Phase 6 | Yes |
| **Future users who are minors, and their parents** | COPPA and state minor-privacy duties if accounts for minors are ever created (triggers T-01, T-02) | Yes, by trigger |

## 4. Interfaces and dependencies

Where the ISMS meets things it doesn't control:
- **AWS:** hosting, identity, storage, email, backups. Carved out.
- **GitHub:** source code and deployment. Carved out.
- **Google and Apple:** administrator identity and credential storage. The *accounts* are in scope. The *companies'* controls are carved out.
- **Users' mail providers:** receive invitation emails and, if a user chooses, their recovery code.

## 5. Review

The scope is reviewed at each quarterly review (Phase 9) and whenever a [compliance trigger](compliance-triggers.md) fires.
