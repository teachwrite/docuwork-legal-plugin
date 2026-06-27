<!--
CONFIGURATION LOCATION

User-specific configuration for this plugin lives at a version-independent path that survives plugin updates:

  ~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md

Rules for every skill, command, and agent in this plugin:
1. READ configuration from that path. Not from this file.
2. If that file does not exist or still contains [PLACEHOLDER] markers, STOP before doing substantive work. Say: "This plugin needs setup before it can give you useful output. Run /litigation-legal:cold-start-interview — it takes about 10-15 minutes and every command in this plugin depends on it. Without it, outputs will be generic and may not match how your practice actually works." Do NOT proceed with placeholder or default configuration. The only skills that run without setup are /litigation-legal:cold-start-interview itself and any --check-integrations flag.
3. Setup and cold-start-interview WRITE to that path, creating parent directories as needed.
4. On first run after a plugin update, if a populated CLAUDE.md exists at the old cache path
   (~/.claude/plugins/cache/claude-for-legal/litigation-legal/<version>/CLAUDE.md for any version)
   but not at the config path, copy it forward to the config path before proceeding.
5. This file (the one you are reading) is the TEMPLATE. It ships with the plugin and shows the
   structure the config should have. It is replaced on every plugin update. Never write user data here.

**Shared company profile.** Company-level facts (who you are, what you do, where you operate, your risk posture, key people) live in `~/.claude/plugins/config/claude-for-legal/company-profile.md` — one level above this file, shared by all 12 plugins. Read it before this plugin's practice profile. If it doesn't exist, this plugin's setup will create it.
-->

# Litigation Practice Profile
*Written by cold-start on 2026-06-27. If `[PLACEHOLDER]` appears below, run `/litigation-legal:cold-start-interview`.*

This file is the house-level frame every matter is triaged against. Risk calibration, landscape, style. It is persistent across matters. Update whenever the underlying reality changes. Don't paper over drift at the matter level.

---

## Company profile

**Org / legal entity:** DocuWork, Inc., a Delaware corporation
**Industry:** Enterprise document management software
**Public / private / subsidiary:** Private
**Regulated status:** None (not SEC-registered, not HIPAA-covered, not FINRA)
**Core jurisdictions:** California (operations), Delaware (incorporation), N.D. Cal. and C.D. Cal. for federal matters
**Headcount:** 280
**Legal team size:** 4 (GC, two in-house counsel, one litigation support specialist)

### Key internal contacts

| Role | Name | Contact | When to loop in |
|---|---|---|---|
| GC / CLO | Elena Vasquez | evasquez@ | Everything above GC-escalation threshold |
| CFO | Michael Chen | mchen@ | Reserves, settlements above threshold |
| Head of HR | Rachel Kim | rkim@ | All employment matters |
| Head of Comms | Tom Garrett | tgarrett@ | Matters with media or reputational risk |
| CISO | Andy Patel | apatel@ | Data incidents, cyber litigation, regulator inquiries on security |
| Board litigation chair | James Reilly | via GC | Critical matters, disclosure items |

### This counsel

**Counsel:** Julia
**Reports to:** Elena Vasquez, GC

---

## Who's using this

**Role:** Non-lawyer with attorney access
**Attorney contact:** Elena Vasquez, GC

---

## Practice role

**Role:** `in-house`

---

## Side

**Default side:** `both — default defense`

*Defense posture: risk calibration is exposure, reserves, settlement authority, insurance coverage. Demand letters are received and triaged. Discovery is defensive.*

*Skills that branch on side: `demand-draft` / `demand-received`, `subpoena-triage`, `matter-intake` (per-matter), `chronology` (offensive vs defensive framing), `claim-chart` (proving vs disproving elements).*

---

## Available integrations

| Integration | Status | Fallback if unavailable |
|---|---|---|
| DMS (NetDocuments) | ✗ | Matter docs read from local/cloud paths; no DMS-native profiling |
| Document storage (Google Drive) | ✓ | — |
| Gmail | ✓ | — |
| Scheduled-tasks | ✗ | Deadline and hold-refresh reminders run on demand only |
| CLM | ✗ | Contract pulls are manual |

*Re-check: `/litigation-legal:cold-start-interview --check-integrations`*

---

## Outputs

**Work-product header** (prepended to every internal analysis, briefing, triage, or review this plugin generates):

Role is Non-lawyer with attorney access: `RESEARCH NOTES — NOT LEGAL ADVICE — REVIEW WITH A LICENSED ATTORNEY BEFORE ACTING`

---

## Decision posture on subjective legal calls

When a skill in this plugin faces a subjective legal judgment and the answer is uncertain, the skill prefers the recoverable error: flag the specific line with `[review]` inline and note the uncertainty there. Under-flagging is a one-way door; over-flagging is a two-way door an attorney closes in 30 seconds. Default to the two-way door.

---

## 1. Risk calibration

### Risk appetite

**Posture:** Settle nuisance employment claims quickly. Fight contract and IP matters with clear merits. Avoid published opinions. Prefer early mediation on commercial disputes under $2M.

### Severity × likelihood matrix

|                         | Low likelihood   | Medium likelihood | High likelihood |
|-------------------------|------------------|-------------------|-----------------|
| **High severity**       | Monitor          | Priority          | **Critical**    |
| **Medium severity**     | Routine          | Priority          | Priority        |
| **Low severity**        | Routine          | Routine           | Monitor         |

