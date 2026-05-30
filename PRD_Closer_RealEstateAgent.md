# PRD: Closer - AI Real-Estate Lead-to-Offer Agent

**Product owner:** Stanley Mei  
**Event:** OpenAI Workspace Agents Hackathon, The AI Collective, San Francisco, May 30, 2026  
**Build window:** 2 hours  
**Status:** Hackathon MVP PRD  
**One-liner:** Closer turns a messy buyer lead into an offer-ready packet for a licensed real-estate agent to review.

## 1. Product Focus

The original vision is an end-to-end AI real-estate deal agent. For a 2-hour hackathon, the winning slice is smaller and sharper:

**Lead message -> qualified buyer profile -> ranked listings -> offer packet draft.**

The product should feel like an always-on assistant for a solo real-estate agent, but the demo should avoid legal, MLS, payment, e-signature, and live API risk. Closer does not act as a broker. It prepares grounded recommendations and draft paperwork for human review.

## 2. Problem

Solo and small-team real-estate agents lose time on repetitive, high-context coordination work:

- **Lead leakage:** Inbound leads arrive from DMs, Zillow/Redfin-style portals, referrals, and texts. Slow response hurts conversion.
- **Manual qualification:** Agents repeatedly ask for budget, area, beds, baths, square footage, financing, timeline, household needs, and intent.
- **Listing grind:** Matching buyers to homes and explaining "why this fits" is slow and repetitive.
- **Offer prep:** Turning buyer preferences and listing facts into a clean offer summary takes time and must stay human-reviewed.

The result: agents can only manage a small number of active buyers before quality drops.

## 3. Target User

**Primary user:** A solo residential real-estate agent who is handling leads, follow-up, matching, and paperwork without an operations team.

**End beneficiary:** A home buyer who receives faster, more personalized responses.

## 4. 2-Hour MVP

Build one polished vertical slice. Everything below should be demoable without external services.

### F1 - Lead Intake

- User pastes or types a buyer message.
- App creates a lead record with `name`, `channel`, `status`, and original message.
- Initial status is `new`, then moves to `qualifying`.

### F2 - Buyer Qualification

- AI extracts and incrementally updates a structured buyer profile.
- The app shows the profile live as fields fill in.
- Required schema:
  - `areas`
  - `price_min`
  - `price_max`
  - `home_type`
  - `beds`
  - `baths`
  - `min_sqft`
  - `financing`
  - `timeline`
  - `household`
  - `must_haves`
  - `dealbreakers`
  - `intent_score`
  - `missing_info`
- If key fields are missing, the AI asks 2 or 3 focused follow-up questions.
- Once enough fields are present, status moves to `qualified`.

### F3 - Seeded Listing Match

- Use a fixed local listing dataset for demo reliability.
- Rank 3 listings against the buyer profile.
- Each recommendation must include:
  - Match score
  - 2 or 3 reasons grounded in listing data
  - Any caveat, such as over budget or missing square footage
  - Source/listing id

### F4 - Offer-Ready Packet

- User selects one listing.
- App generates a draft offer packet for agent review.
- Packet includes:
  - Buyer summary
  - Selected listing summary
  - Suggested offer price
  - Financing summary
  - Contingencies
  - Proposed close date
  - Missing items the agent must confirm
- Every generated packet must be labeled: **Draft only - licensed agent review required.**

## 5. Explicitly Out of Scope

These are not part of the 2-hour MVP:

- Live MLS, Zillow, Redfin, or paid listing API integrations
- Sending offers to sellers or seller agents
- Real signatures, e-signature, escrow, payments, or legal execution
- Multi-agent brokerage admin
- Real Slack/email automation
- Fully autonomous transaction management

Stretch features can be faked as a preview card if the core flow is complete.

## 6. Demo Script

Target demo length: 3 to 5 minutes.

1. Start with an empty CRM.
2. Paste a buyer message:

   > Hi, looking for a place in the South Bay, budget around 1.2M. We have a toddler and care about schools.

3. Closer creates a lead and extracts an initial buyer profile.
4. Closer asks 2 focused follow-ups, such as financing and bedroom needs.
5. The buyer profile updates live and CRM status changes to `qualified`.
6. Closer ranks 3 seeded listings with grounded explanations.
7. User selects a listing.
8. Closer generates an offer-ready packet marked for agent review.
9. End with the value statement:

   > Closer lets one agent turn more leads into offer-ready buyers without losing the human-in-the-loop control required in real estate.

