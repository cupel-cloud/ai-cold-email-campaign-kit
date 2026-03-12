# Saleshandy API Documentation

> Source: `https://open-api.saleshandy.com/api-doc/` (OpenAPI 3.0)

## Base URL
```
https://open-api.saleshandy.com/v1
```

## Authentication
All requests require the `x-api-key` header:
```
x-api-key: your_api_key_here
```
Get your API key from: **Settings → API Settings** (in company settings)

---

## Endpoints Overview

### 📋 Sequences

#### GET `/v1/sequences`
Get all sequences and their steps.

**Query params:**
| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `sequenceName` | string | — | Search by sequence title |
| `pageSize` | number | 100 | Items per page (max 1000) |
| `page` | number | 1 | Page number |
| `sort` | string | — | `ASC` or `DESC` |
| `sortBy` | string | — | `sequence.createdAt` or `sequence.title` |
| `clientIds` | string | — | Filter by client IDs |

**Response:**
```json
{
  "message": "Successfully fetched all sequences and their associated steps",
  "payload": [
    {
      "id": "JA5YdAr9wy",
      "title": "Sample Sequence",
      "active": true,
      "steps": [
        { "id": "bwOLEx4l8G", "name": "Step 1" },
        { "id": "zDkLwJJlVG", "name": "Step 2" }
      ],
      "client": {
        "id": "g9bzZq4aMd",
        "companyName": "Demo",
        "firstName": "John",
        "lastName": "Doe"
      }
    }
  ]
}
```

---

#### POST `/v1/sequences/status`
Activate or pause sequences.

**Body:**
```json
{
  "sequenceIds": ["2dP27N0gZ4", "2dP27NugZ3"],
  "status": "resume"  // or "pause"
}
```

---

#### GET `/v1/sequences/{sequenceId}/steps/{stepId}`
Get sequence step variants (A/B test versions).

---

#### PATCH `/v1/sequences/update-prospect-outcome`
Update prospect outcome in a sequence.

**Body:** `UpdateProspectOutcomeDto`

---

### 👥 Prospects

#### POST `/v1/prospects/import`
Import prospects (using field IDs).

**Body:**
```json
{
  "prospectList": [
    {
      "fields": [
        { "id": "0VOLRwYe82", "value": "john" },
        { "id": "pRjY60lBPa", "value": "smith" },
        { "id": "KNz9Zgl7R3", "value": "johnsmith@gmail.com" }
      ]
    }
  ],
  "stepId": "0VOLRwYe82",
  "verifyProspects": true,
  "conflictAction": "overwrite"
}
```
**conflictAction options:** `overwrite`, `noUpdate`, `addMissingFields`

**Status codes in response:**
| Code | Meaning |
|------|---------|
| 10000 | Success |
| 10100 | Invalid Email |
| 10200 | Prospect already in another step of same sequence |
| 10600 | Duplicate prospect |
| 10700 | Bounced prospect |
| 10800 | Unsubscribed prospect |
| 12000 | Prospect active in another sequence |

---

#### POST `/v1/prospects/import-with-field-name` ⭐ RECOMMENDED
Import prospects using human-readable field names. Supports up to **100,000 prospects** (90MB max).

**Body:**
```json
{
  "prospectList": [
    {
      "First Name": "Jon",
      "Last Name": "Smith",
      "Email": "johnsmith@example.com",
      "Company": "ABC",
      "Country": "USA",
      "Phone Number": "1234567890",
      "Job Title": "Procurement Manager"
    }
  ],
  "verifyProspects": true,
  "conflictAction": "addMissingFields",
  "tags": ["tag1", "tag2"]
}
```

**Response:**
```json
{
  "message": "Prospect import has started successfully...",
  "payload": { "requestId": "129a184" }
}
```

---

#### POST `/v1/sequences/prospects/import-with-field-name` ⭐ KEY ENDPOINT
Import prospects directly into a sequence step.

