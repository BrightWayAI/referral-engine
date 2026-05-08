---
description: Configure referral-engine for connector taxonomy, quiet threshold, trigger patterns, ask cadence, and ICP. Captures how you tag connectors in your CRM so the digest can find them.
---

# /setup-referrals

Short interview — captures what referral-engine needs to find your connectors and surface the right moments.

---

## Pre-step — Read shared identity and voice (if available)

Check `~/Documents/Claude/identity.md` and `~/Documents/Claude/voice.md`. If populated, use values for company info and voice. Otherwise capture inline or recommend running `/setup-identity` and `/setup-voice` first.

---

## Step 1 — Connector taxonomy

How do you tag connectors in your CRM? Pick what applies:

- **CRM property** — e.g., HubSpot's `relationship_type` field with values like "Connector," "Friend/Peer," "Influencer/Creator"
- **List membership** — e.g., a HubSpot list called "Network — Connectors"
- **Tag** — comma-separated CRM tags
- **Past Customer** — automatically include past clients (often the strongest referral source)

Capture per-property values. Multiple categories OK (most users have 2-3 connector types).

---

## Step 2 — Quiet threshold

How many days without contact before someone "goes quiet" and surfaces as a value-share opportunity? Default: 60 days.

Some users do 30 days (high-touch network), some do 90 (lower-touch). Your call.

---

## Step 3 — Trigger patterns

The plugin watches for these triggers as ask-worthy moments. Pick which apply and add custom:

**Default triggers:**
- They replied positively to a recent message (last 14 days)
- A project they referred (or that touched them) closed successfully
- They publicly mentioned you on LinkedIn / podcast / blog
- Their org's fiscal year is flipping in next 30 days
- Their industry's conference season is approaching
- Their org just had positive press / funding / hiring news

**Custom triggers** — anything specific to your domain (e.g., "their org is RFP'ing for [type of work]", "they just hired a [role you usually work with]")

---

## Step 4 — Ask cadence cap

How often is it OK to ask the same connector for a referral?

- Default: every 6 months
- Conservative: every 12 months
- Aggressive (if relationships are very warm): every 3 months

This prevents accidentally double-asking within a short window.

---

## Step 5 — ICP

For the "anyone come to mind in your network?" framing, the plugin needs your ICP.

- If `lead-engine` is installed and configured → pull from there (no questions needed; just confirm)
- Otherwise → capture: titles you sell to, org types/industries, sizes, regions, problem framing ("the kind of org wrestling with [X]")

---

## Step 6 — Voice match

Voice for drafting referral asks comes from `~/Documents/Claude/voice.md`. Confirm:
- Have you run `/setup-voice` (in cortex)? If yes → drafts will use that voice automatically.
- If no → recommend running it; otherwise asks will use generic-warm voice.

---

## Step 7 — Write config

Populate `references/user-context.md` per the template structure.

---

## Step 8 — Confirm and offer next step

Summarize. Offer:
> "Try `/referrals` to see this week's digest. Run weekly (Friday afternoon is a common pattern)."

---

## Behavior rules

- One section at a time.
- Skip what doesn't apply (e.g., if user doesn't have CRM custom properties).
- Idempotent.
