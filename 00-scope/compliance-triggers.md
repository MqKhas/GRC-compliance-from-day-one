# Compliance Triggers

Events that would change what laws, frameworks or controls apply to Khas Tracker. Today none of these are true. When one becomes true, the listed review happens **before** the change goes live.

This is checked at every quarterly review (Phase 9), and `accounts.sh create` is the main moment where a trigger can fire.

| ID | Trigger | Status | Why it matters | Review required before it happens |
|---|---|---|---|---|
| T-01 | An account is made for someone **under 13** | Not met (2026-10-28) | COPPA applies to collecting personal information *from* children under 13 with actual knowledge. Needs verifiable parental consent, a direct notice to parents, and parental access and deletion rights | Parent consent recorded; parent-facing notice; data minimization review; confirm whether a child-specific account setup is needed; update privacy notice and data inventory |
| T-02 | An account is made for someone **13–17** | Not met (2026-10-28) | Several US state laws add duties for minors' data (for example, limits on targeted processing and stricter defaults). Health, mood and religious data make this more sensitive | Re-run the applicable-law analysis (Phase 6); parent awareness; review default section settings |
| T-03 | A user lives in, or data is processed in, **Washington State** (or another state with a consumer health data law) | To confirm in Phase 6 | Consumer health data laws, such as Washington's My Health My Data Act, can apply regardless of company size | Phase 6 applicability analysis; consent and notice requirements |
| T-04 | Someone who is **not a friend or family member** gets an account | Not met (2026-10-28) | Informal support and no written terms stop being reasonable once users don't know the admin personally | Privacy notice and terms in place; formal support and request channel |
| T-05 | Any **payment**, ad or monetization is added | Not met (2026-10-28) | Turns a personal project into a business: consumer protection, tax, payment card rules and many privacy law thresholds | Full scope re-review |
| T-06 | A **second person** gets admin access to AWS or GitHub | Not met (2026-10-28) | Changes access control, separation of duties and onboarding/offboarding | Access review process; role-based access; offboarding procedure |
| T-07 | Any feature that can read **unencrypted** user data server-side (search, AI, analytics) | Not met (2026-10-28) | Breaks the end-to-end encryption assumption that much of the risk and privacy analysis depends on | Full risk and privacy re-assessment |

Decisions behind this list: D-005 (minors), D-006 (non-user data) in [PROGRESS.md](../PROGRESS.md).
