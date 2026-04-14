# Multi-Job Workflow — Complete Batch Processing System

## Overview

This file defines the complete workflow for processing multiple job applications simultaneously. Multi-job mode maximizes efficiency by running shared work once (library build, bullet quality, experience discovery) and then processing each job independently using the enriched foundation.

**Core Principle:** Ask once, apply everywhere. The user's experiences don't change between applications — only the framing and emphasis does.

---

## When Multi-Job Mode Activates

Detection is automatic (from SKILL.md detection logic). Confirm with user before starting:

```
"I see you're applying to {N} jobs. Multi-job mode will:

✅ Run library build and bullet quality scan ONCE (not {N} times)
✅ Ask experience discovery questions ONCE, covering gaps across ALL jobs
✅ Process each job individually with full research and tailoring
✅ Let you review all resumes together at the end
✅ Generate cover letters for all jobs in one batch

TIME ESTIMATE:
  {N} jobs sequentially: ~{N × 15} minutes
  Multi-job mode: ~{estimate} minutes
  You save: ~{savings} minutes

Ready to start? I'll need all {N} job descriptions before we begin."
```

---

## PHASE 0 — Batch Initialization

### 0.1 Job Collection

Collect all job descriptions before proceeding:

```
"Please share all {N} job descriptions now.
You can paste them one by one or all at once.

For each job, I'll need:
  - The job description (text or URL)
  - Company name (if not obvious from JD)
  - Any notes you have about this role (optional but useful)

Go ahead whenever you're ready."
```

**As each JD is received, parse immediately:**

```
JOB_BATCH = [
  {
    job_id: "job_1",
    company: "Company Name",
    role: "Job Title",
    jd_text: "full text",
    user_notes: "anything they shared",
    parsed: false,
    status: "pending"
  },
  ...
]
```

### 0.2 Batch Overview

After collecting all JDs, present a structured overview:

```
"Got all {N} jobs. Here's what I'm working with:

JOB BATCH:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
#1  {Company} — {Role}
    Type: {archetype detected}
    Seniority: {level detected}
    
#2  {Company} — {Role}
    Type: {archetype detected}
    Seniority: {level detected}

#3  {Company} — {Role}
    Type: {archetype detected}
    Seniority: {level detected}
...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SIMILARITY ANALYSIS:
  {If similar roles}: Jobs #1, #2, and #3 are all PM roles — 
   discovery will cover shared requirements efficiently.
  
  {If diverse roles}: These are quite different role types.
   Discovery will need to cover {X} distinct gap areas.

Any corrections to the above before I proceed? [Y/N]"
```

### 0.3 Library Initialization

Run Phase 0 from SKILL.md exactly once:

```
"Building resume library... [once for all jobs]
Running Bullet Quality Engine... [once for all jobs]

Library: {N} resumes loaded
Bullets scanned: {N} total
  Strong (85+): {N}
  Good (70-84): {N}
  Weak (<70): {N}

{IF weak bullets found}
Upgrade session before we continue? 
Stronger library = better output for ALL {N} jobs.
(~{time} minutes)"
```

---

## PHASE 1 — Aggregate Gap Analysis

**Goal:** Identify ALL gaps across ALL jobs in one pass, then deduplicate.

### 1.1 Per-Job Quick Parse

For each job, run a fast JD parse (full research comes in Phase 3):

```
For each job in batch:
  Parse must-haves, keywords, archetype, seniority
  Score against library (rough pass, no detailed matching yet)
  Identify gaps: requirements with <60% library coverage
  Tag gap by type: technical / leadership / domain / scale
```

### 1.2 Gap Aggregation & Deduplication

Collect all gaps across all jobs and merge overlapping ones:

```
RAW GAPS (before dedup):
  Job 1: Python, stakeholder management, A/B testing, SQL, scaled product
  Job 2: Python, data analysis, SQL, leadership, product roadmap
  Job 3: SQL, stakeholder management, product strategy, executive comms

DEDUPLICATED GAP MAP:
  UNIVERSAL (all 3 jobs): SQL, stakeholder management
  MAJORITY (2+ jobs): Python, leadership/management
  JOB-SPECIFIC:
    Job 1 only: A/B testing, scaled product experience
    Job 2 only: Data analysis depth
    Job 3 only: Product strategy, executive communication
```

### 1.3 Gap Priority Matrix

Classify every gap for discovery prioritization:

