---
description: Weekly referral digest. Surfaces connectors who've gone quiet, recent positive moments worth following up, fiscal-year and seasonal triggers, and prioritized actions per connector. Run weekly to stay on top of your latent referral pipeline without manually tracking everyone.
---

# /referrals

Weekly digest that surfaces who's worth a touch *for referrals* this week. The plugin reads your CRM, recent activity, and external triggers (fiscal year, seasonal cycles) to highlight the moments most consultants miss.

---

## Step 0 — Preflight

Read `references/user-context.md`. If missing, route to `/setup-referrals` and stop. Read `~/Documents/Claude/identity.md` (for time zone, fiscal year defaults) and `~/Documents/Claude/voice.md` (for ask drafting).

Extract from this plugin's user-context:
- Connector taxonomy (CRM property + value matching)
- Quiet threshold (default 60 days)
- Trigger patterns (default + custom)
- Ask cadence cap (default: 6 months between asks of the same connector)
- ICP (or pulled from lead-engine user-context if installed)

---

## Step 1 — Pull connector contacts from CRM

Query the CRM for contacts matching your connector taxonomy. Default match in HubSpot:
- `relationship_type` ∈ ["Connector", "Friend/Peer", "Influencer/Creator"]
- Plus past clients (`lifecyclestage` = "Past Customer" or "Customer")

Pull for each:
- Name, title, company, email
- Last contact date
- Recent activity log (notes, meetings, emails — last 90 days)
- Any deals associated (open or recently closed)
- Tags / lists they're on

Cap at 200 connectors — sort by `last_contacted` ascending if more.

---

## Step 2 — Classify each connector

Bucket each into one of:

**A. Quiet** — `last_contacted` >= quiet threshold (default 60d), no upcoming meeting on calendar, no recent activity. Action: send a value-share to stay warm.

**B. Warm with recent positive moment** — recent activity in last 14d that signals a positive touch:
- They replied positively to a recent message
- Project closed successfully (cortex memory shows project-close commit)
- They publicly mentioned you (LinkedIn post, comment, podcast guest mention) — surfaceable via web search
- They referred someone in last 90d (track via CRM note pattern or cortex commit)

These are the **gold** moments — highest-leverage referral-ask windows.

**C. Approaching trigger** — fiscal year boundary in next 30 days, conference season, budget cycle. Action: pre-trigger value-share or wait-and-watch.

**D. Recently asked** — within ask cadence window. Action: don't ask again yet; cooling period.

**E. Active deal contact** — has open deal in pipeline. Action: handled by `/lead-pipeline` or `weekly-outreach`; skip from referral digest.

---

## Step 3 — Cross-reference cortex memory (if installed)

For each Bucket A or B connector:
- Check `~/Documents/Claude/memory/` for any node mentioning this person
- If past commitments exist (e.g., "Promised Sarah I'd send the AI for nonprofits whitepaper"), flag for follow-through

For each Bucket B connector:
- Verify the "positive moment" — is it real? Look at Gmail or CRM notes to confirm it wasn't a misclassification.

---

## Step 4 — Build the digest

```
## Referral Digest — week of [Monday date]

### 🔥 High-leverage asks this week ([N])
[Bucket B — recent positive moments. For each:]
- **[Name]** ([title] @ [company])
  - **Why now:** [specific positive moment, ≤30 words]
  - **Suggested action:** [Specific ask — "Reply to their LinkedIn comment with a referral ask" / "Reply to their email thread referencing X"]
  - Last contact: [date] · Last referral asked: [date or "never"]

### 💧 Going quiet — value-share opportunities ([N])
[Bucket A — connectors not contacted in 60+ days. For each:]
- **[Name]** ([co]) — last touched [date] ([N] days ago)
  - **Suggested value-share:** [Specific share — relevant article / framework / intro / thought-leadership content]

### 📅 Approaching triggers ([N])
[Bucket C. For each:]
- **[Name]** ([co]) — [trigger: fiscal year flip on [date] / [conference] in [month] / etc.]
  - **Suggested action:** [Pre-trigger value-share now / wait and act on [date]]

### ⏸ Cooling (recently asked or contacted)
- [N] connectors not surfaced this week — they're in cooling period or recently warm.

---

### This week's recommendation
**Top 3 actions** (prioritized — do these if nothing else):
1. **[Name]** — [specific action, e.g., "Reply to her congrats note on the [Client] project close — natural moment to ask if she knows other [ICP type] orgs in the same boat"]
2. **[Name]** — [action]
3. **[Name]** — [action]

Want me to draft the asks for any of these? Reply with names or numbers.
```

---

## Step 5 — Offer to draft

If user asks to draft, route to `/referral-ask [contact]` workflow per name. The drafter reads voice rules and produces an ask in voice.

---

## Step 6 — Log the digest run

If `claude-cortex` is installed, append a CHANGELOG entry to `~/Documents/Claude/memory/network.md` (create if needed) with a one-line summary: "Referral digest run [date] — surfaced [N] high-leverage, [N] value-share, [N] trigger-approaching."

This prevents asking the same person twice in the same week and provides historical context for future digests.

---

## Behavior rules

- **Quality > volume.** Better to surface 3 high-leverage moments than 15 weak ones. Cap at "top 3 actions" for clarity.
- **Honor cooling periods.** If a connector was asked for a referral 2 months ago, don't surface as a high-leverage moment again until cadence cap clears (default 6 months).
- **Be specific about "why now."** "Hasn't heard from you in a while" is weak. "Just liked your LinkedIn post about [X] last Tuesday" is specific and actionable.
- **Don't auto-send.** All asks are drafts requiring user review.
- **Cross-check memory.** If cortex memory shows the user already followed up with this person about something else last week, don't surface as "going quiet."

## Edge cases

- **No connectors in CRM** — surface as a setup gap. Recommend running `/setup-referrals` and tagging contacts as connectors first.
- **Single dominant connector** (one person who's referred everyone) — don't over-ask them. Surface as "your top referrer — give them more value than asks this period."
- **Connector died, retired, etc.** — flag for the user to mark in CRM. Don't keep surfacing.
- **Quiet threshold too aggressive** — if too many connectors fall into "quiet" each week, the digest gets noisy. Recommend bumping the threshold in setup.

## What this is NOT for

- **Cold prospecting.** Use `lead-engine` or `bizdev-outreach` for cold direct prospects.
- **Pipeline management** — that's `pipeline-analyst`. Referral-engine is about latent network value, not active deals.
- **Mass-mailing connectors.** The digest surfaces 3-5 high-leverage moments. Don't shotgun your network.
