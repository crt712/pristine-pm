# Lindy Integration Guide — Pristine PM + Pristine Clean Club

This file is the authoritative reference for how Lindy should understand and interact with this GitHub repository. Read this before reading any other file.

---

## 1. What This Repository Is

This is the **project state repository** for **Pristine Clean Club**, a premium eco-smart fabric care brand operated by **Cole Totton** under **ChitoBrands LLC** (Delaware). The brand's tagline is *Clean Without Compromise*. The product system is a three-stage fabric care protocol: Clean → Treat → Finish.

The repository is managed by **Pristine PM** — a custom AI-assisted project management tool that converts plain-language updates into surgical edits to structured markdown files. Every meaningful project decision, status change, blocker, and action item flows through this system.

**Operator:** Cole Totton  
**Entity:** ChitoBrands LLC (EIN: 33-4930671)  
**Platform:** Replit-hosted Node.js/React app, deployed at `https://pristine-pm.replit.app`  
**Telegram bot:** @pristinepmbot (pocket PM assistant, same data)

---

## 2. Repository File Map

```
/
├── LINDY.md                          ← You are here
├── active/
│   └── pristine/
│       ├── STATUS.md                 ← Current project phase, active work, blockers
│       ├── CONTEXT.md                ← Project background, constraints, environment
│       ├── DECISIONS.md              ← Confirmed finalized decisions only
│       ├── WORKING_SESSION.md        ← Active focus, current task, session notes
│       ├── ACTION_PLAN.md            ← Milestone roadmap and execution plan
│       ├── RISKS.md                  ← Active risks and mitigation status
│       ├── OPEN_QUESTIONS.md         ← Unresolved questions needing answers
│       ├── FOUNDERS_CHEAT_SHEET.md   ← Master business reference (entity, legal, contacts, strategy)
│       ├── PRODUCT.md                ← Canonical fragrance library and product lineup
│       ├── FORMULATION.md            ← Chemistry specs, lab-track status, materials
│       ├── FOUNDER_BRIEF.md          ← Human-readable digest for stakeholders
│       ├── data/
│       │   ├── contacts.json         ← Live export of the full contacts database
│       │   └── tasks.json            ← Live export of the full tasks/actions database
│       └── artifacts/
│           └── active-insights.md    ← High-signal learnings, not yet promoted to canon
```

---

## 3. The Four Canon Files (Current Project State)

These four files are the **single source of truth** for what is happening right now. They are updated via the Pristine PM app and committed to this repo on every change.

| File | What it tracks | When to read it |
|------|---------------|-----------------|
| `STATUS.md` | Current phase, active work items, blockers, next steps | Always — highest signal file |
| `CONTEXT.md` | Project facts, constraints, background, environment | When you need "why" context |
| `DECISIONS.md` | Confirmed, finalized decisions with rationale | When verifying something was decided |
| `WORKING_SESSION.md` | What Cole is focused on right now, current blocker, live session notes | For real-time awareness |

**Reading the project state:** Start with `STATUS.md`. If blockers exist, treat them as the highest-priority signal. `WORKING_SESSION.md` reflects the most recent activity.

---

## 4. Reference Files (Static Business Data)

These files change infrequently. They define the permanent factual anchors of the business.

| File | What it contains |
|------|-----------------|
| `FOUNDERS_CHEAT_SHEET.md` | Entity details, EIN, trademark, legal summary, warehouse address, digital accounts, key contacts |
| `PRODUCT.md` | Fragrance library (Fleur Blanche, Linen Noir, Pure Haven + others), SKU structure, product system |
| `FORMULATION.md` | Chemistry specs, QCMC/CMCS material summaries, lab track status, concentration targets |
| `ACTION_PLAN.md` | Execution milestones and roadmap |
| `RISKS.md` | Active risks with likelihood and mitigation notes |
| `OPEN_QUESTIONS.md` | Unresolved strategic and operational questions |

---

## 5. Live Data Files (Database Exports)