```
GAP PRIORITY MATRIX:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PRIORITY 1 — UNIVERSAL CRITICAL GAPS:
  Appear in ALL jobs as critical requirements
  → Must find or develop during discovery
  → These are the most valuable minutes to spend

PRIORITY 2 — MAJORITY CRITICAL GAPS:
  Appear in 2+ jobs as critical requirements
  → High value to address; improves multiple applications

PRIORITY 3 — JOB-SPECIFIC CRITICAL GAPS:
  Critical for only 1 job but that job is high priority
  → Address during per-job discovery if needed

PRIORITY 4 — IMPORTANT (NOT CRITICAL) GAPS:
  Important but not eliminators across any job
  → Nice to address if time allows

SKIP — BONUS GAPS:
  Only in "bonus/preferred" category
  → Don't spend discovery time on these
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Present to user:**

```
"Here's what we're working with across all {N} jobs:

SHARED GAPS (discovered experience here helps MULTIPLE applications):
  🔴 Critical: {gap 1} — needed for {N} jobs
  🔴 Critical: {gap 2} — needed for {N} jobs
  🟡 Important: {gap 3} — needed for {N} jobs

JOB-SPECIFIC GAPS:
  Job #1 ({Company}): {specific gap}
  Job #2 ({Company}): {specific gap}

TOTAL UNIQUE GAPS TO EXPLORE: {N}
  (vs. {N × individual gaps} if done sequentially)

Starting shared discovery now to cover the most ground first."
```

---

## PHASE 2 — Shared Experience Discovery

**Run once. Covers gaps for ALL jobs.**

### 2.1 Multi-Job Context in Questions

When asking about each gap, reference ALL jobs it affects:

```
STANDARD (single job):
  "Have you worked with SQL?"

MULTI-JOB VERSION:
  "Have you worked with SQL? This comes up in {Company 1}, 
   {Company 2}, and {Company 3} — anything we find here 
   helps all three applications."
```

This reinforces the value of each answer and motivates thorough responses.

### 2.2 Discovery Order

```
STEP 1: Universal Critical gaps (all jobs need these)
STEP 2: Majority Critical gaps (most jobs need these)
STEP 3: Quick sweep of job-specific gaps (per job, brief)
  "For {Company X} specifically — any experience with {specific gap}?
   Just for that one application."
STEP 4: Differentiator questions
  "Any experiences across all these applications that 
   would make you stand out compared to other candidates?"
```

### 2.3 Experience Tagging

Tag every discovered experience with job relevance:

```
DISCOVERED EXPERIENCE:
  "{bullet draft}"
  
  RELEVANT FOR:
  ✅ Job #1 ({Company}) — addresses: {gap}, confidence: {%}
  ✅ Job #2 ({Company}) — addresses: {gap}, confidence: {%}
  ⚠️  Job #3 ({Company}) — partially relevant, confidence: {%}
  ❌ Job #4 ({Company}) — not relevant for this role
```

### 2.4 Library Enrichment

After discovery, update library once — all per-job processing uses enriched library:

```
"Discovery complete.

FOUND: {N} new experiences
  {Experience 1}: relevant for {N} jobs
  {Experience 2}: relevant for {N} jobs
  ...

Library enriched. All {N} job applications will now draw from 
your enhanced experience database.

Proceeding to per-job processing. This is where each 
application gets its own tailoring."
```

---

## PHASE 3 — Per-Job Processing

**Each job gets full, individual treatment using the enriched library.**

### Processing Mode Selection

Before starting per-job work, offer two modes:

```
"Now I'll process each job individually. Two modes available:

INTERACTIVE MODE (recommended for high-priority applications):
  → I'll show you key decisions for each job
  → You review template, matching, and final resume
  → Takes ~10-12 minutes per job
  → Best quality, most control

EXPRESS MODE (for lower-priority applications):
  → I'll make all decisions automatically using best judgment
  → You review only the final resume
  → Takes ~5-6 minutes per job
  → Good quality, less control