**Body:**
```json
{
  "prospectList": [
    {
      "First Name": "Jon",
      "Last Name": "Smith",
      "Email": "johnsmith@example.com",
      "Company": "ABC",
      "Job Title": "VP Sales"
    }
  ],
  "stepId": "bwOLEx4l8G",
  "verifyProspects": true,
  "conflictAction": "addMissingFields",
  "tags": ["campaign-q1", "tier-1"]
}
```

---

#### GET `/v1/prospects/import-status/{requestId}`
Check import job status.

**Response:**
```json
{
  "payload": {
    "isCompleted": true,
    "failedProspectsURL": "https://example.com/import-prospect/file?..."
  }
}
```

---

#### POST `/v1/prospects/verification-status`
Check email verification status.

**Body:**
```json
{ "emails": ["john@example.com", "jane@example.com"] }
```

**Verification statuses:** `valid`, `pending`, `skip`, `inProgress`, `risky`, `bad`

---

#### GET `/v1/prospects`
Fetch all prospects with pagination.

---

#### PATCH `/v1/prospects/status`
Change prospect status: `replied`, `finished`, `paused`, `unsubscribed`, `active`

---

#### GET `/v1/prospects/attribute`
Get prospect long text attribute value by ID.

---

### 📧 Email Accounts

#### POST `/v1/email-accounts`
Fetch all email accounts.

#### POST `/v1/email-accounts/connect`
Connect new sending email accounts with SMTP/IMAP settings.

#### POST `/v1/email-accounts/reconnect`
Reconnect existing email accounts.

#### POST `/v1/email-accounts/bulk-update`
Bulk update email account settings.

#### GET `/v1/email-accounts/connect/status/{requestId}`
Check email account connection status.

---

#### POST `/v1/sequences/{sequenceId}/steps` ⭐ CREATE STEPS
Create a new step with email content in a sequence.

**Body:**
```json
{
  "type": 1,
  "absoluteDays": 1,
  "variants": [
    {
      "payload": {
        "subject": "{{First Name}}, quick question",
        "content": "<div>Hi {{First Name}},</div><div>Email body here as HTML.</div>",
        "preheader": "Optional preheader text"
      }
    }
  ]
}
```

**Notes:**
- `type`: 1 = email
- `absoluteDays`: day number in sequence (1 = first day)
- Variants support A/B testing (max 2 variants)
- Content must be HTML (use `<div>` tags)
- Use Saleshandy merge tags: `{{First Name}}`, `{{Company}}`, `{{Job Title}}`, etc.

---

#### POST `/v1/sequences/{sequenceId}/email-accounts/add`
Add sending email accounts to a sequence.

**Body:**
```json
{
  "emailAccountIds": ["1Gz3xlNwr9", "ajzR8xpPAq"]
}
```

---

#### POST `/v1/sequences/{sequenceId}/email-accounts/remove`
Remove sending email accounts from a sequence.

---

#### GET `/v1/sequences/{sequenceId}/email-accounts`
List sending email accounts attached to a sequence.

---

### 🏷️ Tags

#### GET `/v1/prospects/tags`
Get all tags.

#### POST `/v1/prospects/tags/assign`
Assign tags to prospects.

#### POST `/v1/prospects/tags/un-assign`
Remove tags from prospects.

---

### 🚫 Blocklist / DNC

#### POST `/v1/blacklist-domains`
Add domain to blacklist.

#### POST `/v1/dnc`
Add domain/email(s) to DNC list.

#### GET `/v1/dnc`
Get all DNC lists.

#### GET `/v1/dnc/{dncListId}`
Get DNC items by list ID.

#### GET `/v1/dnc/item/search`
Search DNC item across all lists.

---

### 🔧 Fields

#### GET `/v1/fields`
Get all custom fields for the user. Use this to map field IDs for the legacy import endpoint.

---

### 👤 Contacts & Sequences

#### POST `/v1/sequences/{sequenceId}/contacts`
Attach existing contacts to a sequence step.

**Body:**
```json
{
  "stepId": "bwOLEx4l8G",
  "contactIds": ["2dP27N0gZ4", "VMw56r9jPb"]
}
```

#### GET `/v1/prospects/{contactId}/minimal/sequences`
Get sequence histories for a contact.

