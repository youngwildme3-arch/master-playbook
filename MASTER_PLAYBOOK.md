# YWM CONSULTING — MASTER PLAYBOOK

> Single source of truth for everything Evan Jones is building at YWM Consulting. Read this first when starting any new conversation about CallCanvas AI, RentRanger AI, or any future site in the portfolio. Updated May 4, 2026.
>
> **Note:** All credentials (API keys, PATs, admin keys, etc.) are stored in Claude's memory layer, NOT in this document. This document is the public-readable architectural reference.

---

## SECTION 0: HOW TO USE THIS DOCUMENT

If you are Claude reading this in a new conversation, your first action is:

1. Read this entire file
2. Read your memory entries (credentials, current state, behavioral preferences live there)
3. Search past conversations for the most recent context: `conversation_search` with the site name being asked about
4. Check the latest commits on the relevant repo to see actual current state of code
5. Then respond to the user

The playbook is the **stable** layer. Memory entries are the **volatile** + **credentialed** layer. Past conversations are the **detailed** layer. Use all three.

If Evan says any of these phrases, this is what they mean:

- **"CallCanvas"** → site #1, callcanvasai.com, $59/mo territory research for outside sales reps
- **"RentRanger"** → site #2, rentrangerai.com, $39/mo apartment hunting AI for renters
- **"Make me a new website on my desktop"** → activate the duplication playbook (Section 4) for site #N
- **"Run silent"** → minimal narration, batch tool calls, stop only for credentials or completion
- **"Take over"** → use Chrome extension end-to-end without per-step confirmation
- **"Cooldown"** → walk away from a deployed site for 24-48 hours and verify it auto-runs without intervention
- **"Big batches"** → use `browser_batch` for everything; predict 5-10 actions ahead

---

## SECTION 1: THE OPERATOR

**Evan Jones** runs **YWM Consulting** out of **Sugar Grove, Illinois**.

- Working name: YWM Consulting (NOT an LLC, NOT incorporated)
- Legal structure: Sole Proprietor / Individual
- Tax structure: Personal Schedule C
- Background: Banking / commercial lending; relationships with small business owners; B2B sales prospecting experience
- Other: YWM Family lifestyle/influencer brand (~2M followers) — separate from this portfolio

**Critical:** Never pre-fill "LLC" or "Corporation" anywhere. Always Sole Proprietor / Individual until Evan says otherwise. Quote: "I'm not gonna incorporate it until I'm making money."

**Communication style:**
- Minimal narration. One status line per major milestone.
- No "should I continue?" confirmations.
- Batch tool calls aggressively. Use browser_batch for everything.
- Push back honestly when something won't work.

**Email accounts** are stored in memory entries.

---

## SECTION 2: ACTIVE PORTFOLIO

### SITE #1: CallCanvas AI
- Domain: callcanvasai.com
- Status: LIVE, profitable-ready, Netlify Pro tier
- Niche: Territory research SaaS for outside sales reps
- Price: $59/month, 7-day trial, 30-day money-back guarantee
- Customer: Individual outside sales reps (insurance, financial services, B2B tech, telecom, commercial services)
- One-line promise: "Research 50 companies in under 5 minutes — owners, decision-makers, phones, opening lines"
- AI workhorse: territory-research.js (1, 3, 5, 10, 25, 50 prospects per search)
- Stripe: Live (Sole Prop)
- GitHub: youngwildme3-arch/callcanvas-site

### SITE #2: RentRanger AI
- Domain: rentrangerai.com (Spaceship, DNS pending)
- Live URL: rentrangerai.netlify.app
- Status: LIVE on Netlify, feature-complete, awaiting Stripe + DNS
- Niche: Apartment hunting AI for renters in tough markets
- Price: $39/month, 7-day trial, 30-day money-back guarantee
- Customer: Apartment hunters in competitive markets
- One-line promise: "Find the apartment before someone else does"
- AI workhorse: apartment-search.js
- Aesthetic (DIFFERENT from CallCanvas): Warm cream + terracotta + warm brown + soft gold; Fraunces serif headlines + Inter body; full-bleed Unsplash apartment photos; house icon logo
- Stripe: NOT YET CONFIGURED
- GitHub: youngwildme3-arch/rentrangerai-site

---

## SECTION 3: CREDENTIALS (STORED IN MEMORY)

All credentials live in Claude's memory layer, not in this document. Memory contains:

- GitHub PAT
- Netlify PAT (no-expiration token)
- Anthropic API key (used by both sites)
- Stripe account (Sole Prop, live mode)
- Stripe Price ID for CallCanvas
- Admin keys for CallCanvas and RentRanger dashboards
- Netlify Site UUIDs for both sites
- Account IDs and login emails

