# Security Policy

## What this plugin does with your data

Referral Engine tracks connectors in your network, surfaces ask-worthy moments, and drafts referral asks in your voice. Read-only against connectors; writes only drafts plus an optional cortex changelog.

**Reads:**
- **CRM** — contacts matching your connector taxonomy (per `/setup-referrals`); their recent activity, open deals, tags, last-contacted dates.
- **Email** (Gmail) — recent threads with a connector for context during `/referral-ask`.
- **Web** (`WebSearch`, optional) — public mentions to verify "positive moment" signals.
- **Cortex memory** (if installed) — historical asks and connector-related context.
- **Lead Engine user-context** (if installed) — for ICP definition used in ask framing.
- **Plugin references** — `references/user-context.md`, `references/templates/referral-ask-templates.md`.
- **Shared user-level config** — `~/Documents/Claude/identity.md`, `~/Documents/Claude/voice.md` (read-only).

**Writes:**
- **Drafts** — produced inline for review. Not auto-sent.
- **Plugin user-context** — `references/user-context.md` (after `/setup-referrals`).
- **Cortex network log** (if cortex installed) — appends a one-line entry to `~/Documents/Claude/memory/network.md` (creates if missing) recording that a digest ran or an ask was drafted/sent on `[date]`. Used to enforce ask-cadence cooling caps.

**Does not:**
- **Send referral asks on your behalf.** Drafts only.
- **Modify CRM contacts** beyond logging activity (and only when the user confirms a send).
- **Bypass cooling-period rules.** If the user-context says "don't ask the same connector within 6 months" and a connector was asked 4 months ago, the plugin refuses to draft and surfaces the cooling status.
- **Surface contacts with active deals as referral-ask targets.** Active prospects are excluded from the digest — asking referrals from someone you're trying to close is a misfit and the plugin filters them out.

## Where data lives

- Plugin reference files inside the installed plugin directory.
- Drafts and digest output inline in conversation.
- Cortex network log at `~/Documents/Claude/memory/network.md` (if cortex installed).

## What gets sent off your machine

- Whatever your authorized CRM, Gmail, WebSearch connectors send when invoked.

## Privacy note about the connector network

The plugin reads contact details for connectors you've tagged in your CRM. Treat the resulting digest and drafts as confidential — they reveal who's in your referral network and what your relationship with each connector looks like.

## Supported versions

| Version | Supported |
|---------|-----------|
| 0.1.x   | Yes       |

## Reporting a vulnerability

Report privately via GitHub Security Advisories:

https://github.com/BrightWayAI/referral-engine/security/advisories/new

Do not open a public issue for security concerns. We aim to respond within 5 business days.
