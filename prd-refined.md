# PRD — "Closer": AI Co-Agent for Real-Estate Buyers

**Event:** OpenAI Workspace Agents Hackathon · The AI Collective · SF · May 30, 2026
**Tracks:** Technical (Codex + Agents SDK), with a no-code Workspace Agent fallback
**Status:** v3 (simplified, scope locked)

## Mission

**Empower real-estate agents to find their customers' dream house, more effectively.**

## What it is

An always-on AI co-agent that helps a real-estate agent manage their customers, analyze what each one wants, and lift the rate at which a pool of customers turns into a signed offer — and a commission.

**The AI's job, in a loop:** summarize each customer's requirements → find matching properties → recommend them to the customer. The human agent stays in control and steps in to close.

## Scope — three pillars

**Lead Management + Intention Analysis exist to do one thing: double the agent's conversion rate from ~5% to 10%.** Most leads die from slow follow-up and shallow understanding; nailing both is what turns a browser into a buyer.

1. **Lead Management** — keep every existing buyer organized in one pipeline with status and notes, and never let one go cold. *(We do NOT source or generate leads — the agent brings their own.)* The co-agent actively works each customer:
   - **Greeting & check-ins** — auto-welcome a new buyer and send periodic, personalized nudges so no one goes cold.
   - **Email / reply drafting** — draft (or send, on approval) responses to customer messages in the agent's voice.
   - **Proactive recommendations** — push new matching listings to the customer as they hit the market.
   - **Requirement tracking** — detect when a buyer's needs shift in conversation, update their profile, and flag the agent.
   - **Follow-up reminders** — surface stale or due-for-contact buyers so the agent acts before the lead drifts.
2. **Intention Analysis** — understand each buyer's real needs and how serious they are: home type, area, price range, square footage, beds/baths, cash vs. financed, timeline, family situation, and an intent score. The picture **updates as the buyer's needs change.** Deeper, current understanding is the lever that lifts conversion.
3. **Matching** — for each buyer, find and rank the best-fit homes, each with a short, grounded reason ("under budget, has the yard you wanted, 8 min to that school").

**Out of scope:** lead sourcing, offer drafting, escrow, deposits/deadlines, e-signature, MLS write access. The human takes over once a buyer is qualified and matched.

## Target user

Independent and small-team residential agents who are their own ops department, and the buyers they serve (who get instant, relevant matches).

## Success metrics

- **North star: conversion rate ~5% → 10%** — the share of a customer pool that goes all the way to a signed offer (and the agent's commission), driven by faster, never-cold follow-up (lead management) and deeper, current understanding of intent (intention analysis).
- A complete buyer profile built from a chat with no manual data entry.
- Profile updates correctly when the buyer changes their mind, and matches re-run.
- 3 ranked, justified home matches per buyer.
- Supporting: qualified-and-matched buyers one agent can manage at once (≈5 → 25+).

## Architecture (Agents SDK + Responses API)

Two agents under a thin router:

- **Intent agent** — chats with the buyer, emits a structured profile (via structured outputs), keeps it current, and handles outreach. Tools: `update_buyer`, `get_buyer`, `set_status`, `draft_message` (email/DM in the agent's voice), `schedule_followup`.
- **Match agent** — given a buyer profile, searches listings, ranks the top 3, and writes a grounded rationale. Tools: `search_listings`, `get_listing_details`.

Store: a simple pipeline (Airtable / Sheets / SQLite). Listings: seeded dataset for a reliable demo, with live web search as an optional toggle. Use Codex to scaffold fast.

## Demo (~4 min)

1. A buyer DMs; they appear in the pipeline.
2. The agent asks a few sharp questions — the **profile fills in live**, status flips to qualified.
3. Buyer says "actually we could go to 1.4" → **profile updates, matches re-run.**
4. Agent returns **3 ranked homes, each with a one-line reason.**
5. End on the pipeline view: several buyers, all understood and matched, one human in control.

## Risks

- Live listings unreliable on stage → seed a dataset, web search optional.
- Made-up listing facts → ground every reason in tool data, show the source.
- Scope creep → locked to the three pillars above.

## Why it wins (for EF judges)

~3M US agents already pay for CRMs yet still qualify and search by hand. We own the two highest-frequency, license-safe tasks — understanding the buyer and finding the home — and expand into offers/transactions later. Agents are paid on closings, so letting one carry 5x the buyers is direct ROI.

---
*Product name "Closer" is a placeholder. Built for the OpenAI Workspace Agents Hackathon.*