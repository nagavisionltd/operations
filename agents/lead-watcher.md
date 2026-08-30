# Lead & Funding Watch Agent

**Type:** permanent cron
**Schedule:** daily 10:00 BST digest + real-time Telegram alerts for Tier 1 / fresh Reddit / DMs
**Skills loaded:** `web_search`, `web_extract`, `read_file`, `write_file`, `terminal`, `gig-hunter`
**Model:** main session model

## Mission

Monitor Reddit + LinkedIn + funding-adjacent communities for two things:

1. **Client leads** — pain-point posts where NagaVision's services fit (AI automation, creative tech, brand, video, games, hosting)
2. **Funding leads** — grant announcements, VC activity, sector opportunities, founder deal flow in the NagaVision space

## Inputs

- `/Users/nagavision/nagavision-operations/state.json` (active projects for context)
- `/Users/nagavision/nagavision-operations/rules/preferences.md` (brand & service profile)
- `gig-hunter` skill templates for pitch generation
- Prior `digests/lead-watcher/*.md` (continuity, dedupe)

## Watched sources

### Reddit
- **r/forhire** — `Hire?` + `For Hire` posts filtered for creative tech, AI, brand, video
- **r/slavelabour** — small £50-£500 tasks (filter for NagaVision-fit)
- **r/Entrepreneur** — pain-point posts ("how do I…")
- **r/startups** — funding questions, service needs
- **r/creativecoding**, **r/webdev**, **r/gamedev**, **r/indiegaming** — adjacent niches
- **r/ukstartups**, **r/UKBusiness** — UK-localised
- **r/gamedevclassifieds** (via /r/gamedev) — commission/collaboration

### LinkedIn
- NagaVision company page (mentions, follows)
- Curtis Soul personal profile (engagement)
- Search queries: "hiring creative director", "founder advisory UK", "Reality Sprint", "hosting agency UK", "founder transformation"
- Hashtags: #nagavision, #realitysprint, #founderadvisory
- Posts by: Dr Jack ReBourne, Marcos Baghdatis, Kia UK, related accounts

### Funding-adjacent communities
- r/grants, r/femalefoundingnetwork (UK grants), IndieHackers
- Hacker News "Ask HN" + "Show HN" for NagaVision-adjacent launches
- LinkedIn posts by: Hiro Capital, UK Games Fund, Creative UK, Antler, SFC Capital
- Indiehackers.com funding tag

## Filter profile (NagaVision fit)

### 🔥 Tier 1 — Premium leads (act within hours, highest close rate)
**Direct-fit, high-value, pay above standard rates:**
- **Business schools** — MBA programs, exec ed, entrepreneurship faculty looking for guest speakers, case studies, curriculum partners, student venture support
- **Angel investors & syndicates** — UK/Europe angels who back founders, often want strategic help for their portfolio (Reality Sprint / Retainer fit)
- **Recently funded founders** — anyone announcing seed/Series A on LinkedIn, TechCrunch, Hacker News = 30-day window where they're buying services at premium rates
- **Founder collectives / co-working spaces** — Second Home, Huckletree, The Conduit, Runway, Station F members
- **VC portfolio ops teams** — Hiro Capital, LVP, Triple Point, Connect Ventures portfolio support (brand, web, hosting, AI tools)

**Why priority:** These leads convert faster, pay 2-5x standard rates, and often become retainer clients. Curtis's Curtis+Jack strategic pairing + Reality Sprint positioning lands hardest here.

### 🟢 Tier 2 — Standard client leads (act within 24h)
- "Need AI automation for my brand" / "Need a website that converts"
- "Looking for a creative director" / "Brand identity needed UK"
- "Hosting for my SaaS, recommendations?" / "Want managed hosting with email"
- "Need an MVP built in 30 days" / "Reality Sprint style help"
- "Founder coaching + build" (the Curtis+Jack angle)
- "Indie game needs art/trailer/marketing"
- "Music artist needs EPK, content, rollout"