If a credential is missing from memory, ask Evan. Never guess. Treat all credentials as sensitive — never commit them to a public repo.

---

## SECTION 4: THE STREAMLINED BUILD PROCESS

Repeatable process for spinning up a new SaaS site. ~2 hours total, ~30-45 min Evan's manual time.

### Step 0: Niche validation
1. Confirm fits template constraints (Section 8)
2. Pick name; suggest 10-15 candidates
3. Verify .com via Verisign RDAP: https://rdap.verisign.com/com/v1/domain/{name}.com (404=available, 200=taken)
4. Have Evan buy domain (Spaceship cheapest, ~$2.90 first year with code COMPROS, ~$10/yr renewal)
5. Confirm before proceeding

### Step 1: Create GitHub repo
POST to https://api.github.com/user/repos with name {site-name}-site, private:true, auto_init:true.

### Step 2: Foundation commit (Phase 1)
Atomic commit with: _config.js, _helpers.js, predeploy-qa.js, netlify.toml, package.json (deps: @netlify/blobs ^8.1.0, stripe ^17.4.0), README.md, robots.txt. Use Git Trees API.

### Step 3: Core commit (Phase 2)
chat.js (Alex), validate-coupon.js, {niche}-search.js (AI workhorse), public/index.html.

### Step 4: Create Netlify site + env vars + GitHub link
POST to /api/v1/{ACCT}/sites. Required env vars: ANTHROPIC_API_KEY, NETLIFY_SITE_ID, NETLIFY_PAT, SITE_URL, ADMIN_KEY (24 random bytes hex = 48 chars).

CRITICAL: First deploy fails with "Host key verification failed" because GitHub App isn't auto-linked. Have Evan click Manage repository → Link to a different repository in Netlify UI.

### Step 5: Pages commit (Phase 3)
8 HTML files + check-access function: research, login, account, welcome, help, privacy, terms, contact, sitemap.xml, check-access.js.

### Step 6: Full feature commit (Phase 4) — 14 files
4 Stripe (create-checkout, customer-portal, stripe-webhook, verify-checkout-session). 3 Outreach bots. 2 Admin dashboards (gated by ADMIN_KEY). 3 Blog functions. 1 Health check. Updated netlify.toml with cron schedules.

### Step 7: Aesthetic commit (Phase 5) — make it visually DIFFERENT
Cardinal rule: every site visually differs. Distinct color palette, typography, imagery, logo, tone.

### Step 8: Stripe wire-up (Evan, ~5 min)
Create Product → Recurring → save Price ID. Add Webhook → save secret. Tell Claude. Claude updates env vars + redeploys.

### Step 9: DNS (Evan, ~2 min)
A record @ → 75.2.60.5. CNAME www → {slug}.netlify.app. Wait 5-30 min. Add custom domain in Netlify → SSL auto-provisions.

### Step 10: 48-hour cooldown
Don't touch for 2 days. Verify daily via /_health.json + admin dashboard.

---

## SECTION 5: ARCHITECTURE PATTERN

3 files drive everything else.

### _config.js — Single source of truth per site
PRODUCT_NAME, PRODUCT_TAGLINE, PRODUCT_DESC, PRICE_USD, PRICE_DISPLAY, TRIAL_DAYS, GUARANTEE_DAYS, CUSTOMER_DESC, CUSTOMER_PROBLEM, SITE_DOMAIN, SUPPORT_EMAIL, BRAND_PRIMARY, BRAND_BG, RESEARCH_FN_NAME, RESEARCH_OUTPUT_KEY, AI_RESEARCH_INSTRUCTION, OUTREACH_BRAND_VOICE, FAQS array.

### _helpers.js — Shared utilities
getBlobStore (explicit siteID + token), callClaude (Haiku for chat, Sonnet for workhorse), parseBody, validateFields, jsonResponse, errorResponse, htmlResponse, siteUrl (env-driven, NEVER hardcode), isValidEmail, todayISO, yesterdayISO, getWithYesterdayFallback, watermark.

### predeploy-qa.js — Build gate
5 checks: hardcoded URLs, deprecated Claude models, UTF-8 mojibake byte signature, bare getStore() calls, missing _helpers imports.

---

## SECTION 6: 8 BUG CATEGORIES FROM CALLCANVAS

