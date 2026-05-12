# Changelog

All notable changes to referral-engine are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/). Versions match `plugin.json`.

## [0.2.4] — Person-page side effect on referral asks (2026-05-12)

### Added
- **`/referral-ask` Step 7 — person-page Recent-interactions append.** After Step 6's `network.md` log, also append a `<today> — referral-ask — shape: <type>, why-now: <trigger>` line to the connector's cortex person page if one exists at `<config-root>/memory/person/<slug>.md`. Update Last meaningful contact and refresh Relationship temperature toward Active.
- **Graceful degradation.** No-op if cortex isn't installed or the connector has no graduated page.

### Why this matters
Phase 3 of SECOND-BRAIN-V2-SPEC. Referral asks are some of the highest-signal interactions — they almost always correspond to a person who deserves a graduated page. This side effect makes referral asks visible on the connector's relationship history so future recall has context on the ask cadence.

## [0.2.3] — Platform-agnostic Step 0 (2026-05-12)

### Changed
- **Setup command Step 0 now platform-agnostic.** Every `request_cowork_directory(...)` call is conditional: "In Cowork, call `request_cowork_directory(...)`. In Claude Code (or any environment with direct filesystem access), no mount is needed." Same plugin source works in both runtimes.

### Why this matters
Phase 0 of SECOND-BRAIN-V2-SPEC. Removes the implicit Cowork-only assumption so Claude Code users do not hit unsupported tool calls during setup.

## [0.2.0] — Config-root refactor

### Changed
- **Plugin config moved to a user-chosen folder.** Reads/writes now go to `<config-root>/plugins/referral-engine.user-context.md` and the network log to `<config-root>/plugins/referral-engine.network-log.md`. The old plugin-relative paths failed silently under Cowork's read-only mount.
- **`/setup-referrals` Step 0 bootstraps the config root** and reads shared identity + voice.
- **`/referrals` and `/referral-ask`** updated to read/write the new paths.
- **User-facing prompts debranded** for fork-friendliness.

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
