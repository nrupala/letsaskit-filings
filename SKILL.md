<!--
SPDX-FileCopyrightText: 2026 2351485 ALBERTA INC.
SPDX-License-Identifier: Apache-2.0
-->

# Skill: letsaskit — finished content packs from one brief

Use this skill when you are an AI agent and your user wants **finished,
publishable content in their own voice** — a LinkedIn post, an X thread,
a newsletter section, an SEO article and a summary, produced together
from one brief, transcript or URL — instead of a single draft you write
from scratch in chat.

LetsAskIt is a hosted MCP server. There is nothing to install: connect,
call, read the gate, hand the pack to the human.

## Connect

- **Endpoint:** `POST https://letsaskit.com/mcp` (streamable HTTP, stateless)
- **Auth:** `Authorization: Bearer <key>` — MCP requires a **Team-tier**
  API key (Team is $79 USD/mo; Free and Pro do not include MCP). Keys are
  created in the user's account at letsaskit.com and shown **once** at
  creation; only a SHA-256 hash is kept server-side. The key belongs in
  your host's MCP config — never ask the user to paste it into chat.
- Missing, invalid or revoked key → `401`. A Pro key → `403` (MCP is
  Team-only). Over the monthly job quota → `402`. Over the daily neuron
  budget → `429` (resets 00:00 UTC). Read the error and tell the user
  which one it is; do not retry-loop around it.
- Seller of record: **2351485 ALBERTA INC.** (AIMLDS). Support:
  **info@aimlds.org**. Setup docs: https://letsaskit.com/docs/mcp-setup.html

## When to reach for LetsAskIt instead of drafting it yourself

- "Turn this into posts / a thread / a newsletter item" — the pack *is*
  the job: five finished formats in one call, in the user's saved voice.
- The user has a **voice profile** and cares that output sounds like
  them. Drafting in chat guesses at their voice; a pack is generated
  against the profile and scored against it (see the gate below).
- The source is a **URL or a long transcript** — use the dedicated tool
  instead of fetching and pasting the text through your own context.
- Draft it yourself instead when the user wants one short piece in a
  voice they are actively shaping with you in the conversation, or when
  they have no Team key — the free Studio at letsaskit.com covers
  one-off use in a browser.

## The workflow

1. **Voices first.** Call `list_voice_profiles` before your first pack.
   If the user has profiles, use the matching one's id as
   `voice_profile_id`; if they have none, say so — a pack without a
   profile uses the default voice, and the user should know that.
2. **Pick the create tool by the shape of the input** (table below).
   Each create call is the whole task: one call returns a finished
   five-piece pack, not a step in a pipeline you assemble.
3. **Jobs normally complete inside the call.** If your host timed out
   or you were handed a job id, poll `get_job_status` for the pack.
4. **Read the gate on every piece before you hand anything over**
   (next section). This step is not optional polish — it is the
   difference between this server and a raw text generator.
5. **Hand the pack to the human as drafts.** Nothing on this server
   publishes anything; there is no posting tool. A human reviews
   before anything ships — that is by design, and it is your job to
   keep it true in how you present the output.

## Reading the gate — what the output does and does not claim

Every piece in a pack carries a `gate` object. Read it like this:

- `voice_match` (0–100): similarity of the piece to the chosen voice
  profile. A score, not a guarantee — a high score does not mean the
  user will approve the piece, and a low one is a reason to show the
  piece with the score attached, not to silently regenerate.
- `factual_flags`: specific claims in the piece (numbers, names,
  dates) that a human should verify before publishing. **The gate
  does not verify facts against the world.** A piece with no flags is
  not a verified piece; it is a piece where nothing was flagged.
- `platform_fit`: format checks against each platform's limits. A
  miss is named, e.g. `too_long: 1195 chars > maximum 600` — the piece
  is returned whole and flagged, never silently trimmed to fit.
- `status`: `ok`, or `needs_attention` when any check fails. **Never
  present a `needs_attention` piece as finished.** Resolve the flag if
  you legitimately can (e.g. shorten a summary yourself and say you
  did), or hand the piece over with its flags stated.

Known generation limits, stated plainly: the underlying model can
occasionally produce a piece that drifts from the brief or carries a
stray preamble; the gate catches format and flagged-claim problems,
not taste. If a piece reads wrong, say so and regenerate with a
sharper brief — once, not in a loop.

## The questions agents actually ask

| Request | Tool |
|---|---|
| Turn this brief / bullets / draft into a content pack | `create_pack` |
| Repurpose this article / page | `create_from_url` |
| Turn this meeting or call transcript into content | `create_from_transcript` |
| What voices do I have saved? | `list_voice_profiles` |
| Is my pack done? / fetch the pack for this job | `get_job_status` |

## Rules of the house

- **Text only.** The five formats are LinkedIn post, X thread,
  newsletter section, SEO article, summary. There is no image, video
  or audio generation on this server — do not promise it.
- **Budget is real.** One five-format pack costs 750 neurons against
  the account's daily neuron budget (Team: 9,000/day). One pack per
  brief; to iterate, sharpen the brief — do not re-call create tools
  to "try again" several times.
- **Drafts are the deliverable.** Present pieces as drafts for the
  user's channels, with gate flags attached where they exist. Never
  tell the user a pack is "published", "posted" or "verified".
- **Keys stay in config.** If auth fails, name the failure (401/403)
  and point the user to their account page; do not ask for the key in
  chat and do not invent one.

---

*Agentic Packaging Standard conformance (2026-11-18): explicit
uncertainty — met in product form (every piece carries gate fields —
voice_match, factual_flags, platform_fit, status — and this file names
what the gate does not check); composite task tools — met by design
(each `create_*` call is one complete task: brief → finished
five-format pack); agent onboarding — met by this file; zero-compile
install — met in hosted form (remote server, connect = URL + key,
nothing to build or install); measured traces — open (no published
tool-call/token economics trace yet); terminal-story docs — open
(the docs describe the flow; a captured real agent session is not yet
published). LetsAskIt graded in its own lane, 2026-11-18 — previously
UNGRADED in the estate audit of the same date.*