| # | Bug | Template fix |
|---|-----|--------------|
| 1 | Netlify Blobs auto-config silently failed | All blobs through getBlobStore() with explicit siteID + token |
| 2 | Functions had no input validation | validateFields() helper returns 400 |
| 3 | Hardcoded production domain in 3 functions | siteUrl() reads from env. predeploy-qa blocks literals |
| 4 | Affiliate program left ghost references | Template doesn't ship with affiliate code |
| 5 | UTF-8 mojibake — em-dashes quadruple-encoded | predeploy-qa scans for byte signature |
| 6 | Wrong Claude model snapshots | predeploy-qa blocks deprecated names |
| 7 | Frontend wiring broke silently | Health check has frontend tests |
| 8 | AI prompts hallucinated features | All prompts pull from _config.PRODUCT_DESC |

---

## SECTION 7: HOW MONEY FLOWS

### Customer flow
1. Visitor → {site}.com → "Start 7-Day Free Trial"
2. /.netlify/functions/create-checkout
3. Stripe Checkout Session with trial_period_days: 7
4. Card details on Stripe-hosted page
5. Redirect to /welcome?session_id=...
6. Welcome page → /verify-checkout-session
7. /research → AI workhorse

### Webhook flow
Stripe POSTs to /.netlify/functions/stripe-webhook on every event:
- checkout.session.completed → save email + customer_id to Blobs
- customer.subscription.created/updated → mark active
- customer.subscription.deleted → mark cancelled
Signature verification via STRIPE_WEBHOOK_SECRET. Unsigned = 400.

### Real costs per site
| Item | Cost |
|------|------|
| Domain (Spaceship) | ~$10/yr / 12 = $0.85/mo |
| Netlify Pro (shared) | $19/mo total |
| Anthropic API (~50 users) | ~$15-30/mo |
| Stripe fees | 2.9% + $0.30 per txn |
| Variable cost per active sub | ~$1-2/mo |

### Revenue at 100 subs ($59 site)
Gross $5,900/mo. Stripe ~$200. Anthropic ~$80. Net ~$5,600/mo per site.

---

## SECTION 8: SITE TEMPLATE RULES

Hard rules for new niches:
1. AI generates content per query — that IS the product. No scraping.
2. Input is text/form-based.
3. Output saves the customer hours.
4. Customer uses it MULTIPLE times.
5. Individual user buys, not team buy-in.
6. Real pain people search for.
7. Domain .com under $15/yr.
8. Subscription $29-99/mo.

### Niche candidates
**Built:** CallCanvas AI ($59), RentRanger AI ($39).
**Strong:** Insurance Claim Appeal Writer ($29-39), Grant Writer for Solo Nonprofits ($79-99), Layoff Recovery Kit ($49), Interview Prep AI ($29-39), Cold Email Scripts ($39-49), Listing Optimizer ($29), Ad Copy Generator ($49), Local SEO Audit ($59).
**Rejected:** Amazon arbitrage / Alibaba margin finder — scraping, TOS violations, $200-500/mo data costs.

---

## SECTION 9: ALEX CHAT

Both sites have an "Alex" chat widget. Same character, niche-specific knowledge.

- /.netlify/functions/chat
- Model: claude-haiku-4-5-20251001
- Widget HTML/CSS in public/index.html

System prompt template (instantiated from _config.js): Alex as AI sales/support assistant. Includes PRODUCT_DESC, pricing, trial, money-back guarantee. Voice: direct, friendly, slightly contrarian, never pushy. 3 sentences max.

Greeting: "Hey! I'm Alex. Ask me anything about {PRODUCT_NAME} — pricing, what it does, how the trial works, whatever."

Critical: em-dash MUST be U+2014. CallCanvas hit a bug where it got quadruple-UTF-8-encoded into 6KB of mojibake. Predeploy-qa scans for the byte signature.

---

## SECTION 10: OUTREACH BOTS

Three bots generate daily content for Evan to manually copy/paste. NO automated posting.

### linkedin-content.js
Cron: 0 8 * * *. Single 200-280 word post. Voice from OUTREACH_BRAND_VOICE. Angles rotate. No hashtags, no emojis, no buzzwords.

### facebook-scripts.js
Cron: 0 9 * * *. 5 hypothetical-post + reply pairs. Reply gives value first, mentions product as PS.

### reddit-outreach.js
Cron: 0 10 * * *. 3 subreddit-specific scripts. 1 of 3 should NOT mention product.

### Usage
Each morning Evan visits {site}.com/outreach?key={ADMIN_KEY}, copies into LinkedIn/FB/Reddit. ~10 min/day. Dashboard has yesterday-fallback for dead zone.

---

## SECTION 11: BLOG SYSTEM

