---
description: Configure referral-engine for connector taxonomy, quiet threshold, trigger patterns, ask cadence, and ICP. Captures how you tag connectors in your CRM so the digest can find them.
---

# /setup-referrals

Short interview — captures what referral-engine needs to find your connectors and surface the right moments.

---

## Step 0 — Resolve plugin config root

Per-plugin config in this marketplace lives under a user-chosen folder, recorded at `~/.claude-plugin-config-root` (single-line text file in the user's home).

### A — Try the pointer

Call `request_cowork_directory(~)` if not granted, then read `~/.claude-plugin-config-root`.
- **Exists**: read line 1 → mount via `request_cowork_directory(<config-root>)`. Skip to section C.
- **Missing**: continue to section B.

### B — First-time bootstrap

Prompt: "First-time plugin setup. Where should I store your plugin config — identity, voice, and per-plugin settings? Pick a folder you control (e.g., `~/Documents/Claude/` or `~/Documents/PluginConfig/`). The folder will hold `identity.md`, `voice.md`, and a `plugins/` subdirectory."

Then:
1. Call `request_cowork_directory(<path>)`. Create `<path>/plugins/`. Write absolute path to `~/.claude-plugin-config-root`.
2. **Migration**: if `~/Documents/Claude/identity.md` or `voice.md` exists and `<path>` is not `~/Documents/Claude/`, offer to copy.
3. **Pre-staged content**: if `~/Documents/Claude/plugin-configs/*.user-context.md` files exist, offer to copy into `<path>/plugins/`.

### C — Read shared identity and voice

Read `<config-root>/identity.md` and `<config-root>/voice.md`. If populated, pre-fill identity and voice questions in this interview. If missing, offer to run `/setup-identity` and `/setup-voice` first or proceed inline.

For the rest of this document, **`<config-root>`** refers to the resolved path. This plugin's config file lives at **`<config-root>/plugins/referral-engine.user-context.md`**.

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

Populate `<config-root>/plugins/referral-engine.user-context.md` per the template structure.

---

## Step 8 — Confirm and offer next step

Summarize. Offer:
> "Try `/referrals` to see this week's digest. Run weekly (Friday afternoon is a common pattern)."

---

## Behavior rules

- One section at a time.
- Skip what doesn't apply (e.g., if user doesn't have CRM custom properties).
- Idempotent.
