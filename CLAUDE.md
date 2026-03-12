# CLAUDE.md — Campaign Brain

---

> This file directs the agent through the full outbound campaign flow using AI + Saleshandy.

## Overview
You are building an automated cold email campaign. Follow these steps in order:
1. **Score & Tier Companies** → Identify and rank target companies
2. **Find ICPs** → Use Saleshandy's Lead Finder to find decision-makers at tiered companies
3. **Enrich Contacts** → Get verified emails and enrichment data
4. **Generate Copy** → Write personalized cold email sequences
5. **Push to Saleshandy** → Create campaign via API

---

## Critical Rules
1. Never display API keys in output or conversations
2. Sequences MUST be pre-created in the Saleshandy UI before importing prospects
3. Use Python for all scoring and data manipulation
4. Save all outputs to the `outputs/` folder as CSV
5. Read strategy docs BEFORE executing any step

---

## Step 1: Score & Tier Companies

**Goal:** Build a tiered list of target companies (Tier 1/2/3).

**Instructions:**
1. Ask for the target niche/industry and ideal client profile
2. Use the scoring criteria in `scoring-criteria.md` to evaluate companies
3. Research companies using web search (company size, funding, tech stack, revenue)
4. Output a tiered list

**Data sources:** Web search, Saleshandy Lead Finder, company websites, Crunchbase

---

## Step 2: Find ICPs at Target Companies

**Goal:** Find decision-makers at tiered companies.

**Instructions:**
1. Use Saleshandy's Lead Finder to search for contacts at each tiered company
2. Filter by titles: VP Sales, Head of Growth, Director of Marketing, CEO, Founder, CRO
3. Prioritize contacts with verified emails
4. Store enriched contact data

**Saleshandy Lead Finder:**
- Access via Saleshandy dashboard → Lead Finder tab
- People tab: Search by company, title, seniority, location, and 70+ filters
- Company tab: Search by industry, size, funding, tech stack
- Leads found can be pushed directly into sequences

---

## Step 3: Enrich Contacts

**Goal:** Verify emails and gather additional data points for personalization.

**Instructions:**
1. Use Saleshandy's built-in enrichment API (see `saleshandy-api-docs.md`) to enrich contacts
2. Verify email addresses using Saleshandy's verification endpoint
3. Gather personalization signals: recent posts, company news, tech stack, job changes

**Enrichment via Saleshandy API:**
- `POST /v1/enrich/contact` — Enrich by LinkedIn URL or full name + company
- `POST /v1/enrich/company` — Enrich companies by domain
- `GET /v1/enrich/status/{requestId}` — Poll enrichment job status
- `GET /v1/enrich/status/result/{requestId}` — Get enrichment results

---

## Step 4: Generate Email Copy

**Goal:** Write personalized email sequences for each tier.

**Instructions:**
1. Select frameworks from `copy-frameworks.md` based on the tier:
   - **Tier 1:** Highly personalized, case study or direct value prop framework
   - **Tier 2:** Semi-personalized, problem-agitate-solve framework
   - **Tier 3:** Template-based, social proof framework
2. Use Saleshandy merge tags: `{{First Name}}`, `{{Company}}`, `{{Job Title}}`, etc.
3. Generate 3-4 step sequences (initial + follow-ups)
4. Create A/B variants for subject lines and opening lines

---

## Step 5: Push Campaign to Saleshandy

**Goal:** Create the full campaign in Saleshandy via API.

**Instructions:**
1. **List existing sequences** to check for duplicates: `GET /v1/sequences`
2. **Get sequence steps:** Use an existing sequence or create one in the Saleshandy UI first
3. **Import prospects into sequence:**
   ```
   POST /v1/sequences/prospects/import-with-field-name
   ```
4. **Add sending email accounts** to the sequence:
   ```
   POST /v1/sequences/{sequenceId}/email-accounts/add
   ```
5. **Activate the sequence:**
   ```
   POST /v1/sequences/status
   Body: { "sequenceIds": ["<id>"], "status": "resume" }
   ```

**Full API reference:** See `saleshandy-api-docs.md`

---

## Strategy Documents
- `scoring-criteria.md` → ICP scoring rubric (read before scoring)
- `copy-frameworks.md` → Email copy rules (read before writing sequences)
- `saleshandy-api-docs.md` → API reference (read before API calls)

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

## Merge Tag Reference (Saleshandy)
Standard: `{{First Name}}`, `{{Last Name}}`, `{{Email}}`, `{{Company}}`, `{{Phone Number}}`, `{{Job Title}}`, `{{Country}}`, `{{City}}`
Custom: Any field you create, e.g. `{{Pain Point}}`, `{{Personalized Line}}`, `{{Case Study}}`
Sender: `{{Sender First Name}}`, `{{Sender Last Name}}`, `{{Sender Email}}`, `{{Signature}}`
