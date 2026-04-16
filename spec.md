
# SMARD Capsule Specification v1.0

**Status:** Stable — basis for reference implementation  
**License:** Apache 2.0  
**Repository:** github.com/smard-protocol/smard

---

## 1. Overview

This document is the canonical specification of the SMARD protocol.  
It defines what a capsule is, what fields it contains, how it behaves across its lifecycle, and what any client must implement to be considered SMARD-compatible.

This is not an engineering document. It is the conceptual contract that precedes any implementation. A developer should be able to read this and build a compatible client without asking any questions.

### The core idea

A **Smard** is a structured communication capsule — a self-contained unit that groups everything related to a specific topic: title, participants, content, messages, files, and tasks.

> If WhatsApp is a hallway conversation and email is a formal letter,  
> a Smard is a room convened for something specific.  
> It has a name. It has context. And when it ends, it stays — never lost.

---

## 2. Design Principles

These principles resolve any conflict in protocol decisions.  
If a proposed feature violates them, it does not belong in the base protocol.

**P1 — Structure over speed**  
Every Smard has a title. Context is never optional. The title can be auto-generated from content, but it can never be absent.

**P2 — Equal participants**  
No admins. No roles. No hierarchy. If you received a Smard, you participate exactly as the person who created it.

**P3 — Self-contained**  
Any person — or agent — receiving a Smard must understand its purpose without external context. A Smard is complete in itself.

**P4 — Permanent by default**  
Nothing in a Smard disappears. Archive, never delete. What was said stays said.

**P5 — Agent-ready**  
The structure of a Smard must be interpretable by AI systems without additional instructions. The protocol contains no AI — but it is designed to be the natural container where AI operates and records.

**P6 — Privacy by design**  
Data belongs to participants. The protocol contains no mechanism for data exploitation. Any implementation declaring SMARD compatibility while mining user data without explicit consent violates the spirit of this license.

---

## 3. Capsule Types

All Smards share the same base structure. Types are **classifications** — they determine how a client presents and organizes the capsule. A Smard can change type if its content or metadata changes.

| Type | Description |
|------|-------------|
| `inbox` | Communication between two or more participants on a specific topic |
| `note` | A Smard with no external recipients — private, belongs only to the author |
| `scheduled` | Any Smard with an associated date and time |
| `wallet` | A Smard classified as a document of value |

**Type rules:**
- Default type is `inbox` if external participants exist
- Default type is `note` if no external participants exist
- A `note` becomes `inbox` when a participant is added
- Type can be changed manually or suggested automatically by the client

---

## 4. Field Specification

### 4.1 System fields
*Auto-generated at creation. Immutable.*

**`smard_id`**  
Universal unique identifier. 24 alphanumeric characters. Generated at creation. No two Smards anywhere in any system may share this identifier.

**`created_at`**  
Creation timestamp. ISO 8601 UTC. Immutable.

**`updated_at`**  
Last activity timestamp. Updated on every new message, attachment, or status change.

---

### 4.2 Required content fields

**`title`**  
*Required. Always present.*

The topic of the capsule. Plain text. Maximum 80 characters.

If the user creates a Smard without a title, the client extracts the first meaningful words from the body and proposes them as the title. The user accepts with one tap or edits. The field is never empty — this rule is part of the protocol, not the UX.

**`body`**  
Main content. Basic rich text: bold, italic, lists, line breaks. No block editors. No columns.  
Maximum 3,000 characters. May be empty only if at least one attachment exists.

**`type`**  
`inbox` / `note` / `scheduled` / `wallet`  
See Section 3 for rules.

**`status`**  
`open` / `archived`

`open` — active, appears in the main inbox  
`archived` — read-only, accepts no new messages or attachments. Always accessible. Never deleted. Can be re-opened.

**`author`**  
`@handle` of the capsule creator. Cannot be modified after creation.

**`participants`**  
Flat array of `@handles`. No roles. No hierarchy.

