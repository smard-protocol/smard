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
  "author": "@cato",
  "participants": ["@cato", "@marcos"],
  "messages": [
    {
      "author": "@marcos",
      "text": "Got it. Sending the quote this afternoon.",
      "sparks": [{ "author": "@cato", "content": "👍" }]
    }
  ],
  "attachments": [
    { "type": "image", "name": "bathroom_current.jpg" }
  ],
  "tasks": [
    { "text": "Confirm final price", "done": false }
  ]
}
