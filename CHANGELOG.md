# Changelog

All notable changes to referral-engine are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/). Versions match `plugin.json`.

## [0.1.0] — Initial release

### Added
- Weekly referral digest (`/referrals`) — surfaces three buckets: high-leverage asks (recent positive moments), going-quiet value-share opportunities (60+ days no contact), approaching triggers (fiscal year flips, conference seasons). Caps at top 3 actions for the week to avoid noise.
- Referral ask drafter (`/referral-ask [contact]`) — drafts a voice-faithful ask for a specific connector. Four ask shapes:
  - Post-positive-touch (after they did something positive)
  - Post-project (after a project closed)
  - Re-warm (after going quiet, lead with value)
  - Trigger-based (fiscal year, conference, budget cycle)
- `/setup-referrals` interview — captures connector taxonomy (CRM property/list/tag matching for "Connector," "Friend/Peer," etc.), quiet threshold (default 60 days), trigger patterns (default + custom), ask cadence cap (default 6 months between asks of the same connector), ICP source (lead-engine user-context or inline).
- Honors cooling periods — refuses to draft an ask for a connector within their cadence window and surfaces the cooling status instead.
- Excludes connectors with active deals — asking referrals from active prospects is a misfit.
- Cross-references cortex memory at `~/Documents/Claude/memory/network.md` (creates if missing) for historical asks and ask-cadence enforcement.
- Setup reads `~/Documents/Claude/identity.md` and `~/Documents/Claude/voice.md` (shared config from cortex) when available.
- User-editable templates at `references/templates/referral-ask-templates.md`.