### daily-blog-background.js
Cron: 0 11 * * *. Generates 700-1100 word post. Topic rotates. Saves to Blobs (blog store, key post:{slug}). Updates manifest with 50 most recent.

### blog-index.js
Serves /blog/. Reads manifest, renders post cards.

### blog-post.js
Serves /blog/{slug}. Reads post by slug, converts markdown to HTML inline. CTA card at bottom.

---

## SECTION 12: ADMIN DASHBOARDS

Two dashboards. Both 404 without admin key.

### /outreach?key={ADMIN_KEY}
Today's LinkedIn / Facebook / Reddit content. Yesterday-fallback.

### /growth-log?key={ADMIN_KEY}
Stats stub. Wires up when Stripe metrics flow.

### Auth
Function checks queryStringParameters.key against process.env.ADMIN_KEY. If mismatch, returns clean 404 HTML.

ADMIN_KEY for each site is in Claude memory.

---

## SECTION 13: AI WORKHORSE FUNCTION

Same architecture, different prompt + output schema per site.

### CallCanvas: territory-research.js
- Model: claude-sonnet-4-5-20250929
- Input: city, state, product, count, couponCode/email
- Output: companies with name, owner, phone, address, why_match, cold_open, talk_tracks, priority
- Special: 50-count uses two parallel 25-company calls (avoids 26s timeout)

### RentRanger: apartment-search.js
- Model: claude-sonnet-4-5-20250929
- Input: city, budget_max, beds, preferences, specific_building, count, couponCode/email
- Output: apartments with building_name, address, monthly_rent, beds, baths, sqft, building_type, year_built, landlord, landlord_style, amenities, fit_reason, opening_message, talk_tracks, priority

### JSON-only pattern
System prompt MUST end with: "OUTPUT FORMAT: Return ONLY valid JSON, no prose, no markdown fences."
Function defensively strips markdown fences if Claude wraps despite instructions.

### Watermarking
Every output stamped via watermark(): licensed_to (coupon code or email), generated_at (ISO), site (SITE_URL).

---

## SECTION 14: HEALTH CHECK

/.netlify/functions/site-health-check — 7 tests, cron 30 11 * * * daily.

1. env_anthropic_key
2. env_netlify_blobs
3. env_admin_key
4. env_site_url
5. blob_read_write (write/read/delete roundtrip)
6. claude_api_ping ("Reply with the single word: pong")
7. stripe_optional (soft check)

Returns { overall_ok, failed_count, failed_tests, tests, timestamp, today }. Convenience: /_health.json.

---

## SECTION 15: ATOMIC GIT TREES COMMITS

Every push uses Git Trees API. NEVER push files one at a time.

1. GET /git/ref/heads/main → parent SHA
2. GET parent commit → base tree SHA
3. POST /git/blobs (one per file)
4. POST /git/trees with all blobs
5. POST /git/commits pointing at tree
6. PATCH /git/refs/heads/main → new commit SHA (triggers Netlify deploy)

---

## SECTION 16: NETLIFY ENV VAR PATTERNS

### Bulk set
POST /api/v1/accounts/{ACCT}/env?site_id={SITE_ID} with array of {key, values, scopes:['functions'], is_secret}.

### Update one
PUT /api/v1/accounts/{ACCT}/env/{KEY_NAME}?site_id={SITE_ID}.

### Critical
Secret env vars CANNOT use context "all". Always set 3 contexts: production, deploy-preview, branch-deploy.

### Trigger redeploy
POST /api/v1/sites/{SITE_ID}/builds with clear_cache:true.

---

## SECTION 17: COMMON GOTCHAS

### CORS preflight
Cross-domain testing blocked. Solution: navigate to the site you're testing.

### Browser permission walls
Chrome extension requires per-domain permission. Have Evan visit once, or use Netlify/GitHub/RDAP APIs.

### Netlify GitHub auto-link
First deploy fails with "Host key verification failed". Fix: Manage repository → Link to a different repository in Netlify UI. One click, then auto-deploy works forever.

### installation_id won't accept programmatic update
PATCHing Netlify site API with installation_id is silently rejected. MUST use UI.

### PAT expiration
Default 7 days. Always set "No expiration".

### Mojibake byte signature
Quad-encoded em-dash: bytes [195,131,194,162,195,130,194,128,195,130,194,148] (12 bytes, displays as garbage). Replace with [0xE2, 0x80, 0x94] (3 bytes).

### Illinois SOS unreachable
apps.ilsos.gov blocks both Chrome extension and web fetcher. Manual searches only.

### Repo deletion needs delete_repo PAT scope
Standard repo PATs lack it. Either request user to add it, or have user delete via UI.