## 7. Success Criteria

The demo is successful if the audience can clearly see:

- A raw lead becoming a CRM record
- A messy conversation becoming structured buyer data
- Missing information being detected and asked for
- Listings ranked using buyer preferences
- Recommendations grounded in listing data, not hallucinated
- A review-ready offer packet generated from the chosen listing

## 8. Technical Architecture

Recommended implementation for the hackathon:

- **Frontend:** Simple dashboard with four panes:
  - Lead inbox / CRM
  - Buyer chat
  - Structured profile
  - Listing matches / offer packet
- **Backend:** Lightweight local API or server actions.
- **Data store:** Local JSON or SQLite.
- **AI workflow:**
  - Qualification step uses structured outputs for the buyer profile.
  - Matching step reads from seeded listing JSON.
  - Drafting step generates the offer packet from profile + selected listing.
- **Tools/functions:**
  - `create_lead`
  - `update_buyer_profile`
  - `set_lead_status`
  - `search_seeded_listings`
  - `rank_listings`
  - `generate_offer_packet`

Keep the implementation deterministic. The "magic" should be the workflow and structured state changes, not external integrations.

## 9. Seed Data Requirement

Include 5 to 8 sample listings in the repo. Each listing should have:

- `id`
- `address`
- `city`
- `neighborhood`
- `price`
- `beds`
- `baths`
- `sqft`
- `home_type`
- `school_notes`
- `commute_notes`
- `features`
- `caveats`
- `source_url` or placeholder source

Seed at least 3 South Bay listings near the buyer's stated budget so the demo produces useful matches.

## 10. Risk Mitigation

| Risk | Mitigation |
| --- | --- |
| External listing data fails | Use seeded listings only for the core demo |
| AI invents listing facts | Only allow rationale based on listing fields |
| Demo takes too long | Use one prepared buyer scenario |
| Legal concern from judges | Say "draft for licensed agent review" everywhere |
| Scope creep | Build F1-F4 only before stretch work |
| Structured output is incomplete | Show `missing_info` and ask follow-up questions |

## 11. Fallback Demo Mode

If the AI call fails during the live demo:

- Use a prefilled buyer profile from the sample lead.
- Continue to listing ranking and packet generation.
- Explain that the same schema is used for live extraction when the model is available.

If ranking fails:

- Show precomputed top 3 listing matches from the seed data.

If packet generation fails:

- Show a static packet template populated from the selected listing.

## 12. Build Plan for 2 Hours

### First 20 minutes

- Create project skeleton.
- Add seed listings.
- Add buyer profile schema.
- Create basic dashboard layout.

### Minutes 20-50

- Implement lead intake.
- Implement AI profile extraction or mock-compatible schema flow.
- Show live profile state.

### Minutes 50-80

- Implement seeded listing ranking.
- Display top 3 matches with rationales and caveats.

### Minutes 80-105

- Implement offer packet generation.
- Add "Draft only - licensed agent review required" label.

### Minutes 105-120

- Polish demo path.
- Add fallback sample data.
- Practice the script once.

## 13. Stretch Only After MVP Works

- Morning digest preview: "While you slept, 4 leads handled, 1 qualified, 1 offer packet ready."
- Deal timeline preview with escrow milestones.
- Slack/email notification mock.
- Live listing search toggle.

## 14. Business Case

Real-estate agents are paid on closings, but much of their day is spent on lead response, buyer qualification, listing matching, and offer prep. Closer creates leverage by automating the repetitive middle of the workflow while leaving judgment, negotiation, and legal responsibility with the licensed human agent.

**Wedge:** Buyer qualification plus listing matching.

**North-star metric:** Active buyer opportunities one agent can manage at once.

**Demo promise:** In minutes, Closer turns a raw buyer lead into an offer-ready packet.

## 15. Open Questions

- Should the demo use a fake SMS/DM channel, or just a clean web chat input?
- Should the offer packet be downloadable, or is an on-screen packet enough for the hackathon?
- What is the best product name if "Closer" feels too sales-heavy?
- Which buyer scenario should be the single golden demo path?