Which mode for each job?
(You can mix — e.g., Interactive for top 2, Express for the rest)"
```

### Per-Job Processing Template

For each job, run this sequence:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
JOB #{N}: {Role} at {Company}
Progress: {N}/{total} jobs
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

STEP 1: Full Research (research-prompts.md)
  Company intel, role benchmarking, success profile
  [Interactive: present success profile for review]
  [Express: proceed automatically]

STEP 2: Template Generation (Phase 2 from SKILL.md)
  Role consolidation, title reframing, section structure
  [Interactive: present template for approval]
  [Express: use best judgment, note decisions made]

STEP 3: Assembly (matching-strategies.md)
  Match bullets to template slots using enriched library
  Apply reframings
  [Interactive: show top matches per slot]
  [Express: auto-select best matches, show summary only]

STEP 4: Generation
  MD + DOCX + Report

STEP 5: ATS Simulation (ats-simulation.md)
  Score resume, apply fixes, re-score
  [Both modes: always run, auto-apply critical fixes]

STEP 6: Cover Letter (cover-letter-hook.md)
  [Interactive: present for review]
  [Express: generate automatically, present with resume]

COMPLETE: "Job #{N} done. Moving to Job #{N+1}."
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Cross-Job Differentiation Rules

When processing jobs with similar requirements, ensure resumes are distinct:

```
DIFFERENTIATION PRINCIPLE:
  Same person, same truthful experiences, 
  different emphasis per company's actual need.

TACTICS:
  1. SUMMARY PARAGRAPH: Rewrite for each company
     Job 1 (startup): Lead with "builder" narrative
     Job 2 (enterprise): Lead with "scale + process" narrative
     Job 3 (data-heavy): Lead with "analytical + data-driven" narrative

  2. BULLET SELECTION: Use different bullets for same role when possible
     If Role X has 8 strong bullets but template needs only 5:
     → Pick 5 most relevant for THIS company's archetype
     → A different 5 for the next company if their needs differ

  3. TERMINOLOGY: Apply each company's terminology map separately
     Never bleed Company A's language into Company B's resume

  4. TITLE REFRAMING: May differ per application
     "Data Science Lead" → "ML Engineering Lead" for Job 1
     "Data Science Lead" → "Analytics Manager" for Job 2
     Both truthful, both different emphasis

WHEN TO USE THE SAME BULLET:
  If a bullet directly addresses a CRITICAL requirement 
  for multiple jobs and no better option exists → use it
  But note it was reused and watch for differentiation elsewhere
```

---

## PHASE 4 — Batch Finalization

### 4.1 Batch Summary Presentation

After all jobs are processed, present a comprehensive overview:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BATCH COMPLETE — {N} APPLICATIONS READY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
JOB #1: {Role} at {Company}
  JD Coverage: {%}%
  ATS Score: {score}/100
  Direct Matches: {%}%
  Gaps Remaining: {N}
  Cover Letter: ✅

JOB #2: {Role} at {Company}
  JD Coverage: {%}%
  ATS Score: {score}/100
  Direct Matches: {%}%
  Gaps Remaining: {N}
  Cover Letter: ✅

[... repeat for all jobs]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BATCH STATS:
  Total files created: {N × 3-4 files each}
  New experiences discovered: {N}
  Bullets upgraded in library: {N}
  Average JD coverage: {%}%
  Average ATS score: {score}/100
  
OVERALL QUALITY: {Strong / Good / Moderate}
  {1-sentence assessment}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### 4.2 Comparative Analysis (Stretch Application Flag)

If any application is a stretch, flag it:

```
STRETCH APPLICATION FLAGS:
  
  ⚠️  Job #3 ({Company}) — Stretch Application
      JD Coverage: 61% (vs. 80%+ for other applications)
      Key gaps: {gap 1}, {gap 2}
      
      RECOMMENDATION: Submit, but:
      → Cover letter should directly address the gaps
      → Prepare stronger answers for gap areas in interviews
      → Consider whether there's additional experience to surface
      
      Still worth submitting? Most hiring managers value genuine 
      interest and partial fit over silence. But go in eyes open.
```

### 4.3 Batch Review Process

Two review options:

```
"How would you like to review your {N} applications?

OPTION A — SEQUENTIAL REVIEW:
  I'll walk you through each application one by one.
  You approve, revise, or flag each before moving to the next.
  Best for careful review of every application.
  Time: ~{N × 5} minutes

OPTION B — SPOT CHECK:
  I'll highlight only the decisions I wasn't confident about.
  You review flagged items only; auto-approve the rest.
  Best when you trust the process and want to move fast.
  Time: ~10-15 minutes total