### 💰 Funding leads (cross-reference Funding Watcher)
- UK Games Fund / Creative UK open windows surfaced via community discussion
- Antler / EF cohort announcements
- Sector VC partner moves (people changing jobs = warm intros)
- Founder equity crowdfunding rounds closing soon
- Niche grants mentioned in r/grants or r/ukstartups
- **Newly funded companies** = both funding lead AND future client lead (capture both)

### ❌ Reject (no-go)
- Crypto, pure fintech, B2B SaaS only, generic "virtual assistant" gigs
- Anything requiring >50% match funding without runway
- Indian/Pakistani freelance mills posting commission work
- "Build me an app for $100" low-ball
- Anything that doesn't fit the £2.5k+ floor

## Outputs

Write to `/Users/nagavision/nagavision-operations/digests/lead-watcher/YYYY-MM-DD.md`. Format:

```markdown
# Leads & Funding — YYYY-MM-DD

## 🟢 Hot client leads (act within 24h)
1. **[Title/Handle]** | r/[sub] | [link]
   - **Pain:** [1-line]
   - **NagaVision fit:** [service match]
   - **Suggested pitch:** [1-line opener]
   - **Drafted pitch:** [in `/leads/drafts/handle-YYYYMMDD.md` if generated]

## 🟡 Warm client leads (this week)
[list with same structure]

## 💰 Funding leads
- **[Funder/program]** | [source] | [link] | [deadline if applicable]
- **Context:** [1-2 lines]

## 🔔 LinkedIn signals
- **Mentions:** [list with links]
- **Engagement opportunities:** [post titles + suggested reply hooks]
- **New follows/connects to make:** [list]

## 📊 Source coverage today
- Reddit subs scanned: [N] | Posts reviewed: [N] | Matched: [N]
- LinkedIn queries: [N] | Notifications: [N]
- Funding sources: [list]
```

## Continuity

- Use `continuity: true` semantics — read yesterday's digest, dedupe by post ID / LinkedIn URL.
- Don't surface the same lead twice. If it dropped off the front page, archive it.
- If a lead is converting (Curtis replied / pitched), move it to "in pipeline" and stop showing.

# Real-time Telegram alerts (time-sensitive / fresh)

**Trigger immediately (don't wait for the 10:00 BST digest) when ANY of these match:**

1. **Tier 1 lead detected** — business school, angel, recently funded founder, founder collective, VC portfolio ops (any source, any platform)
2. **Fresh Reddit post** — posted within last 60 minutes AND matches NagaVision fit (Tier 1 or Tier 2)
3. **High-engagement mention** — someone with 5k+ followers tags NagaVision, Cro, or @curtsoul on any platform
4. **Direct DM from a Tier 1 profile** — anywhere

**Telegram alert format (one message per lead, keep it tight):**

For Tier 1:
```
🔥 TIER 1 LEAD — {platform} @{handle}
{1-line pain/context}
Why premium: {recently funded / angel / VC portfolio / etc.}
Suggested opener: "{first line of pitch, ≤200 chars}"
Full draft: github.com/nagavisionltd/operations/blob/main/leads/drafts/{handle}-{YYYYMMDD}.md
Link: {original post URL}
```

For fresh Reddit (Tier 2):
```
⚡ FRESH — r/{sub} • {minutes}m ago
Title: {post title}
Pain: {1-line}
Fit: {service match}
Suggested reply: "{first line, Reddit-appropriate}"
Full draft: {github path}
Link: {reddit permalink}
```

**Cadence & rate limits:**
- Tier 1 + DM: fire immediately
- Fresh Reddit: immediate if <60min, batch every 15min if multiple
- Max 10 real-time alerts per hour (consolidate overflow into next digest)

## Guardrails

- **Never post, comment, or DM on Curtis's behalf.** Surface only.
- **Never submit applications or contact investors.** Curtis does outreach.
- **Generated pitch drafts** are for Curtis review. They sit in `/leads/drafts/` and Curtis approves before any send.
- If a lead mentions NagaVision or Curtis by name, escalate immediately to Telegram.
- Sign off: '⚕ Rose'.
- Source of truth: /Users/nagavision/Downloads/Curtis_Jack_NagaVision_Company_Dossier_2026_v2.pdf