These JSON files are automatically exported from the live PostgreSQL database and committed to this repo whenever data changes. They are re-exported on every server startup.

### `data/contacts.json`

Full export of the contacts database. Updated whenever a contact is created, edited, or deleted.

**Schema:**
```json
{
  "exported_at": "ISO timestamp",
  "count": 39,
  "contacts": [
    {
      "id": 1,
      "name": "Full Name",
      "email": "primary@email.com",
      "email_secondary": "",
      "phone_mobile": "",
      "phone_office": "",
      "company": "Company Name",
      "role_title": "Title",
      "introduced_by": "Who made the intro",
      "category": "Sourcing/Supply | Legal | Finance | Testing/Labs | Manufacturing | Advisor | ...",
      "context": "Background notes on how this person is relevant",
      "relationship_status": "Active | Warm | Cold | New | ...",
      "last_contact_date": "YYYY-MM-DD or null",
      "next_step": "What needs to happen next with this contact",
      "notes": "Freeform notes",
      "created_at": "ISO timestamp",
      "updated_at": "ISO timestamp"
    }
  ]
}
```

**Key contact categories:** Sourcing/Supply, Legal, Finance, Testing/Labs, Manufacturing, Advisor, Investor, Retail, Media/PR, Technology.

### `data/tasks.json`

Full export of the tasks/actions database. Updated whenever a task is created, edited, or deleted.

**Schema:**
```json
{
  "exported_at": "ISO timestamp",
  "count": 4,
  "tasks": [
    {
      "id": 1,
      "title": "Task description",
      "status": "active | complete | blocked | on-hold",
      "priority": "high | normal | low",
      "owner": "Person responsible",
      "blocked_by": "What is blocking this task",
      "notes": "Additional context",
      "source": "manual | telegram | email | ...",
      "created_at": "ISO timestamp",
      "updated_at": "ISO timestamp",
      "completed_at": "ISO timestamp or null"
    }
  ]
}
```

---

## 6. Authentication

All API calls require an API key. Pass it in either of these headers:

```
Authorization: Bearer <LINDY_API_KEY>
```
or
```
X-Api-Key: <LINDY_API_KEY>
```

The key is stored in Replit under the environment variable `LINDY_API_KEY`. Cole can retrieve the value from the Replit Secrets panel and paste it into Lindy's HTTP action configuration.

---

## 7. API Endpoints Lindy Can Call

Base URL: `https://pristine-pm.replit.app`

All endpoints that read or write project data are available. Key endpoints:

### Read project state
```
GET /api/state
```
Returns all four canon files as JSON. This is the fastest way to get the full current project state in one call.

### Read contacts
```
GET /api/contacts
GET /api/contacts?search=Ron
GET /api/contacts?category=Sourcing/Supply
```

### Create a contact
```
POST /api/contacts
Content-Type: application/json

{
  "name": "Full Name",           (required)
  "email": "email@domain.com",
  "company": "Company",
  "role_title": "Title",
  "category": "Sourcing/Supply",
  "relationship_status": "New",
  "next_step": "Follow up next week",
  "notes": "Met at trade show"
}
```

### Update a contact
```
PUT /api/contacts/:id
Content-Type: application/json
(same fields as POST)
```

### Read tasks
```
GET /api/actions
GET /api/actions?status=active
GET /api/actions?priority=high
```

### Create a task
```
POST /api/actions
Content-Type: application/json

{
  "title": "Task description",   (required)
  "status": "active",
  "priority": "normal",
  "owner": "Cole",
  "notes": "Context"
}
```

### Update a task
```
PUT /api/actions/:id
Content-Type: application/json

{
  "status": "complete",
  "notes": "Done — shipped samples"
}
```

### Force a DB sync to repo
```
POST /api/sync-db
Content-Type: application/json

{ "tables": ["contacts", "tasks"] }
```