#### GET `/v1/contacts/{contactId}/minimal/sequences`
Same as above (alternate path).

---

### 📊 Analytics

#### POST `/v1/analytics/consolidated-stats`
Comprehensive engagement stats across all selected sequences for a time period.

#### POST `/v1/analytics/stats`
High-level stats for a specific sequence (prospects, emails, engagement).

#### POST `/v1/analytics/emailaccount/stats`
Analytics for a specific email account (engagement, sentiment, performance).

#### POST `/v1/analytics/team/stats`
Team performance report.

---

### 🔍 Enrichment

#### POST `/v1/enrich/contact`
Enrich people by LinkedIn URL or full name + company (max 100).

**Body:**
```json
{
  "linkedin_url": ["https://linkedin.com/in/johndoe"],
  "reveal_phone": true,
  "webhook_url": "https://your-webhook.com/callback",
  "full_name_with_company": [
    { "full_name": "John Doe", "company": "Acme Inc" }
  ]
}
```

#### POST `/v1/enrich/company`
Enrich companies by domain, website, company ID, or LinkedIn URL (max 100).

**Body:**
```json
{
  "company_domain": ["acme.com"],
  "company_website": ["https://acme.com"],
  "linkedin_url": ["https://linkedin.com/company/acme"]
}
```

#### GET `/v1/enrich/status/{requestId}`
Poll enrichment job status.

#### GET `/v1/enrich/status/result/{requestId}`
Get full enrichment results once completed.

#### GET `/v1/enrich/credits`
Check credit balance and usage.

#### GET `/v1/enrich/rate-limits`
Check API rate limit status.

---

### 📥 Unified Inbox

#### GET `/v1/unified-inbox/outcome`
Get outcomes list.

#### GET `/v1/unified-inbox/unread-email-threads-count`
Get unread count.

#### POST `/v1/unified-inbox/emails`
Get emails based on filters.

#### GET `/v1/unified-inbox/emails/{emailThreadId}`
Get full email thread.

#### GET `/v1/unified-inbox/emails/{emailThreadId}/{emailId}`
Get specific email in thread.

#### POST `/v1/unified-inbox/emails/reply`
Reply to an email thread.

---

### ✅ Tasks

#### GET `/v1/tasks` — Get all tasks
#### GET `/v1/tasks/counts` — Get task counts by status
#### GET `/v1/tasks/{taskId}` — Get task details
#### POST `/v1/tasks/{taskId}/snooze` — Snooze a task
#### POST `/v1/tasks/{taskId}/skip` — Skip a task
#### POST `/v1/tasks/{taskId}/complete` — Complete a task
#### PATCH `/v1/tasks/{taskId}/note` — Update task note
#### GET `/v1/tasks/assignee/list` — Get assignee list
#### POST `/v1/tasks/bulk-snooze` — Bulk snooze tasks
#### POST `/v1/tasks/bulk-skip` — Bulk skip tasks
#### GET `/v1/tasks/bulk-status/{bulkActionId}` — Get bulk action status

---

### 👥 Clients

#### GET `/v1/clients`
Fetch all clients.

#### POST `/v1/clients/assign/{resourceType}`
Assign sequence or email account to a client.

---

### 👤 Team

#### GET `/v1/user/team-member-list`
Get team and member list.

---

## Error Codes
| Code | Description |
|------|-------------|
| 1001 | Invalid token |
| 3001 | Failed to fetch sequences |
| 4000 | General failure |
| 5001 | Internal server error |
| 40100 | Sequence not found |
| 40105 | Email account already attached |
| 40200 | Email account not found |
| 40201 | Email account not active |

## Rate Limits
Check current limits: `GET /v1/enrich/rate-limits`

## Merge Tags
Use in email templates: `{{First Name}}`, `{{Last Name}}`, `{{Email}}`, `{{Company}}`, `{{Phone Number}}`, `{{Job Title}}`, `{{Country}}`, `{{City}}`, `{{Signature}}`
Custom fields: `{{Your Custom Field Name}}`
