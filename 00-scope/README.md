# Phase 0: Scope & System Description

What is being assessed, where its edges are, and what data it holds. Every later phase builds on this.

| Artifact | What it answers |
|---|---|
| [system-description.md](system-description.md) | What the system is, what it promises users, its components, boundaries, vendors and known limitations (modeled on SOC 2 Section III, DC 200) |
| [boundary-diagram.md](boundary-diagram.md) | What's in and out of scope, and the two paths that reach production |
| [data-flow.md](data-flow.md) | Where data goes, in what form, and across which trust boundaries |
| [data-inventory.md](data-inventory.md) / [.csv](data-inventory.csv) | Every kind of data, its classification and sensitivity, where it lives, who can read it, how long it's kept |
| [compliance-triggers.md](compliance-triggers.md) | Future events that would change what rules apply |

Scoping decisions: D-004, D-006, D-007, D-008 in [PROGRESS.md](../PROGRESS.md).