Which approach?"
```

### 4.4 Batch Approval Options

```
For each resume, user can:
  1. APPROVE — save to library, finalize files
  2. REVISE — specify what to change (I'll update and re-present)
  3. HOLD — save files but don't add to library yet
  4. REGENERATE — start this one over with different emphasis

Batch option: "APPROVE ALL REMAINING" — approve all that 
haven't been flagged for revision
```

---

## PHASE 5 — Batch Library Update

After approval, update library efficiently:

```
"Saving all approved applications to your library...

SAVING:
  ✅ {Name}_{Company1}_{Role}_Resume.md
  ✅ {Name}_{Company2}_{Role}_Resume.md
  ...

LIBRARY STATS:
  Previous: {N} resumes
  New total: {N + approved} resumes
  New experiences captured: {N}
  Bullets upgraded: {N}
  
  Your library is now significantly richer.
  The next batch of applications will be faster and better."
```

---

## Incremental Batch Addition

When user wants to add new jobs to an existing batch (from a previous session):

```
"You have an existing batch with {N} completed applications.
Adding {new_N} new jobs.

OPTIONS:
  A. FULL INCREMENTAL: Build on existing discovery
     → Analyze new JDs for NEW gaps only (gaps not covered before)
     → Run short discovery session for new gaps only
     → Process new jobs using enriched library
     
  B. FRESH START: Treat new jobs independently
     → Run full discovery for new jobs
     → No connection to previous batch

RECOMMENDATION: Option A — saves ~{X} minutes and builds 
on existing library enrichment.

Time estimate for adding {new_N} jobs: ~{estimate} minutes"
```

### Incremental Gap Detection

```
NEW JD GAPS — DETECTION LOGIC:

For each new job:
  Extract all requirements
  Check against EXISTING library (original + discoveries from previous session)
  Identify ONLY NEW gaps (not covered by previous discovery)
  
RESULT:
  "For your 2 new jobs, I found {N} new gaps:
   - {gap 1}: not covered by previous discovery
   - {gap 2}: specific to {Company}
   
   Already covered from previous session:
   - {gap X}: ✅ (found in previous discovery)
   - {gap Y}: ✅ (strong library match)
   
   Running short discovery for {N} new gaps only (~{X} minutes)."
```

---

## File Organization for Batch Output

All batch files organized clearly:

```
OUTPUT STRUCTURE:
  batch_{date}/
  ├── {Name}_{Company1}_{Role}_Resume.md
  ├── {Name}_{Company1}_{Role}_Resume.docx
  ├── {Name}_{Company1}_{Role}_CoverLetter.md
  ├── {Name}_{Company1}_{Role}_CoverLetter.docx
  ├── {Name}_{Company1}_{Role}_Report.md
  │
  ├── {Name}_{Company2}_{Role}_Resume.md
  ├── {Name}_{Company2}_{Role}_Resume.docx
  ├── {Name}_{Company2}_{Role}_CoverLetter.md
  ├── {Name}_{Company2}_{Role}_CoverLetter.docx
  ├── {Name}_{Company2}_{Role}_Report.md
  │
  └── BATCH_SUMMARY.md  ← overview of all applications

BATCH_SUMMARY.md contains:
  - Table of all jobs with JD coverage and ATS scores
  - Discovered experiences (shared across all applications)
  - Library upgrade summary
  - Submission priority recommendation
  - Any stretch application flags
```

---

## Submission Priority Recommendation

At the end of the batch, offer a sequenced submission plan:

```
"Based on JD coverage, ATS scores, and fit analysis, 
here's a recommended submission order:

APPLY FIRST (strongest fit, highest probability):
  1. {Company} ({role}) — {%}% coverage, ATS {score}/100
     Why: {specific reason — e.g. "your background aligns with 
          their exact scaling challenge"}

  2. {Company} ({role}) — {%}% coverage, ATS {score}/100
     Why: {specific reason}

APPLY SECOND (good fit, minor gaps):
  3. {Company} ({role}) — {%}% coverage, ATS {score}/100
     Why: {specific reason}

APPLY LAST / CONSIDER CAREFULLY (stretch):
  4. {Company} ({role}) — {%}% coverage, ATS {score}/100
     Why: {honest assessment of fit and what to watch for}

This is a suggestion, not a rule. You may have context 
about these companies that changes the order."
```

---

## Time Estimates by Batch Size

Use these for setting expectations at session start:

```
BATCH SIZE    MULTI-JOB    SEQUENTIAL    SAVINGS
2 jobs        ~25 min       ~30 min       17%
3 jobs        ~38 min       ~45 min       16%
4 jobs        ~48 min       ~60 min       20%
5 jobs        ~55 min       ~75 min       27%
7 jobs        ~70 min       ~105 min      33%
10 jobs       ~90 min       ~150 min      40%

NOTE: Express mode reduces per-job time to ~5-6 min (vs. ~10-12 interactive)
      Express mode for all jobs in a 5-job batch → ~50 min total
      Interactive mode for all → ~70 min total
```
