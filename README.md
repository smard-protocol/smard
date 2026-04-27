# SMARD — Structured Communication Protocol

**The open standard for structured communication capsules.**  
Not email. Not chat. Something better.

→ [smardapp.com](https://smardapp.com) · Apache 2.0 · Status: Active Development

---

## What is SMARD?

A **Smard** is a structured communication capsule — a self-contained unit that groups everything related to a specific topic: title, body, participants, messages, files, tasks, and history.

If WhatsApp is a hallway conversation and email is a formal letter, a Smard is **a room convened for something specific.** It has a name. It has participants. It has context. And when it's done, it stays archived — never lost.

```json
{
  "smard_id": "a7f3k9m2p8q1w5x6y0z3b4c8",
  "title": "Bathroom renovation — Marcos quote",
  "body": "Marcos, photos attached. We need this done before May 15th.",
  "type": "inbox",
  "status": "open",
  "author": "@cata",
  "participants": ["@cata", "@marcos"],
  "messages": [
    {
      "author": "@marcos",
      "text": "Got it. Sending the quote this afternoon.",
      "sparks": [{ "author": "@cata", "content": "👍" }]
    }
  ],
  "attachments": [
    { "type": "image", "name": "bathroom_current.jpg" }
  ],
  "tasks": [
    { "text": "Confirm final price", "done": false }
  ]
}


Why SMARD?
Every important conversation ends up lost.
Your renovation budget is buried in 400 WhatsApp messages.The email thread has 12 replies and nobody knows the final decision.The meeting notes live in someone’s head.
SMARD fixes this with one idea: every conversation that deserves context lives in its own capsule. Forever. Searchable. Structured. Shared only with who needs it.

Built for today. Designed for what’s next.
SMARD is a communication protocol for humans and for AI agents.
The JSON structure of a Smard is natively readable by any LLM without additional instructions. Every field — title, body, participants, messages, tasks — provides semantic context that agents can act on.
When an AI agent buys something on your behalf, negotiates with a supplier, or coordinates a delivery — all of it can be recorded in a Smard. The user sees a clean conversation. The agent has structured context. Nothing gets lost.
SmardApp is the reference client — clean, open-source, no AI, forever.AI-powered applications read and write Smards without touching the base protocol.The protocol has no opinion about what runs on top of it.

The Capsule — Four Types



|Type         |Description                                                                  |
|-------------|-----------------------------------------------------------------------------|
|**Inbox**    |Communication between two or more participants on a topic                    |
|**Note**     |A Smard with no recipients — private, yours alone. The seed of a conversation|
|**Scheduled**|Any Smard with a date and time — becomes a reminder or event                 |
|**Wallet**   |Documents of value: invoices, tickets, contracts, boarding passes            |

Core Fields



|Field            |Required|Description                                                          |
|-----------------|--------|---------------------------------------------------------------------|
|`smard_id`       |✅       |Unique 24-char identifier, immutable                                 |
|`title`          |✅       |Topic of the capsule, max 80 chars. Auto-generated from body if empty|
|`body`           |✅       |Main content, max 3000 chars, basic rich text                        |
|`type`           |✅       |`inbox` / `note` / `scheduled` / `wallet`                            |
|`status`         |✅       |`open` / `archived` — nothing is ever deleted                        |
|`author`         |✅       |`@handle` of creator                                                 |
|`participants`   |✅       |Flat list of `@handles` — no roles, no hierarchy                     |
|`messages`       |—       |Threaded replies within the capsule, max 500 chars each              |
|`sparks`         |—       |Micro-reactions, max 16 chars — emoji, text, or both                 |
|`attachments`    |—       |Referenced files: image, document, audio, link                       |
|`tasks`          |—       |Checkable items inside the capsule, visible in global Tasks view     |
|`scheduled_at`   |—       |ISO 8601 datetime for Scheduled type                                 |
|`wallet_category`|—       |`invoice` / `ticket` / `contract` / `receipt` / `boarding_pass`      |

Full specification: SPEC.md (coming soon)

Design Principles
P1 — Structure over speed. Every Smard has a title. Context is never optional.P2 — Equal participants. No admins. No roles. If you received it, you participate equally.P3 — Self-contained. A Smard must be fully understandable without external context.P4 — Permanent by default. Nothing disappears. Archive, never delete.P5 — Agent-ready. Structured for AI to read, write, and act — without modifying the protocol.P6 — Privacy by design. Data belongs to participants. No data mining. Ever.

SmardApp — Reference Client
SmardApp is the open-source reference implementation of the SMARD protocol.
What it is: A minimalist messaging app where every conversation is a structured capsule.What it is not: A project management tool, a SaaS platform, or an AI assistant.
Core interface:
	•	Inbox — active Smards, ordered by latest activity
	•	Scheduled — Smards with dates and times
	•	Wallet — documents of value
	•	Notes — private Smards, yours alone
	•	Projects — group Smards in lists or simple kanban boards
	•	Tasks — global view of all tasks across all your Smards
Status: **Status:** Spec complete. 
Reference prototype live: https://direct-smard.lovable.app/


Roadmap



|Phase             |Timeline  |Milestone                                         |
|------------------|----------|--------------------------------------------------|
|✅ Spec v1.0       |April 2026|Capsule specification complete                    |
|🔄 GitHub & Web    |April 2026|Public repository and landing page live           |
|⏳ SPEC.md         |May 2026  |Full protocol specification published             |
|⏳ Reference client|Q2 2026   |SmardApp v0.1 functional and open                 |
|⏳ Community launch|Q2 2026   |Hacker News, first contributors                   |
|⏳ Protocol v1.1   |Q3 2026   |Federated identity, first external implementations|

Contributing
SMARD is in its earliest phase. The best contributions right now are:
	•	Discussion — open an Issue to challenge, improve, or extend the spec
	•	Implementation — build a compatible SMARD client in any stack
	•	Design — help define the SmardApp reference interface
	•	Documentation — improve the spec, add examples, write guides
Read CONTRIBUTING.md before submitting anything. (coming soon)

License
Apache 2.0 — see LICENSE
Build on it. Extend it. Use it commercially.The only requirement: keep the protocol open.

SMARD Protocol — Open Standard for Structured Communicationsmardapp.com · github.com/smard-protocol/smard

​​​​​​​​