**Severity bands:**
- **High:** exposure >$3M, OR injunctive relief threatening core product, OR regulatory action, OR board-level reputational risk
- **Medium:** $500K–$3M, OR non-core injunctive relief, OR material contract loss
- **Low:** <$500K and no non-monetary relief sought

**Likelihood bands:**
- **High:** adverse outcome more likely than not (>50%) on current evidence
- **Medium:** reasonable chance (20–50%)
- **Low:** unlikely (<20%), but not frivolous

### Materiality thresholds

| Trigger | Threshold | Action |
|---|---|---|
| Reserve required | Probable AND estimable | Loss booked; finance notified |
| GC-only escalation | New matter >$1M, regulator inquiry, class action threat | Brief within 48 hours |

### Settlement authority ladder

| Amount | Approver |
|---|---|
| $0–$75K | Litigation counsel |
| $75K–$500K | GC |
| $500K–$2M | CFO + GC |
| >$2M | Board / audit committee |

### Insurance profile

| Coverage | Carrier | Limits | Retention | Notes |
|---|---|---|---|---|
| D&O | TBD | | | |
| EPL | TBD | | | |
| Cyber | TBD | | | |
| GL / Errors & Omissions | TBD | | | |

**Tendering protocol:** Tender to carrier before any demand goes out on covered matters. Elena Vasquez notifies carrier within 48 hours of matter intake.

---

## 2. Landscape

### Business context

**What we do and why we get sued:** DocuWork provides enterprise document management software to law firms and corporate legal departments. Most disputes arise from contract terminations, data handling obligations under client MSAs, and employment separations. Occasional IP exposure from NPE patent assertions targeting search and indexing features. Because our customers are law firms, reputational risk from published adverse opinions is a primary concern: clients notice.

### Dispute patterns

| Type | Frequency | Typical posture | Notes |
|---|---|---|---|
| Employment | High | Defense | FEHA, wage and hour, wrongful termination |
| Contract / commercial | Medium | Defense / plaintiff | SaaS subscription disputes, data breach indemnity claims from law firm clients |
| IP | Low | Defense | NPE patent assertions on platform features |
| Subpoenas (third-party) | Medium | Neutral | Regulatory requests for documents stored on platform by law firm clients |

### Frequent adversaries

| Counterparty / firm | Matter type | History |
|---|---|---|
| NPE patent asserters | IP | Periodic; settle quickly when claims are weak |
| Former employees | Employment | FEHA, wage and hour |

### Outside counsel bench

| Firm | Lead partner | Matter type | Rate posture | Engagement letter |
|---|---|---|---|---|
| TBD | | Employment | Hourly with monthly budget cap | On file |
| TBD | | IP / patent | Flat fee for early-stage NPE response | On file |

### Frequent fora

**Frequent fora:** N.D. Cal., C.D. Cal., Delaware Chancery, JAMS arbitration (per standard MSA clause with law firm clients)

### Document storage

| Source | Type | Path / access | MCP available? |
|---|---|---|---|
| Google Drive — Legal | Cloud drive | /Legal/Matters/ | Yes |
| Gmail archive | Email | litigation@ mailbox | Yes |
| SharePoint — Contracts | Cloud drive | /Contracts/ | No |

**Default matter folder pattern:** Google Drive → Legal → Matters → {matter-slug}
**Matter documents shared with outside counsel via:** Secure share link

### Conflicts clearance

**Method:** Informal — counsel's own judgment plus GC review
**Who runs it:** Julia (litigation support) flags; Elena Vasquez (GC) clears
**What we check against:** Current customer list, active vendors, board members, ex-employees within 2 years
**Required before intake:** Yes, but intake can proceed in parallel

---

## 3. House style

### Privilege conventions

**Marking:** Privileged & Confidential — Attorney-Client Communication / Attorney Work Product
**Default posture:** Flag anything not clearly non-privileged; attorney reviews and narrows
**Review mechanic:** Inline note on each flagged item
**Auto-flag threshold:** Default — flag anything not clearly non-privileged

### Legal hold

**Template:** Google Drive → Legal → Templates → Legal Hold Notice
**Issuance:** Elena Vasquez (GC) issues; Julia (litigation support) tracks acknowledgments and refresh cadence
**Refresh cadence:** Every 90 days or on any material case development

### Board / audit committee memo

**Format:** Bullet summary, risk table, ask, reserve status, next steps
**Tone:** Plain English, no hedging without a reason, every number has a source
**Cadence:** Quarterly portfolio memo, urgent escalation memos as needed

### Escalation

**Channel:** Elena Vasquez (GC) via email for all matters; urgent matters escalate to phone call; CFO Michael Chen via email only; board via GC
**Subject-line convention:** [LITIGATION — CRITICAL] matter name — one-line summary

### Demand-letter practice

**Insurance tender timing:** Before demand goes out
**Materiality threshold for matter creation:** Any demand over $100K or any cease and desist becomes a formal matter

---

## Seed documents

| Doc | Location / pointer | Notes |
|---|---|---|
| Legal hold template | Google Drive → Legal → Templates → Legal Hold Notice | |
| Outside counsel guidelines | Google Drive → Legal → Templates → OCG | |

---

## Updating this file

Update when:
- Risk appetite or authority shifts change
- Outside counsel bench changes
- New dispute patterns emerge
- Insurance renewals change coverage
- Board reporting format changes

Re-run the full cold-start: `/litigation-legal:cold-start-interview --redo`

---

*Last updated: June 2026*