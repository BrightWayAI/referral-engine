# referral-engine

Latent revenue engine for solo consultants and small agencies.

The pain: you have a network of connectors and former clients who'd happily refer you — but referrals feel awkward to ask for, you forget who's gone quiet, and you miss the natural moments to ask. This plugin tracks the network, surfaces the right moments, and drafts the asks so you actually send them.

## What it does

**`/referrals`** (weekly) — produces a digest of:
- **Who's gone quiet** — connectors you haven't touched in 60+ days
- **Recent positive moments** — connectors / clients who recently had a positive touch (closed project, glowing reply, public mention) — *these are your warmest referral-ask moments*
- **Fiscal-year / seasonal triggers** — connectors whose org is approaching budget cycle, fiscal year flip, conference season
- **Suggested actions** — for each: "ask for a referral now," "send a value-share to stay warm," "wait for trigger X"

**`/referral-ask [contact]`** — drafts a referral ask for a specific connector. Reads:
- Their recent context (CRM activity, last email)
- Why-now signal (recent project close, glowing testimonial, etc.)
- Your voice rules (`~/Documents/Claude/voice.md`)
- Your ICP (so the ask is specific: "I'm working with [type of org] on [type of work] — anyone come to mind?")

Drafts go for your review before sending.

## Install

Recommended: via the [BrightWayAI marketplace](https://github.com/BrightWayAI/claude-plugins).

```
/plugin marketplace add BrightWayAI/claude-plugins
/plugin install referral-engine@claude-plugins
```

## First-time setup

Run `/setup-referrals`. Captures:

- **Connector taxonomy** — how you tag connectors in your CRM (e.g., HubSpot's `relationship_type = Connector` or `Friend/Peer`)
- **Quiet threshold** — how many days without contact before someone's "gone quiet" (default 60d)
- **Trigger patterns** — what counts as a "positive moment worth asking"? (Default list provided.)
- **Ask cadence** — how often to ask the same connector (default: every 6 months max)
- **Voice match** — defaults to `~/Documents/Claude/voice.md`
- **Your ICP** — for the "anyone come to mind?" framing in asks (or pull from `lead-engine` user-context if installed)

Saved to `references/user-context.md` (gitignored).

## Companion plugins

- **CRM connector** — primary input source. Reads connector contacts and their activity history.
- **`claude-cortex`** — captures referral-related context and prevents asking the same connector twice within the cadence window.
- **`lead-engine`** — provides ICP definition for "anyone come to mind?" framing.
- **`bizdev-outreach`** — for one-off, ad-hoc connector touches outside the referral pipeline.
- **Calendar connector** — flags upcoming meetings with connectors as natural opportunities.

Works without companions, but the digest is sharper when it can read across the whole stack.

## How it's different from generic outreach

| | bizdev-outreach | referral-engine |
|---|---|---|
| Goal | Drive a specific outcome (book a call, advance a deal) | Surface latent referral moments |
| Trigger | You're explicitly drafting a message | Periodic scan finds *moments worth acting on* |
| Output | One drafted message | A weekly digest with prioritized actions |
| ICP fit | Drafts for direct prospects | Drafts asks of *connectors* (people who refer to your prospects) |

Use bizdev-outreach when you have a specific contact in mind. Use referral-engine when you want the system to tell you who's worth contacting *for referrals* this week.

## What's inside

```
.claude-plugin/plugin.json
commands/
  referrals.md             Weekly digest workflow
  referral-ask.md          Draft a referral ask for a specific contact
  setup-referrals.md       Interview
skills/
  referrals/SKILL.md       Auto-fires on digest phrases
  referral-ask/SKILL.md    Auto-fires on per-contact ask phrases
  setup/SKILL.md           Auto-fires on setup phrases
references/
  user-context.template.md Structure (committed)
  user-context.md          Your config (gitignored)
  templates/
    referral-ask-templates.md  Voice-faithful starter templates per ask type
```

## License

MIT.
