# 🚀 AI Cold Email Campaign Builder — Complete GTM Starter Kit

**By Manthan Patel (@LeadGenMan)**

---

This is the exact system I use to build entire outbound campaigns using AI + [Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan). From finding companies to scoring ICPs, writing personalized emails, and pushing live campaigns — all with natural language prompts.

No code. No guesswork. Just type what you want, and watch it execute.

> 💡 **What you're getting:** A complete, copy-paste-ready system that turns Claude into your personal SDR. You'll have the folder structure, the brain file, scoring templates, email frameworks, and the exact prompts to run everything end-to-end.

---

## 🔧 Tools You'll Need

| Tool | What It Does |
|------|-------------|
| [Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan) | Your all-in-one cold email platform. Lead Finder (800M+ contacts), email sequences, AI copilot, analytics. **This is your command center.** |
| [Bright Data](https://get.brightdata.com/manthan) | Web scraping & data collection. Use for company research, competitor analysis, and enriching prospect data at scale. |
| [Maildoso](https://maildoso.com/?ref=manthan) | Email infrastructure. Set up unlimited sending accounts with proper deliverability for cold outreach. |
| [Clay](https://clay.com?via=566505) | Data enrichment & waterfall workflows. Layer multiple data sources to build hyper-targeted prospect lists. |
| [Apify](https://www.apify.com?fpr=f7e9d) | Web scraping actors. Scrape LinkedIn profiles, company websites, review sites for prospect research. |
| [n8n](https://n8n.partnerlinks.io/manthan) | Workflow automation. Connect your AI agents to run campaigns on autopilot. |
| [Make](https://www.make.com/en/register?pc=manthan) | Visual automation platform. Build no-code workflows that connect all your GTM tools. |
| [Aimfox](https://aimfox.com/?ref=manthan) | LinkedIn outreach automation. Run multi-channel campaigns alongside your email sequences. |
| [GHL / GoHighLevel](https://www.gohighlevel.com/affiliate-30trial?a=manthanpatel) | All-in-one CRM & marketing platform. Manage leads, pipelines, and follow-ups. |
| [Railway](https://railway.com?referralCode=Manthan) | Cloud hosting platform. Deploy your AI agents, APIs, and automation backends with zero DevOps. |

---

## 📁 The Folder Structure

This is what your project looks like. Every file has a purpose.

```
ai-cold-email-campaign/
├── CLAUDE.md              ← The Brain (tells AI what to do)
├── scoring-criteria.md    ← ICP Scoring Rubric
├── copy-framework.md      ← Email Copy Rules
├── saleshandy-api-docs.md ← API Documentation
├── api-keys.md            ← Your API Keys
├── .env                   ← Environment Variables
├── lib/
│   ├── saleshandy_client.py  ← Saleshandy API Wrapper
│   └── email_enricher.py     ← Email Enrichment
├── outputs/
│   └── companies.csv      ← Your Company List
└── requirements.txt       ← Python Dependencies
```

> 🧠 **Why this structure matters:** When Claude opens your project, it reads `CLAUDE.md` first. That file points to everything else — your scoring rubric, copy rules, API docs. It's like giving a new employee an onboarding manual on day one.

---

## 🧠 Step 1 — Create Your Brain File (CLAUDE.md)

The brain file is the single most important file in your project. It tells Claude:

- What tools exist and how to use them
- What strategy docs to read before acting
- The correct pipeline order (score → enrich → write → deploy)
- Rules to follow (use Python for scoring, save outputs, never expose API keys)

> ⚠️ **Without this file, Claude is guessing. With it, Claude is executing your exact playbook.**

### Full CLAUDE.md Template (Copy This)

```markdown
# CLAUDE.md — Campaign Brain

## Critical Rules
1. Always use existing lib files (`lib/saleshandy_client.py`, `lib/email_enricher.py`) — do NOT rewrite them
2. Never display API keys in output or conversations
3. Sequences MUST be pre-created in the Saleshandy UI before importing prospects
4. Use Python for all scoring and data manipulation
5. Save all outputs to the `outputs/` folder as CSV
6. Read strategy docs BEFORE executing any step

## Strategy Documents
- `scoring-criteria.md` → ICP scoring rubric (read before scoring)
- `copy-framework.md` → Email copy rules (read before writing sequences)
- `saleshandy-api-docs.md` → API reference (read before API calls)

## Available Tools

### 1. Company Scoring
- Read `scoring-criteria.md` for dimensions and thresholds
- Input: `outputs/companies.csv`
- Process: Python script, score each company across all dimensions
- Output: `outputs/companies-scored.csv` with tier assignments

### 2. Find Decision-Makers
- Use Saleshandy Lead Finder (UI) to search their 800M+ contact database
- Filter by job titles: CEO, Founder, Creative Director, Head of Post-Production
- Export contacts as CSV with verified emails
- Add them to your `companies.csv` before running the pipeline

### 3. Email Copy Generation
- Read `copy-framework.md` for tone, structure, and frameworks
- Generate sequences using Saleshandy merge tags: {{First Name}}, {{Company}}, {{Job Title}}
- Create A/B variants for subject lines
- Output: `outputs/email-sequences.md`

### 4. Campaign Deployment
- Import prospects into pre-created Saleshandy sequences via API
- Map CSV columns to Saleshandy fields
- Activate sequence after import confirmation
- Output: Deployment confirmation with prospect count

### 5. Analytics & Reporting
- Pull campaign stats via Saleshandy API
- Report: open rates, reply rates, bounce rates, top-performing sequences
- Output: `outputs/campaign-report.md`

## Pipeline Order
1. Score companies → `outputs/companies-scored.csv`
2. Find decision-makers via Saleshandy Lead Finder (UI) → add to CSV
3. Generate copy → `outputs/email-sequences.md`
4. Deploy campaign → Live in Saleshandy
5. Pull analytics → `outputs/campaign-report.md`

## Environment
- API keys are in `.env` — load with `python-dotenv`
- All Python scripts should include error handling
- Log actions to `outputs/pipeline-log.md`
```

---

## 🎯 Step 2 — Define Your ICP Scoring Criteria

Your ICP (Ideal Customer Profile) scoring criteria is how Claude decides which companies to prioritize. Every company gets scored across multiple dimensions and assigned a tier.

### Blank Template

```markdown
# ICP Scoring Criteria

## Scoring Dimensions

### Industry Fit (0-25 points)
- 25: [Your ideal industry]
- 15: [Adjacent industry]
- 5: [Somewhat relevant]
- 0: [Not relevant]

### Company Size (0-20 points)
- 20: [Sweet spot employee range]
- 10: [Acceptable range]
- 0: [Too small or too big]

### Funding Stage (0-15 points)
- 15: [Ideal stage]
- 10: [Acceptable stage]
- 0: [Wrong stage]

### Technology Stack (0-15 points)
- 15: Uses tools that integrate with your product
- 10: Uses adjacent tools
- 0: No relevant tech signals

### Geography (0-10 points)
- 10: [Primary market]
- 5: [Secondary market]
- 0: [Outside target]

### Growth Signals (0-15 points)
- 15: Hiring for relevant roles + recent funding
- 10: One growth signal present
- 0: No signals

## Tier Thresholds
- **Tier 1:** 75-100 (highest priority — deploy immediately)
- **Tier 2:** 50-74 (strong fit — include in campaigns)
- **Tier 3:** 25-49 (worth a shot — lower priority)
- **Disqualified:** <25 (skip)

## Target Personas
- Primary: [Job Title 1], [Job Title 2]
- Secondary: [Job Title 3], [Job Title 4]
```

### Filled-In Example (B2B SaaS Selling a Sales Tool)

```markdown
# ICP Scoring Criteria

## Scoring Dimensions

### Industry Fit (0-25 points)
- 25: SaaS, Software, Technology
- 15: Marketing Agency, Consulting, Professional Services
- 5: E-commerce, FinTech
- 0: Everything else

### Company Size (0-20 points)
- 20: 50-500 employees (sweet spot)
- 10: 20-49 or 501-1000 employees
- 0: <20 or >1000 employees

### Funding Stage (0-15 points)
- 15: Series A or Series B
- 10: Seed or Series C
- 5: Bootstrapped with >$1M ARR
- 0: Pre-revenue or PE-backed

### Technology Stack (0-15 points)
- 15: Uses HubSpot, Salesforce, or Outreach
- 10: Uses any CRM
- 0: No CRM detected

### Geography (0-10 points)
- 10: United States, United Kingdom, Canada
- 5: Western Europe, Australia
- 0: Outside target markets

### Growth Signals (0-15 points)
- 15: Hiring SDRs/AEs + raised funding in last 6 months
- 10: Hiring sales roles OR recent funding
- 5: Growing headcount (10%+ in 6 months)
- 0: No growth signals

## Tier Thresholds
- **Tier 1:** 75-100 → Immediate outreach
- **Tier 2:** 50-74 → Strong fit, include in campaign
- **Tier 3:** 25-49 → Lower priority batch
- **Disqualified:** <25 → Skip

## Target Personas
- Primary: VP of Sales, Head of Sales, Sales Director
- Secondary: CRO, CEO (at companies <100 employees), Head of Growth
```

---

## 📧 Step 3 — Build Your Copy Framework

Your copy framework gives Claude the rules for writing emails. Without it, you get generic AI slop. With it, you get emails that sound like you.

> 💡 **Golden rules:** Under 100 words per email. Conversational tone. One CTA per email. Use [Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan) merge tags for personalization.

---

### Framework 1: Direct Value Prop (4 Steps)

**Step 1 — The Opener** (Send Day 1)

- **Subject A:** `Quick question about {{Company}}'s outbound`
- **Subject B:** `{{First Name}}, noticed something about {{Company}}`

```
Hey {{First Name}},

Saw that {{Company}} is scaling the sales team — congrats on the growth.

Most teams at your stage waste 10+ hours/week on manual prospecting. We built a system that cuts that to under 1 hour using AI.

Would it make sense to show you how it works? Takes 15 min.

Cheers,
[Your Name]
```

**Step 2 — The Nudge** (Send Day 3)

```
Hey {{First Name}},

Following up on my last note. Didn't want it to get buried.

Here's the short version: we help {{Job Title}}s at companies like {{Company}} automate their entire outbound pipeline — from finding prospects to sending personalized sequences.

Worth a quick look?

[Your Name]
```

**Step 3 — The Value Add** (Send Day 7)

```
{{First Name}},

I put together a quick breakdown of how teams similar to {{Company}} are running outbound in 2025.

The biggest shift: AI handles the research and personalization, humans handle the conversations.

Happy to share the playbook if you're interested. No strings.

[Your Name]
```

**Step 4 — The Breakup** (Send Day 14)

```
Hey {{First Name}},

I'll keep this short — I've reached out a few times and totally understand if the timing isn't right.

If outbound becomes a priority for {{Company}} down the line, I'm an easy Google away.

Wishing you and the team a great quarter 🙌

[Your Name]
```

---

### Framework 2: Problem-Agitate-Solve (4 Steps)

**Step 1 — The Problem** (Send Day 1)

- **Subject A:** `{{Company}}'s outbound — manual or automated?`
- **Subject B:** `The #1 bottleneck for {{Job Title}}s right now`

```
Hey {{First Name}},

Here's what I keep hearing from {{Job Title}}s at companies like {{Company}}:

"We know outbound works, but it takes too long to do well."

Manual prospecting, generic emails, no personalization at scale — sound familiar?

We solved this. Want me to show you how?

[Your Name]
```

**Step 2 — The Agitate** (Send Day 3)

```
{{First Name}},

Quick math: if your team spends 2 hours/day on prospecting, that's 500+ hours/year.

Now imagine those hours going into actual selling instead.

That's exactly what our system does for teams like {{Company}}. AI handles the grunt work, your reps handle the conversations.

15 min to show you?

[Your Name]
```

**Step 3 — The Proof** (Send Day 7)

```
Hey {{First Name}},

One of our clients went from 200 to 1,400 qualified prospects/month after switching to AI-powered outbound.

Same team size. Same budget. Just smarter tools.

I think {{Company}} could see similar results. Want me to walk you through what they did?

[Your Name]
```

**Step 4 — The Breakup** (Send Day 14)

```
{{First Name}},

Last note from me — I promise.

If automating outbound for {{Company}} ever moves up the priority list, just reply to this thread. I'll be here.

All the best,
[Your Name]
```

---

### Framework 3: Social Proof (4 Steps)

**Step 1 — The Name Drop** (Send Day 1)

- **Subject A:** `How [Similar Company] 3x'd their pipeline`
- **Subject B:** `{{First Name}} — this worked for [Similar Company]`

```
Hey {{First Name}},

We recently helped [Similar Company] go from 50 to 200 booked meetings/month — without adding headcount.

The secret? AI-powered outbound that handles prospecting, personalization, and follow-ups automatically.

Given {{Company}}'s growth, I think you'd find this interesting. 15 min worth of your time?

[Your Name]
```

**Step 2 — The Metric** (Send Day 3)

```
{{First Name}},

Quick follow-up with a number that might interest you:

Teams using our approach see a 3-5x increase in reply rates compared to traditional outbound.

The difference? Every email is researched and personalized by AI before it sends.

Happy to show you a live demo for {{Company}}.

[Your Name]
```

**Step 3 — The Case Study** (Send Day 7)

```
Hey {{First Name}},

Just published a breakdown of how a 10-person sales team generated $2M+ pipeline in 90 days using AI outbound.

Thought it might be relevant for {{Company}} given your current growth phase.

Want me to send it over?

[Your Name]
```

**Step 4 — The Breakup** (Send Day 14)

```
{{First Name}},

Closing the loop here. Totally understand if now's not the right time.

If {{Company}} ever needs help scaling outbound without scaling headcount, you know where to find me.

Cheers,
[Your Name]
```

---

## 🔑 Step 4 — Set Up Your API Keys

Create a `.env` file in your project root:

```
SALESHANDY_API_KEY=your_key_here
```

### Where to Get Your Saleshandy API Key

1. Log in to [Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan)
2. Go to **Settings** → **API Settings** (under Company Settings)
3. Generate a new API key
4. Copy it into your `.env` file

> ⚠️ **Never share your API key publicly.** The `.env` file is gitignored by default. Keep it that way.

---

## 📊 Step 5 — Prepare Your Company List

Create `outputs/companies.csv` with this format:

```csv
name,domain,industry,employee_count,funding_stage,annual_revenue,technologies
Acme Corp,acmecorp.com,SaaS,150,Series A,5000000,HubSpot;Slack;AWS
Widget Inc,widget.io,Technology,80,Seed,1500000,Salesforce;Jira
```

### Where to Source Companies

- **[Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan) Lead Finder** — 800M+ contacts. Search by industry, company size, tech stack, location. This is the fastest way.
- **[Clay](https://clay.com?via=566505)** — Build enrichment workflows that pull from 50+ data sources and output clean CSVs.
- **LinkedIn Sales Navigator** — Export saved lists, then enrich with Clay or Saleshandy.
- **[Bright Data](https://get.brightdata.com/manthan)** — Scrape company directories, review sites, industry lists at scale.
- **[Apify](https://www.apify.com?fpr=f7e9d)** — Pre-built scrapers for LinkedIn, G2, Capterra, Crunchbase, and more.

> 💡 **Pro tip:** Start with 50 companies, not 500. Validate your scoring and copy first, then scale.

---

## ⚡ Step 6 — The Pipeline (How It All Works)

This is the full flow from raw company list to live campaign:

```
companies.csv (your company list)
       │
       ▼
[1] SCORE → reads scoring-criteria.md → Python script → companies-scored.csv
       │
       ▼
[2] FIND CONTACTS → Saleshandy Lead Finder (UI) → export verified emails
       │
       ▼
[3] WRITE COPY → reads copy-framework.md → generates personalized sequences
       │
       ▼
[4] DEPLOY → imports prospects into Saleshandy sequence → campaign goes live 🚀
       │
       ▼
[5] ANALYZE → pulls campaign stats → optimizes for next batch
```

> 🧠 **Each step is a single prompt.** You type one sentence, Claude reads the right docs, runs the right scripts, and outputs the result. That's the power of the brain file.

---

## 💬 Step 7 — Example Prompts

These are the exact prompts you type into Claude at each step:

| Step | What to Type |
|------|-------------|
| **Score** | "Score the companies in companies.csv against our ICP criteria" |
| **Enrich** | "Enrich the Tier 1 and Tier 2 companies and get verified work emails for our target personas" |
| **Write Copy** | "Write a 4-step cold email sequence using Framework 1 from our copy framework" |
| **Deploy** | "Import all Tier 1 prospects into our Saleshandy sequence and activate it" |
| **Analytics** | "Pull campaign analytics and show me open rates, reply rates, and top-performing subject lines" |
| **Optimize** | "Based on the analytics, which subject lines and frameworks should we double down on?" |

> 💡 **Bonus prompt:** "Create a new scoring criteria focused on [industry] companies that use [technology] and have raised funding in the last 12 months"

---

## 🔌 What is MCP? (Bonus)

**MCP = Model Context Protocol**

Think of it as **USB-C for AI tools**. It lets AI assistants (like Claude) plug directly into platforms like [Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan) — reading data, creating sequences, importing contacts — all through a standardized connection.

### How to Set Up Saleshandy MCP

Add this to your Claude Desktop config:

```json
{
  "mcpServers": {
    "saleshandy": {
      "url": "https://mcp.saleshandy.com/mcp",
      "args": ["-y", "the official Saleshandy MCP"],
      "env": {
        "SALESHANDY_API_KEY": "your_key_here"
      }
    }
  }
}
```

**What this enables:**
- Claude can directly search Saleshandy's 800M+ contact database
- Create and manage email sequences from natural language
- Import prospects and activate campaigns
- Pull analytics without leaving the chat

> 🧠 **This is what makes the "just type a prompt" magic possible.** MCP connects Claude to Saleshandy's full API through a simple config file.

### 📖 Full MCP Setup Guide

For the complete step-by-step walkthrough on setting up Saleshandy MCP, check out this guide:

👉 [**How to Set Up Saleshandy MCP — Complete Guide**](https://claude.ai/public/artifacts/e42fb4d2-d578-4ba8-9289-4de2e1463153)

---

## 🤖 Automation & Scaling

Once your first campaign is running, here's how to put it on autopilot:

### Weekly Campaign Automation
- Use [n8n](https://n8n.partnerlinks.io/manthan) or [Make](https://www.make.com/en/register?pc=manthan) to schedule weekly pipeline runs
- Trigger: New companies added to CSV → Auto-score → Auto-enrich → Auto-deploy
- Set up Slack/email notifications for campaign milestones

### Email Infrastructure at Scale
- Set up [Maildoso](https://maildoso.com/?ref=manthan) for unlimited sending accounts
- Proper SPF, DKIM, DMARC configuration out of the box
- Rotate sending accounts automatically for better deliverability

### Multi-Channel Outreach
- Use [Aimfox](https://aimfox.com/?ref=manthan) to run LinkedIn outreach in parallel with email
- Sequence: Email Day 1 → LinkedIn connect Day 2 → Email follow-up Day 3
- 2-3x higher response rates with multi-channel vs. email alone

### CRM & Pipeline Management
- Track everything in [GHL / GoHighLevel](https://www.gohighlevel.com/affiliate-30trial?a=manthanpatel)
- Auto-sync replies from [Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan) to your CRM
- Build follow-up automations for warm leads

---

## 💡 Pro Tips

1. **Always pre-test API calls** before going live — run on 5 contacts first
2. **Start with 50 companies, not 500** — validate your ICP scoring and copy, then scale
3. **A/B test subject lines from day one** — small changes = big impact on open rates
4. **Pull analytics weekly** and feed winning patterns back into your copy framework
5. **Use [Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan)'s built-in email warmup** for 2 weeks before sending cold emails
6. **Keep emails under 100 words** — shorter emails get more replies
7. **Follow up at least 3 times** — most replies come from follow-ups 2-4
8. **Personalize opening lines** based on role + company stage + recent news
9. **Clean your list quarterly** — remove bounces, unsubscribes, and dead domains
10. **Track reply sentiment** — positive replies tell you what messaging resonates

> 🎯 **The 80/20 rule applies:** 80% of your results will come from 20% of your list. Focus on Tier 1 prospects and nail the messaging before scaling.

---

## 🏗️ Advanced: Full GTM Framework

> 🚀 **This is the next level.** The campaign kit above gets you running. This framework turns you into a full GTM operating system — managing paid ads, outbound, RevOps, content, and analytics across multiple clients, all governed by AI agents with human oversight.

---

### The Big Idea: Two Projects, Not One

Instead of one folder for everything, you split into **two separate Claude Code projects**:

| Project | Role | What It Does |
|---------|------|-------------|
| **Governance** | 🧠 The Brain | Rules, audits, SOPs, blueprints. Never does client work — only governs how everything is structured. |
| **GTM Operations** | 💪 The Muscle | Ads, outbound, RevOps, content, analytics. Does the actual work across all channels. |

```
gtm-workspace/
├── governance/        ← The brain. Rules, audits, SOPs, blueprints
├── gtm-operations/    ← The muscle. Ads, outbound, RevOps, content, analytics
├── product/           ← Optional: SaaS tools you're building
└── personal/          ← Optional: private coaching, strategy, reflection
```

---

### The 3-Layer Architecture (Inside GTM Operations)

This is the key insight: **strategy never calls APIs. Bots never make strategic decisions. Client data stays isolated.**

| Layer | Purpose | Rules |
|-------|---------|-------|
| **Knowledge** (Strategy) | Methodology, best practices, frameworks per channel | Read-only. No live API calls. |
| **Execution** (API Bots) | Scripts that actually run campaigns, pull data, push ads | API safety, credential handling, rate limits. |
| **Context** (Client Data) | Per-client ICPs, history, deliverables, account IDs | Complete isolation. No cross-client data leaks. |

Each layer has its own rules file that auto-loads based on which folder you're working in.

---

### The Governance Loop: How It Self-Improves

Working agents **never modify skills, SOPs, or rules directly**. They file proposals. Only governance reviews and applies changes after explicit human approval.

```
Working session → /feedback → governance/registry/proposals/
                                        │
                      /review-proposals ←── weekly
                                        │
                      Human approves (CONFIRM)
                                        │
                      Governance applies changes across all projects
                                        │
                      registry/learnings-log.md ← captured
```

> 🧠 **Why this matters:** This prevents AI drift. Your system gets smarter over time, but only with your explicit sign-off. No rogue changes, no inconsistencies across projects.

---

### Full Folder Structure

#### Governance (The Brain)

```
governance/
├── CLAUDE.md                    ← Master rules. Analyze mode vs Execute mode
├── .claude/
│   ├── rules/
│   │   └── governance-rules.md  ← Cross-project standards, naming, change control
│   ├── skills/
│   │   ├── audit.md             ← Scan all projects against blueprints
│   │   ├── session-end.md       ← Capture learnings before closing
│   │   └── review-proposals.md  ← Review improvement proposals
│   └── agents/
│       ├── researcher.md        ← Cheap model, concise summaries
│       ├── code-reviewer.md     ← Unbiased code review
│       └── qa-tester.md         ← Isolated test execution
├── blueprints/                  ← Expected structure per project
├── sops/                        ← Standard Operating Procedures
│   ├── skill-building-sop.md
│   ├── tool-building-sop.md
│   └── interaction-sop.md
├── registry/                    ← Living state
│   ├── learnings-log.md         ← [PROPOSED] → [APPLIED]
│   ├── remaining-todos.md
│   └── proposals/
├── research/
└── checkpoints/
```

#### GTM Operations (The Muscle)

```
gtm-operations/
├── CLAUDE.md                    ← Three-layer routing
├── MASTER_SOP.md
├── .claude/
│   ├── rules/
│   │   ├── knowledge-rules.md   ← Strategy layer: no API calls
│   │   ├── execution-rules.md   ← Bot layer: API safety
│   │   └── clients-rules.md     ← Client layer: privacy, isolation
│   ├── skills/                  ← One skill per GTM function
│   │   ├── linkedin-ads.md
│   │   ├── meta-ads.md
│   │   ├── google-ads.md
│   │   ├── outbound.md
│   │   ├── revops.md
│   │   ├── content-social.md
│   │   ├── client-audit.md
│   │   └── report-builder.md
│   └── agents/
│       ├── researcher.md
│       └── code-reviewer.md
│
├── ── LAYER 1: KNOWLEDGE ──────────────────────
│
├── knowledge/
│   ├── paid-acquisition/
│   │   ├── linkedin/            ← ABM, audience sizing, bidding, formats
│   │   ├── meta/                ← Audience strategy, creatives, placements
│   │   ├── google/              ← Campaign types, keyword strategy
│   │   └── cross-platform/      ← Personas, budgets, channel selection
│   ├── outbound/
│   │   ├── knowledge-base/
│   │   │   ├── cold-email-frameworks.md
│   │   │   ├── linkedin-outreach.md
│   │   │   ├── icp-signal-mapping.md
│   │   │   └── list-building.md
│   │   └── templates/
│   ├── revops/                  ← Pipeline stages, lead scoring, routing
│   ├── content/                 ← Content pillars, social playbook, SEO
│   └── analytics/               ← KPIs, reporting cadence, dashboards
│
├── ── LAYER 2: EXECUTION ──────────────────────
│
├── execution/
│   ├── linkedin-bot/            ← LinkedIn Marketing API
│   ├── meta-bot/                ← Meta Marketing API
│   ├── google-ads-bot/          ← Google Ads API
│   ├── email-sequencer-bot/     ← Saleshandy API for sequences & warmup
│   ├── linkedin-outreach-bot/   ← LinkedIn outreach automation
│   ├── enrichment-bot/          ← Clay, data enrichment, signals
│   ├── crm-bot/                 ← HubSpot, Salesforce, Attio, GHL
│   ├── reporting-bot/           ← Google Docs, Sheets, PDF generation
│   ├── tag-manager-bot/         ← GTM, conversion tracking
│   └── analytics-bot/           ← Cross-platform unified analytics
│
├── ── LAYER 3: CLIENT CONTEXT ─────────────────
│
└── clients/                     ← Per-client, completely isolated
    ├── client-a/
    │   ├── KNOWLEDGE_BASE.md    ← ICP, positioning, pain points
    │   ├── LINKS.md             ← Dashboards, portals, Slack
    │   ├── changelog.md
    │   ├── docs/
    │   ├── notes/
    │   ├── audits/
    │   └── scripts/
    ├── client-b/
    └── client-c/
```

---

### Skills Per GTM Function

| Skill | What It Covers |
|-------|---------------|
| **LinkedIn Ads** | ABM targeting, audience sizing, bidding strategy, ad formats, campaign management |
| **Meta Ads** | Audience strategy, creative testing, placements, scaling frameworks |
| **Google Ads** | Campaign types, keyword strategy, quality score, search term optimization |
| **Outbound** | Cold email sequences ([Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan)), LinkedIn outreach ([Aimfox](https://aimfox.com/?ref=manthan)), ICP signal mapping, list building |
| **RevOps** | Pipeline stages, lead scoring, lead routing, attribution models, lifecycle definitions |
| **Content** | Content pillars, social playbook, SEO strategy, thought leadership |
| **Analytics** | KPI definitions, reporting cadence, dashboard specs, cross-channel benchmarks |
| **Client Audit** | Full cross-channel account audit framework |
| **Report Builder** | Automated report & document generation |

---

### Knowledge Loading Hierarchy

Not everything loads at once. The system is tiered:

| Tier | What | When |
|------|------|------|
| **Tier 1** | `CLAUDE.md` | Always loaded. Identity + critical rules + routing. |
| **Tier 2** | `.claude/rules/` | Auto-loaded when you touch matching file paths. |
| **Tier 3** | `.claude/skills/` | On-demand. Activated when task matches skill description. |
| **Tier 4** | `knowledge-base/` + SOPs | Referenced by skills. Deep methodology. |
| **Tier 5** | `MEMORY.md` | Always loaded. Quick lookups, evolving state. |

---

### Change Control

| File Type | Examples | How Changes Work |
|-----------|---------|-----------------|
| **Constitutional** | CLAUDE.md, SOPs, governance rules | Requires explicit human `CONFIRM` |
| **Structural** | Skills, rules, agent definitions | Via `/feedback` → governance review cycle |
| **Working** | MEMORY.md, client data, learnings | Freely mutable during sessions |

> 💡 **When to use this framework:** If you're running campaigns for multiple clients, managing paid + organic + outbound across channels, or want your AI system to self-improve with guardrails — this is the architecture. Start with the basic campaign kit above, graduate to this when you're ready to scale.

---

## ❓ FAQ

**"Do I need to know how to code?"**
→ No. Everything is natural language. You type prompts, Claude writes and runs the code. The lib files handle the technical stuff.

**"How much does this cost?"**
→ A [Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan) plan + Claude Pro subscription (~$20/month). That's it for a basic setup. Add tools as you scale.

**"Can this run autonomously?"**
→ Yes. Schedule your pipeline with [n8n](https://n8n.partnerlinks.io/manthan) or [Make](https://www.make.com/en/register?pc=manthan). New companies in → scored, enriched, and deployed automatically.

**"How do I handle deliverability?"**
→ Use [Maildoso](https://maildoso.com/?ref=manthan) for email infrastructure (proper DNS, rotation, warmup). [Saleshandy](https://www.saleshandy.com/?p=v3&via=manthan) handles email warmup and sending reputation.

**"Can I use this for LinkedIn outreach too?"**
→ Absolutely. Pair your email campaigns with [Aimfox](https://aimfox.com/?ref=manthan) for LinkedIn automation. Multi-channel always outperforms single-channel.

**"How do I build my ICP if I don't know my ideal customer?"**
→ Start with your existing customers. What industry are they in? How big are they? What tech do they use? Find the patterns, then build your scoring criteria around them.

**"What if I don't have an existing customer base?"**
→ Pick 10 companies you'd dream of working with. Reverse-engineer what they have in common. That's your starting ICP. Refine as you get data.

**"How many emails should I send per day?"**
→ Start with 30-50/day per sending account. Use [Maildoso](https://maildoso.com/?ref=manthan) to set up multiple accounts and scale to 200-500/day total.

---

# 🏗️ Advanced: Full GTM Framework

> Once you've mastered the basic campaign kit above, this is the next level. This framework structures your entire GTM operation with AI agents.

## The Architecture

Two core projects:

### 1. Governance (The Brain)
Controls rules, audits, SOPs, and the self-improvement loop. Working agents never modify their own rules — they file proposals, governance reviews, human confirms.

```
governance/
├── CLAUDE.md              ← Master rules (Analyze vs Execute mode)
├── blueprints/            ← Project structure templates
├── sops/
│   ├── skill-building-sop.md
│   ├── tool-building-sop.md
│   └── interaction-sop.md
├── registry/
│   ├── learnings-log.md   ← [PROPOSED] → [APPLIED]
│   ├── remaining-todos.md
│   └── proposals/
└── checkpoints/           ← Session state snapshots
```

### 2. GTM Operations (The Muscle)
Where the real work happens. Organized in 3 layers:

**Layer 1: Knowledge** — Strategy, methodology, best practices. Never calls APIs.
**Layer 2: Execution** — API bots and scripts. Never makes strategic decisions.
**Layer 3: Context** — Per-client data, ICPs, history. Isolated per client.

```
gtm-operations/
├── CLAUDE.md              ← 3-layer routing
├── knowledge/
│   ├── paid-acquisition/  ← LinkedIn Ads, Meta Ads, Google Ads
│   ├── outbound/          ← Cold email frameworks, LinkedIn outreach
│   ├── revops/            ← CRM, pipeline, lead scoring, routing
│   ├── content/           ← Social, SEO, thought leadership
│   └── analytics/         ← Attribution, cross-channel measurement
├── execution/
│   ├── linkedin-bot/      ← LinkedIn Marketing API
│   ├── meta-bot/          ← Meta Marketing API
│   ├── google-ads-bot/    ← Google Ads API
│   ├── email-sequencer-bot/ ← Saleshandy API
│   ├── enrichment-bot/    ← Data enrichment
│   ├── crm-bot/           ← CRM integration
│   └── reporting-bot/     ← Automated reports
└── clients/
    └── client-name/       ← Isolated per client
        ├── icp.md
        ├── history/
        └── deliverables/
```

## The Governance Loop

```
Agent does work → Files improvement proposal → Governance reviews → Human confirms (CONFIRM) → Changes applied
```

> 💡 This prevents drift. Agents can't modify their own rules. Every change goes through human approval. Your system stays consistent as it scales.

## Key Principles

- **Strategy never calls APIs.** Knowledge layer is read-only.
- **Bots never make strategic decisions.** Execution layer follows instructions.
- **Client data stays isolated.** No cross-client data leakage.
- **One skill per GTM function.** LinkedIn Ads, Outbound, RevOps, Content, Analytics each get their own skill file.
- **Subagents for research.** Use cheap models for research, review, and QA tasks.

## How to Start

1. Master the basic campaign kit (everything above)
2. Once you're running campaigns consistently, add the governance layer
3. Build out one GTM function at a time (start with outbound)
4. Add clients/ when you start managing multiple accounts

> This is how agencies scale AI-powered GTM to 10+ clients without chaos.

---

## 🔗 Connect With Me

Built by **Manthan Patel** (@LeadGenMan)

- 🔗 [LinkedIn](https://www.linkedin.com/in/leadgenmanthan/)
- 📸 [Instagram](https://www.instagram.com/leadgenman/)
- 🎥 [YouTube](https://www.youtube.com/@LeadGenMan)
- 🎵 [TikTok](https://www.tiktok.com/@leadgenmanthan)

---

> 🚀 **You now have the complete system.** Stop watching tutorials. Open Claude, paste the brain file, load your companies, and type your first prompt. Your AI-powered outbound machine starts today.
