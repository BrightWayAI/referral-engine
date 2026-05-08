---
description: Draft a referral ask for a specific connector. Reads their recent context (CRM activity, last email), the why-now signal, your voice rules, and your ICP. Produces a draft ask ready for your review and send. Use after /referrals surfaces a high-leverage moment, or ad-hoc when you spot one organically.
---

# /referral-ask [contact-name-or-email]

Draft a referral ask for one specific connector.

---

## Step 0 — Preflight

Read `references/user-context.md` (this plugin) and `~/Documents/Claude/voice.md` (shared voice). Read `~/Documents/Claude/identity.md` for company info.

If `lead-engine` is installed, read its user-context for ICP definition. Otherwise pull from this plugin's user-context (which captured ICP during /setup-referrals).

---

## Step 1 — Identify the contact

If contact name passed → look up in CRM (search by name; disambiguate by company if needed).
If email passed → direct lookup.

If contact not found in CRM: ask user "Not in CRM — paste any context you have on them and I'll work from that."

---

## Step 2 — Gather context

For the contact, pull:

**A. CRM**
- Recent notes / activity (last 60d)
- Last meeting if any
- Their tagged role (Connector / Past Customer / Friend-Peer / etc.)
- Open deals (if any — usually shouldn't be asking referrals from active prospects)

**B. Recent email exchange** (Gmail or equivalent)
- Last 1-2 threads — verbatim. The thread context is critical for "what's the natural way to reference this?"

**C. Public signal**
- Optional: WebSearch for "[name] [company] recent" to surface anything they've publicly said in the last 30 days

**D. Cortex memory** (if installed)
- Check for prior referral asks of this person (don't double-ask within cadence cap)
- Check for any cortex-captured context about them

---

## Step 3 — Determine the ask shape

Match to the right ask type based on context:

### A. Post-positive-touch ask
**When:** They just did something positive — replied warmly, sent a glowing note, publicly mentioned you, made an intro.
**Shape:** Reference their specific positive moment, then make the ask easy.

> "Thanks for [specific thing they did]. While I have you — I'm working with [type of org] on [type of work] right now and looking to talk to a couple more like them. If anyone in your network is wrestling with [problem you solve], I'd love a forwardable intro. Happy to send a 1-paragraph blurb you can pass along if helpful."

### B. Post-project ask
**When:** A project just closed (theirs or another client they referred).
**Shape:** Reference the specific outcome, anchor the ask to the proof.

> "[Specific outcome from the project, e.g., 'Now that the [Client] AI Op Model is wrapped — they had X land really well.'] Curious if anyone in your network is wrestling with [related problem]. I have capacity opening up in [month] and would love to do another one of these."

### C. Re-warm + ask
**When:** They've gone quiet (60+ days) but were warm before. The ask comes after a value-share, not as the lead.
**Shape:** Lead with value, ask near the end as a soft afterthought.

> "Saw [specific thing in their world] and thought of [their initiative / our last conversation]. [1-2 sentences sharing something genuinely useful — article / framework / intro / observation.] On my end, [brief one-line update on what you're up to]. Anyone come to mind in your network worth a conversation? No pressure — happy to keep the door open."

### D. Trigger-based ask
**When:** Fiscal year, conference, or budget cycle approaching.
**Shape:** Reference the trigger, frame the ask as helping them help their network.

> "[Specific trigger] coming up — I know [their org / industry] is heading into [budget season / conference / FY-flip]. If you're hearing from peers wrestling with [your problem space] as they plan, happy to be a resource — even a 15-min sounding-board conversation, no pitch. Forward this if useful."

---

## Step 4 — Draft

Apply voice rules from `~/Documents/Claude/voice.md`:
- Voice descriptors (3 words)
- Banned phrases (none of "just checking in," etc., plus user's custom banned list)
- Sentence length / rhythm
- Sign-off pattern

Apply ICP framing — be specific about *who you're trying to talk to*:
- "I'm working with [type of org] on [type of work]" → uses ICP from lead-engine user-context or this plugin's user-context
- The connector should be able to mentally browse their network and recognize a fit

Length cap: typically 3-5 sentences for the ask itself (not counting any value-share preamble in re-warm asks).

---

## Step 5 — Output the draft

```
**Referral Ask — [Contact Name]**

**Why now:** [the trigger / positive moment]
**Ask shape:** [post-touch / post-project / re-warm / trigger-based]
**Channel:** [email / LinkedIn DM — recommend based on prior correspondence]
**Subject** (if email): [subject line]

---

[Full draft]

---

**Notes for sending:**
- [Any context the user should hold while sending — e.g., "Last referred Acme to you in Feb — be ready if they ask 'how'd that go'"]
- [Pacing note — e.g., "If no reply in 2 weeks, don't follow up; this is a soft ask"]
```

---

## Step 6 — Log

If user confirms they're sending, log to cortex memory `network.md` (or wherever connector tracking lives):

```
[YYYY-MM-DD] Referral ask sent to [Name] — shape: [type], why-now: [trigger]
```

Prevents double-asking within cadence cap.

---

## Behavior rules

- **Specific > generic.** "Anyone in your network" is weak. "Anyone in your nonprofit network wrestling with how to roll out AI safely" is specific.
- **Honor voice rules.** Read `~/Documents/Claude/voice.md` and follow it. If it bans "leverage," don't use it.
- **No pressure language.** "Happy to," "no rush," "if useful" — the ask is a *standing invitation*, not a transaction.
- **One clear ask.** Don't bury two asks in one message. Pick the strongest.
- **Don't draft for someone in the cooling window.** If user-context says don't ask the same person within 6 months and this person was asked 4 months ago, refuse and surface in the response: "Cooling — last asked [date]. Want to send a value-share instead?"

## Edge cases

- **Contact has an open deal in your pipeline** — don't draft a referral ask. Asking a prospect for referrals while you're trying to close them is weird. Surface and recommend `bizdev-outreach` for that contact instead.
- **Contact is technically a connector but has never referred anyone** — fine, draft the ask. First-time asks should be slightly softer than subsequent ones.
- **Cortex memory shows you owe them something** (e.g., "promised Sarah a teardown of X") — flag and recommend completing that first; the ask after delivering will land better.
