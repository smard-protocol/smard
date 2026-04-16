# Contributing to SMARD

Thanks for your interest in building the new standard for structured communication.

SMARD is in its earliest phase. This is the best moment to contribute — the protocol is not set in stone, the reference client hasn't been built yet, and the decisions made now will define the standard for years.

---

## What we need right now

### 1. Spec discussion
Read [SPEC.md](./SPEC.md) and open an Issue if you want to:
- Challenge a design decision
- Propose a new optional field
- Suggest an edge case we haven't considered
- Question a compatibility requirement

The spec is stable but not final. Good arguments change things.

### 2. Reference client
SmardApp is the open-source reference implementation of SMARD.
It needs to be built. We are looking for developers who want to own parts of it.

Current status: design phase.
Stack: to be decided with the community — open an Issue titled `[Stack] Your proposal` to start the conversation.

Scope of v0.1 reference client:
- Create, send, and receive Smards
- Four capsule types: inbox, note, scheduled, wallet
- Messages, sparks, whispers, attachments, tasks
- Six views: Inbox, Scheduled, Wallet, Notes, Projects, Tasks
- @handle identity, no phone number visible
- Export any Smard as canonical JSON
- No AI. No monetization. No tracking.

### 3. Design

The visual design system for SmardApp is defined by the core team and is not open for alternative proposals.

The reference design will be published as a public Figma file.
Developers implementing compatible clients are welcome to use it as reference
or create their own visual language — the protocol does not dictate UI.

If you find inconsistencies between the design spec and the protocol spec, open an Issue.


### 4. Documentation
- Improve the spec with better examples
- Write implementation guides for specific stacks
- Translate the spec (after v1.0 is finalized)

---

## How to contribute

1. **Open an Issue first** — before writing any code or design, open an Issue describing what you want to do and why. This avoids duplicated effort and misaligned work.

2. **Fork the repository** — make your changes in your own fork.

3. **Open a Pull Request** — with a clear description of what changed and why. Reference the Issue it addresses.

4. **Keep it minimal** — SMARD's core value is simplicity. If a contribution adds complexity without a very strong reason, it won't be merged into the base protocol.

---

## What we will not merge

- AI functionality in the base protocol
- Role-based permissions or admin features
- Ephemeral messages or deletion mechanisms
- Anything that breaks backward compatibility with v1.0 without a MAJOR version bump
- Proprietary dependencies

---

## Principles that guide every decision

**P1 — Structure over speed**
**P2 — Equal participants**
**P3 — Self-contained**
**P4 — Permanent by default**
**P5 — Agent-ready**
**P6 — Privacy by design**

If your contribution aligns with these, it belongs here.

---

## Communication

- **Issues** — for spec discussion, proposals, and bug reports
- **Pull Requests** — for concrete changes with working implementations
- **Discussions** — for open-ended questions and ideas (enable in repo settings)

---

## Code of Conduct

Be direct. Be respectful. Argue with ideas, not people.
We are building infrastructure. The work is what matters.

---

*SMARD Protocol — github.com/smard-protocol/smard*
​​​​​​​​​​​​​​​​