### GitHub secret scanning
Public commits with PATs/keys auto-revoke within minutes. Never commit credentials, even briefly.

---

## SECTION 18: AUTOPILOT PIPELINE (NOT YET BUILT)

Spec — needs n8n + Twilio + Namecheap API + Airtable.

### Workflow A — Mon 6 AM
Scour trends → 10 candidate ideas → RDAP availability → filter under-$15/yr → Twilio SMS to Evan with top 3.

### Workflow B — Mon 9 AM
Triggered by SMS approval. Runs Section 4 autonomously. Twilio SMS when site live.

### Queue: Airtable
Site name, niche, status, 60-day metrics.

### 60-day eval
Keep if: any paid sub OR 500+ visitors. Kill if: 0 paid + under 100 visitors + under 5 signups.

---

## SECTION 19: CURRENT STATUS (May 4, 2026)

### CallCanvas AI
- Live, Netlify Pro
- Stripe wired ($59/mo, 7-day trial, webhook signature verified)
- All bots running daily
- Blog cron running daily
- Health check passing daily
- Admin dashboards hidden behind ADMIN_KEY
- 48-hour cooldown verified — runs hands-off

### RentRanger AI
- Live on rentrangerai.netlify.app
- 19 functions deployed
- Warm aesthetic deployed
- Cron schedules wired
- Stripe product not yet created
- DNS rentrangerai.com not yet pointed

### Unlock RentRanger to public
1. Evan creates Stripe Product ($39/mo, 7-day trial)
2. Evan sets webhook destination
3. Claude updates 3 Netlify env vars
4. Evan points DNS A → 75.2.60.5 + CNAME www → rentrangerai.netlify.app
5. Wait 5-30 min DNS + SSL
6. Test end-to-end checkout
7. Begin 48-hour cooldown

---

## SECTION 20: MONEY HARD CONSTRAINTS

- Domain budget: under $15/yr
- Hosting: Netlify Pro shared $19/mo
- Stripe: 2.9% + $0.30 per txn
- Anthropic: Haiku ~$0.25/M tokens, Sonnet ~$3/M tokens
- ONE Stripe account, ONE Netlify account, ONE GitHub user, ONE Anthropic key

### Per-site startup cost
~$3 (domain only). Everything else shared.

---

## SECTION 21: WHEN EVAN SAYS X, DO Y

| Phrase | Action |
|--------|--------|
| Make me a new website on my desktop | Pull this playbook, ask 5 blanks: name, promise, price, customer, domain |
| Run silent | Stop narrating. One status line per major milestone |
| Take over | Chrome extension end-to-end, no per-step confirmation |
| Cooldown | Stop work. Verify automation runs hands-off 24-48hr |
| CallCanvas | Site #1, callcanvasai.com |
| RentRanger | Site #2, rentrangerai.com |
| Big batches | browser_batch for everything. Predict 5-10 actions ahead |

---

## SECTION 22: SHORT BIO FOR FUTURE CLAUDE

Evan Jones, sole proprietor, Sugar Grove IL. Building portfolio of AI-powered SaaS sites. Site #1 CallCanvas AI ($59/mo, sales rep tool, live and stable). Site #2 RentRanger AI ($39/mo, apartment hunting, live but pre-Stripe). Both built on same template — _config.js + _helpers.js + predeploy-qa.js foundation. Evan controls Chrome extension that lets you (Claude) drive the browser. He prefers minimal narration and large batches. Memory entries hold credentials — read them. Search past conversations for what you don't have memorized. Code repos: youngwildme3-arch/{site}-site. Context repo: youngwildme3-arch/master-playbook (this file).

---

## SECTION 23: APPENDIX — FILE TREE

A fully-built site has 30 files, ~90KB source, ~$3/yr running cost after shared Netlify Pro tier.

Root: README.md, package.json, netlify.toml.
scripts/: predeploy-qa.js.
netlify/functions/: _config.js, _helpers.js, chat.js, validate-coupon.js, {niche}-search.js, check-access.js, create-checkout.js, customer-portal.js, stripe-webhook.js, verify-checkout-session.js, linkedin-content.js, facebook-scripts.js, reddit-outreach.js, outreach-dashboard.js, growth-dashboard.js, daily-blog-background.js, blog-index.js, blog-post.js, site-health-check.js.
public/: index.html (changes per site), research.html, login.html, account.html, welcome.html, help.html, privacy.html, terms.html, contact.html, sitemap.xml, robots.txt.

---

*End of Master Playbook. Version 1.1, May 4, 2026.*
*Public reference. Credentials in Claude memory only.*