### Propose a canon update (AI-generated)
```
POST /api/propose
Content-Type: application/json

{
  "update": "Plain language description of what changed or happened"
}
```
Returns a proposed set of edits to the canon files with rationale. Does not apply them — Cole reviews first.

### Daily briefing
```
POST /api/brief
```
Returns an operator-grade briefing synthesized from current canon state.

---

## 8. Telegram Bot Commands

The Pristine PM Telegram bot (@pristinepmbot) gives Cole mobile access to the same system. Lindy can be aware of these so it understands the operator's natural workflow:

| Command | What it does |
|---------|-------------|
| `/brief` | Full daily status briefing |
| `/focus` | Current priority focus areas |
| `/blockers` | Active blockers |
| `/pending` | Items needing attention |
| `/contacts <name>` | Look up a contact from the live DB |
| `/task <text>` | Capture a new task |
| `/blocker <text>` | Log a blocker |
| `/decision <text>` | Record a decision for review |
| `/suggest` | AI action suggestions |
| `/draft <topic>` | Draft social content |

Natural language also works — Cole can just type "who is our contact at Belle Aire?" or "what's blocking us right now?" and the bot answers from live data.

---

## 9. How Project Updates Work

Cole does not edit these markdown files directly. The workflow is:

1. Cole describes what happened in plain English via the Pristine PM web app or Telegram
2. The AI proposes minimal, surgical edits to the relevant canon file(s)
3. Cole reviews the proposed diff and applies or rejects
4. On apply: files are updated on disk, committed, and pushed to this repo
5. The `data/contacts.json` and `data/tasks.json` files sync automatically on any contact or task change

**What to watch for in commits:**
- Commit messages from `Pristine PM:` prefix → canon file updates applied by Cole
- Commit messages from `data-sync:` prefix → automated database exports (contacts or tasks changed)
- Commit messages from `startup-sync` → server restarted, DB exported to refresh files

---

## 10. Key Relationships and Contacts

Top contacts for context (from contacts database):

| Role | Company / Name | Relevance |
|------|---------------|-----------|
| Fragrance supplier | Belle Aire Creations | Cloning Pristine's bespoke fragrances for scale |
| Fragrance developer | Ron Knight | Developed the original fragrance library |
| Contract manufacturer | Beauty Private Label LLC | Primary manufacturing partner |
| Sustainable materials | Insectta | Chitin-based sustainable ingredient sourcing |
| IP / Legal | TBD | Trademark and patent on file |

For full contact list with emails and phone numbers, read `data/contacts.json`.

---

## 11. Suggested Lindy Automations

Based on how Cole uses this system, the most valuable automations Lindy could run:

| Trigger | Suggested Action |
|---------|-----------------|
| New commit to `STATUS.md` with a new blocker | Notify Cole via preferred channel with blocker summary |
| `next_step` date on a contact passes | Remind Cole to follow up with that contact |
| Task status changes to `complete` | Log to a summary or notify relevant stakeholder |
| New contact added (via `data/contacts.json` change) | Enrich with LinkedIn or web info if available |
| `OPEN_QUESTIONS.md` is updated | Surface the new questions in a daily digest |
| Weekly cadence | Pull `STATUS.md` + `DECISIONS.md` diff, summarize what changed this week |
| `WORKING_SESSION.md` has a blocker open for >48h | Flag as stale, prompt Cole to update or escalate |

---

## 12. Important Rules for Lindy

- **Never directly edit `STATUS.md`, `CONTEXT.md`, `DECISIONS.md`, or `WORKING_SESSION.md`** — these are managed exclusively through the Pristine PM propose/apply workflow to maintain integrity and audit trail
- **`data/contacts.json` and `data/tasks.json` are read-only exports** — to create or update contacts/tasks, use the API endpoints listed in Section 7
- **`DECISIONS.md` only contains finalized, confirmed decisions** — do not interpret open questions or working notes as decisions
- **The `FOUNDERS_CHEAT_SHEET.md` is the authoritative source** for entity details, legal info, and business identity — use it, not any other file, for those facts