```json
"participants": ["@alice", "@bob", "@carol"]


Minimum for inbox: 2 participants (author + at least one more)For note: author onlyMaximum: 50 participants in v1.0
The author may add participants. Any participant may suggest adding someone — no formal approval required. No vetoes. Social simplicity is a design decision.

4.3 Optional fields
scheduled_atFor scheduled type only.ISO 8601 datetime. When this moment arrives, the client triggers a notification and visually marks the Smard as “today”. The client may detect natural language dates in the title or body and suggest this field automatically.
wallet_categoryFor wallet type only.Suggested values (not enforced): invoice / ticket / contract / receipt / boarding_pass / warranty / otherThe client may suggest automatically based on attachment patterns. User confirms.
messagesThreaded replies within the capsule. All replies on a topic live here — no parallel threads.

"messages": [
  {
    "id": "msg_001",
    "author": "@bob",
    "text": "Got it. Sending the quote this afternoon.",
    "created_at": "2026-04-15T10:15:00Z",
    "whisper_to": null,
    "sparks": [
      {
        "author": "@alice",
        "content": "👍",
        "created_at": "2026-04-15T10:18:00Z",
        "target_id": "msg_001"
      }
    ]
  }
]


Each message: maximum 500 characters.
Whisper rule:If whisper_to contains a @handle, the message is a whisper — visible only to author and recipient. Other participants see a neutral indicator that a whisper exists. Its existence is public. Its content is private.
sparksMicro-reactions. Maximum 16 characters. Emoji, text, or combination.

{
  "author": "@alice",
  "content": "👍",
  "created_at": "2026-04-15T10:18:00Z",
  "target_id": "msg_001"
}


Valid examples: 👍 done ✅ confirmed on my way can't make it 🔥
Sparks do not open conversation. They are signals. Displayed grouped by content with a counter: 👍 × 3
attachmentsFiles or resources referenced by the capsule. The protocol defines how attachments are described — not where they are stored. Each implementation decides storage.

"attachments": [
  {
    "id": "att_001",
    "type": "image",
    "name": "bathroom_current.jpg",
    "ref": "https://storage.example.com/att_001",
    "added_by": "@alice",
    "added_at": "2026-04-15T09:00:00Z"
  }
]


Valid types: image / document / audio / link
All attachments are accessible from a dedicated area at the top of the open capsule — not buried in the message thread.
tasksCheckable items inside the capsule. Visible both within the Smard and in the client’s global Tasks view.

"tasks": [
  {
    "id": "task_001",
    "text": "Confirm final price",
    "done": false,
    "due_date": null,
    "created_by": "@alice",
    "created_at": "2026-04-15T09:00:00Z"
  }
]


Maximum task text: 200 characters. due_date is optional, ISO 8601.
project_refsArray of project IDs this Smard belongs to. A Smard may belong to multiple projects simultaneously. Projects are separate entities — the Smard stores only references.

"project_refs": ["proj_bathroom_reno", "proj_house_2026"]


5. Canonical JSON Structure
A complete Smard:

{
  "smard_id": "a7f3k9m2p8q1w5x6y0z3b4c8",
  "title": "Bathroom renovation — Marcos quote",
  "body": "Marcos, photos attached. We need this done before May 15th.",
  "type": "inbox",
  "status": "open",
  "author": "@alice",
  "created_at": "2026-04-15T09:00:00Z",
  "updated_at": "2026-04-15T14:32:00Z",
  "scheduled_at": null,
  "wallet_category": null,
  "participants": ["@alice", "@marcos"],
  "messages": [
    {
      "id": "msg_001",
      "author": "@marcos",
      "text": "Got it. Sending the quote this afternoon.",
      "created_at": "2026-04-15T10:15:00Z",
      "whisper_to": null,
      "sparks": [
        {
          "author": "@alice",
          "content": "👍",
          "created_at": "2026-04-15T10:18:00Z",
          "target_id": "msg_001"
        }
      ]
    }
  ],
  "attachments": [
    {
      "id": "att_001",
      "type": "image",
      "name": "bathroom_current.jpg",
      "ref": "https://storage.example.com/att_001",
      "added_by": "@alice",
      "added_at": "2026-04-15T09:00:00Z"
    }
  ],
  "tasks": [
    {
      "id": "task_001",
      "text": "Confirm final price",
      "done": false,
      "due_date": null,
      "created_by": "@alice",
      "created_at": "2026-04-15T09:00:00Z"
    }
  ],
  "project_refs": []
}


6. Capsule Lifecycle

CREATION
    ↓
[open] → visible in inbox, accepts messages and attachments
    ↓
[archived] → read-only, always accessible, never deleted
    ↑
[open] ← can be re-opened if needed


Rules:
	•	Every Smard is born open
	•	Only the author can archive in v1.0
	•	Archiving is not deleting — it is closing
	•	Permanent deletion does not exist in the base protocol
	•	Status change history exists in the system but is not exposed in the UI by default

7. Identity
Handle: @username — 3 to 30 alphanumeric characters. No spaces. No visible phone number.
Users register with email or phone for verification. They communicate exclusively via @handle. The handle belongs to the user — not the platform. Any compatible implementation must allow profile and Smard history export.
Federated identity (proposed for v1.1): format @handle@domain for cross-instance communication, analogous to ActivityPub. Not implemented in v1.0 — documented for forward compatibility.

8. Compatibility Requirements
A client is SMARD v1.0 compatible if it:
	1.	Implements all required fields with their exact names and types
	2.	Respects all character limits defined in this spec
	3.	Implements all four capsule types
	4.	Implements the lifecycle with exactly two states: open and archived
	5.	Does not error on unrecognized optional fields — silently ignores them
	6.	Allows export of any Smard in the canonical JSON format defined in Section 5
	7.	Does not store or process user data for exploitation without explicit consent
What the protocol does not dictate:Visual design, technology stack, business model, file storage location, notification system, and any AI functionality.

9. What SMARD Is Not
These are permanent decisions — not pending features.
No roles or permissions. If you participate, you participate equally. Hierarchy is not the protocol’s responsibility.
No workflows or automations in the core. Automations live in ecosystem applications, not in the base protocol.
No AI in the protocol. AI is built on top. The protocol is the ground — it must be solid and simple, not intelligent.
No ephemeral messages. What is said in a Smard stays. For ephemeral, use WhatsApp. SMARD is for what must endure.
No permanent deletion. Archive is the final state.

10. The Agent Layer
This section is architecture, not speculation.
A Smard’s JSON is processable by any LLM without additional instructions. The fields title, body, type, participants, messages, and tasks are sufficient for an AI agent to understand what this Smard is, what happened, and what might need to happen next.
This means SMARD is not only the communication format between humans today. It is the natural record of communication between agents tomorrow.
When a personal agent manages a purchase, negotiates with a supplier, or coordinates a delivery — the entire interaction fits in a Smard. The user did not configure anything. The agent created the Smard, updated it, and archived it when done.
The protocol requires no changes for this. It is already prepared. This is the direct consequence of P3 (self-contained) and P5 (agent-ready).

11. Versioning
SMARD uses MAJOR.MINOR versioning.
MINOR changes (1.0 → 1.1): new optional fields, clarifications, type extensions. Backward compatible.
MAJOR changes (1.x → 2.0): changes to required fields or lifecycle. Require declared migration.
Roadmap target: reach SMARD 2.0 with federated identity implemented, at least three independent compatible clients, and the proprietary application ecosystem operational.

SMARD Protocol — Capsule Specification v1.0Apache 2.0 Licensegithub.com/smard-protocol/smard
