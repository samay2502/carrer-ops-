---
name: resume-tailoring
description: Use when creating tailored resumes for job applications - researches company/role, creates optimized templates, conducts branching experience discovery to surface undocumented skills, generates professional multi-format resumes and cover letters from user's resume library while maintaining factual integrity. Includes ATS simulation scoring and bullet quality engine.
---

# Resume Tailoring Skill

## Overview

Generates high-quality, tailored resumes optimized for specific job descriptions while maintaining factual integrity. Builds resumes around the holistic person by surfacing undocumented experiences through conversational discovery.

**Core Principle:** Truth-preserving optimization - maximize fit while maintaining factual integrity. Never fabricate experience, but intelligently reframe and emphasize relevant aspects.

**Mission:** A person's ability to get a job should be based on their experiences and capabilities, not on their resume writing skills.

## When to Use

Use this skill when:
- User provides a job description and wants a tailored resume
- User has multiple existing resumes in markdown format
- User wants to optimize their application for a specific role/company
- User needs help surfacing and articulating undocumented experiences
- User wants a cover letter matched to their tailored resume

**DO NOT use for:**
- Generic resume writing from scratch (user needs existing resume library)
- LinkedIn profile optimization (different skill)

## Quick Start

**Required from user:**
1. Job description (text or URL)
2. Resume library location (defaults to `resumes/` in current directory)

**Workflow:**
1. Build library from existing resumes + run Bullet Quality Engine
2. Research company/role
3. Create template (with user checkpoint)
4. Optional: Branching experience discovery
5. Match content with confidence scoring
6. Generate MD + DOCX + PDF + Report
7. **Run ATS Simulation** → apply fixes if needed
8. **Generate Cover Letter** (optional, uses existing research)
9. User review → Optional library update

## Implementation

See supporting files:
- `research-prompts.md` - Structured prompts for company/role research
- `matching-strategies.md` - Content matching algorithms and scoring
- `branching-questions.md` - Experience discovery conversation patterns
- `ats-simulation.md` - ATS scoring, format checks, keyword analysis
- `bullet-quality-engine.md` - Library bullet scoring and upgrade system
- `cover-letter-hook.md` - Cover letter generation from research context

## Workflow Details

### Multi-Job Detection

**Triggers when user provides:**
- Multiple JD URLs (comma or newline separated)
- Phrases: "multiple jobs", "several positions", "batch", "3 jobs"
- List of companies/roles: "Microsoft PM, Google TPM, AWS PM"

**Detection Logic:**

```python
# Pseudo-code
def detect_multi_job(user_input):
    indicators = [
        len(extract_urls(user_input)) > 1,
        any(phrase in user_input.lower() for phrase in
            ["multiple jobs", "several positions", "batch of", "3 jobs", "5 jobs"]),
        count_company_mentions(user_input) > 1
    ]
    return any(indicators)
```

**If detected:**
```
"I see you have multiple job applications. Would you like to use
multi-job mode?

BENEFITS:
- Shared experience discovery (faster - ask questions once for all jobs)
- Batch processing with progress tracking
- Incremental additions (add more jobs later)

TIME COMPARISON (3 similar jobs):
- Sequential single-job: ~45 minutes (15 min × 3)
- Multi-job mode: ~40 minutes (15 min discovery + 8 min per job)

Use multi-job mode? (Y/N)"
```

**If user confirms Y:**
- Use multi-job workflow (see multi-job-workflow.md)

**If user confirms N or single job detected:**
- Use existing single-job workflow (Phase 0 onwards)

**Backward Compatibility:** Single-job workflow completely unchanged.

**Multi-Job Workflow:**

When multi-job mode is activated, see `multi-job-workflow.md` for complete workflow.

**High-Level Multi-Job Process:**

```
┌─────────────────────────────────────────────────────────────┐
│ PHASE 0: Intake & Batch Initialization                      │
│ - Collect 3-5 job descriptions                              │
│ - Initialize batch structure                                │
│ - Run library initialization (once)                         │
│ - Run Bullet Quality Engine (once, covers all jobs)         │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│ PHASE 1: Aggregate Gap Analysis                            │
│ - Extract requirements from all JDs                         │
│ - Cross-reference against library                           │
│ - Build unified gap map (deduplicate)                       │
│ - Prioritize: Critical → Important → Job-specific          │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│ PHASE 2: Shared Experience Discovery                       │
│ - Single branching interview covering ALL gaps              │
│ - Multi-job context for each question                       │
│ - Tag experiences with job relevance                        │
│ - Enrich library with discoveries                           │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│ PHASE 3: Per-Job Processing (Sequential)                   │
│ For each job:                                               │
│   ├─ Research (company + role benchmarking)                 │
│   ├─ Template generation                                    │
│   ├─ Content matching (uses enriched library)              │
│   └─ Generation (MD + DOCX + Report)                        │
│ Interactive or Express mode                                 │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│ PHASE 3.5: ATS Simulation (per job)                        │
│ - Score each resume: keyword, format, structure             │
│ - Apply fixes automatically                                 │
│ - Regenerate files if changes made                          │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│ PHASE 4: Batch Finalization                                │
│ - Generate batch summary                                    │
│ - User reviews all resumes together                         │
│ - Approve/revise individual or batch                        │
│ - Offer cover letters for all jobs (batch mode)             │
│ - Update library with approved resumes                      │
└─────────────────────────────────────────────────────────────┘
```

**Time Savings:**
- 3 jobs: ~40 min (vs 45 min sequential) = 11% savings
- 5 jobs: ~55 min (vs 75 min sequential) = 27% savings

**Quality:** Same depth as single-job workflow (research, matching, generation)

**See `multi-job-workflow.md` for complete implementation details.**

### Phase 0: Library Initialization

**Always runs first - builds fresh resume database**

**Process:**

1. **Locate resume directory:**
   ```
   User provides path OR default to ./resumes/
   Validate directory exists
   ```

2. **Scan for markdown files:**
   ```
   Use Glob tool: pattern="*.md" path={resume_directory}
   Count files found
   Announce: "Building resume library... found {N} resumes"
   ```

3. **Parse each resume:**
   For each resume file:
   - Use Read tool to load content
   - Extract sections: roles, bullets, skills, education
   - Identify patterns: bullet structure, length, formatting

4. **Build experience database structure:**
   ```json
   {
     "roles": [
       {
         "role_id": "company_title_year",
         "company": "Company Name",
         "title": "Job Title",
         "dates": "YYYY-YYYY",
         "description": "Role summary",
         "bullets": [
           {
             "text": "Full bullet text",
             "themes": ["leadership", "technical"],
             "metrics": ["17x improvement", "$3M revenue"],
             "keywords": ["cross-functional", "program"],
             "source_resumes": ["resume1.md"],
             "quality_score": null
           }
         ]
       }
     ],
     "skills": {
       "technical": ["Python", "Kusto", "AI/ML"],
       "product": ["Roadmap", "Strategy"],
       "leadership": ["Stakeholder mgmt"]
     },
     "education": [],
     "user_preferences": {
       "typical_length": "1-page|2-page",
       "section_order": ["summary", "experience", "education"],
       "bullet_style": "pattern"
     },
     "library_health": {
       "last_quality_scan": null,
       "health_score": null,
       "upgraded_bullets": 0
     }
   }
   ```

5. **Tag content automatically:**
   - Themes: Scan for keywords (leadership, technical, analytics, etc.)
   - Metrics: Extract numbers, percentages, dollar amounts
   - Keywords: Frequent technical terms, action verbs

6. **▶ NEW: Run Bullet Quality Engine**
   ```
   After library is built, immediately run bullet quality scan.
   See: bullet-quality-engine.md for full scoring logic.
   
   Score every bullet on 5 dimensions (total 100 pts):
   - Action verb strength (25 pts)
   - Metric / quantification (30 pts)
   - Outcome / impact clarity (25 pts)
   - Specificity (15 pts)
   - Length & readability (5 pts)
   
   Tag each bullet with quality_score in library database.
   
   IF all bullets ≥70: proceed silently
   IF any bullets <70: surface quality report to user
                       offer upgrade session before proceeding
   
   This step runs ONCE per session regardless of job count.
   See bullet-quality-engine.md for full upgrade process.
   ```

**Output:** In-memory database with quality scores, ready for matching

**Code pattern:**
```python
# Pseudo-code for reference
library = {
    "roles": [],
    "skills": {},
    "education": [],
    "library_health": {}
}

for resume_file in glob("resumes/*.md"):
    content = read(resume_file)
    roles = extract_roles(content)
    for role in roles:
        role["bullets"] = tag_bullets(role["bullets"])
        library["roles"].append(role)

# NEW: Run quality engine after library built
library = run_bullet_quality_engine(library)  # see bullet-quality-engine.md

return library
```

### Phase 1: Research Phase

**Goal:** Build comprehensive "success profile" beyond just the job description

**Inputs:**
- Job description (text or URL from user)
- Optional: Company name if not in JD

**Process:**

**1.1 Job Description Parsing:**
```
Use research-prompts.md JD parsing template
Extract: requirements, keywords, implicit preferences, red flags, role archetype
```

**1.2 Company Research:**
```
WebSearch queries:
- "{company} mission values culture"
- "{company} engineering blog"
- "{company} recent news"

Synthesize: mission, values, business model, stage
```

**1.3 Role Benchmarking:**
```
WebSearch: "site:linkedin.com {job_title} {company}"
WebFetch: Top 3-5 profiles
Analyze: common backgrounds, skills, terminology

If sparse results, try similar companies
```

**1.4 Success Profile Synthesis:**
```
Combine all research into structured profile (see research-prompts.md template)

Include:
- Core requirements (must-have)
- Valued capabilities (nice-to-have)
- Cultural fit signals
- Narrative themes
- Terminology map (user's background → their language)
- Risk factors + mitigations
- ATS keyword list (all meaningful terms from JD, for use in Phase 4.5)
```

**Checkpoint:**
```
Present success profile to user:

"Based on my research, here's what makes candidates successful for this role:

{SUCCESS_PROFILE_SUMMARY}

Key findings:
- {Finding 1}
- {Finding 2}
- {Finding 3}

Does this match your understanding? Any adjustments?"

Wait for user confirmation before proceeding.
```

**Output:** Validated success profile document + ATS keyword list

### Phase 2: Template Generation

**Goal:** Create resume structure optimized for this specific role

**Inputs:**
- Success profile (from Phase 1)
- User's resume library (from Phase 0)

**Process:**

**2.1 Analyze User's Resume Library:**
```
Extract from library:
- All roles, titles, companies, date ranges
- Role archetypes (technical contributor, manager, researcher, specialist)
- Experience clusters (what domains/skills appear frequently)
- Career progression and narrative
```

**2.2 Role Consolidation Decision:**

**When to consolidate:**
- Same company, similar responsibilities
- Target role values continuity over granular progression
- Combined narrative stronger than separate
- Page space constrained

**When to keep separate:**
- Different companies (ALWAYS separate)
- Dramatically different responsibilities that both matter
- Target role values specific progression story
- One position has significantly more relevant experience

**Decision template:**
```
For {Company} with {N} positions:

OPTION A (Consolidated):
Title: "{Combined_Title}"
Dates: "{First_Start} - {Last_End}"
Rationale: {Why consolidation makes sense}

OPTION B (Separate):
Position 1: "{Title}" ({Dates})
Position 2: "{Title}" ({Dates})
Rationale: {Why separate makes sense}

RECOMMENDED: Option {A/B} because {reasoning}
```

**2.3 Title Reframing Principles:**

**Core rule:** Stay truthful to what you did, emphasize aspect most relevant to target

**Strategies:**

1. **Emphasize different aspects:**
   - "Graduate Researcher" → "Research Software Engineer" (if coding-heavy)
   - "Data Science Lead" → "Technical Program Manager" (if leadership)

2. **Use industry-standard terminology:**
   - "Scientist III" → "Senior Research Scientist" (clearer seniority)
   - "Program Coordinator" → "Project Manager" (standard term)

3. **Add specialization when truthful:**
   - "Engineer" → "ML Engineer" (if ML work substantial)
   - "Researcher" → "Computational Ecologist" (if computational methods)

4. **Adjust seniority indicators:**
   - "Lead" vs "Senior" vs "Staff" based on scope

**Constraints:**
- NEVER claim work you didn't do
- NEVER inflate seniority beyond defensible
- Company name and dates MUST be exact
- Core responsibilities MUST be accurate

**2.4 Generate Template Structure:**

```markdown
## Professional Summary
[GUIDANCE: {X} sentences emphasizing {themes from success profile}]
[REQUIRED ELEMENTS: {keywords from JD}]

## Key Skills
[STRUCTURE: {2-4 categories based on JD structure}]
[SOURCE: Extract from library matching success profile]

## Professional Experience

### [ROLE 1 - Most Recent/Relevant]
[CONSOLIDATION: {merge X positions OR keep separate}]
[TITLE OPTIONS:
  A: {emphasize aspect 1}
  B: {emphasize aspect 2}
  Recommended: {option with rationale}]
[BULLET ALLOCATION: {N bullets based on relevance + recency}]
[GUIDANCE: Emphasize {themes}, look for {experience types}]

Bullet 1: [SEEKING: {requirement type}]
Bullet 2: [SEEKING: {requirement type}]
...

### [ROLE 2]
...

## Education
[PLACEMENT: {top if required/recent, bottom if experience-heavy}]

## [Optional Sections]
[INCLUDE IF: {criteria from success profile}]
```

**Checkpoint:**
```
Present template to user:

"Here's the optimized resume structure for this role:

STRUCTURE:
{Section order and rationale}

ROLE CONSOLIDATION:
{Decisions with options}

TITLE REFRAMING:
{Proposed titles with alternatives}

BULLET ALLOCATION:
Role 1: {N} bullets (most relevant)
Role 2: {N} bullets
...

Does this structure work? Any adjustments to:
- Role consolidation?
- Title reframing?
- Bullet allocation?"

Wait for user approval before proceeding.
```

**Output:** Approved template skeleton with guidance for each section

### Phase 2.5: Experience Discovery (OPTIONAL)

**Goal:** Surface undocumented experiences through conversational discovery

**When to trigger:**
```
After template approval, if gaps identified:

"I've identified {N} gaps or areas where we have weak matches:
- {Gap 1}: {Current confidence}
- {Gap 2}: {Current confidence}
...

Would you like to do a structured brainstorming session to surface
any experiences you haven't documented yet?

This typically takes 10-15 minutes and often uncovers valuable content."

User can accept or skip.
```

**Branching Interview Process:**

**Approach:** Conversational with follow-up questions based on answers

**For each gap, conduct branching dialogue (see branching-questions.md):**

1. **Start with open probe:**
   - Technical gap: "Have you worked with {skill}?"
   - Soft skill gap: "Tell me about times you've {demonstrated_skill}"
   - Recent work: "What have you worked on recently?"

2. **Branch based on answer:**
   - YES/Strong → Deep dive (scale, challenges, metrics)
   - INDIRECT → Explore role and transferability
   - ADJACENT → Explore related experience
   - PERSONAL → Assess recency and substance
   - NO → Try broader category or move on

3. **Follow-up systematically:**
   - Ask "what," "how," "why" to get details
   - Quantify: "Any metrics?"
   - Contextualize: "Was this production?"
   - Validate: "Does this address the gap?"

4. **Capture immediately:**
   - Document experience as shared
   - Ask clarifying questions (dates, scope, impact)
   - Help articulate as resume bullet
   - Tag which gap(s) it addresses

**Capture Structure:**
```markdown
## Newly Discovered Experiences

### Experience 1: {Brief description}
- Context: {Where/when}
- Scope: {Scale, duration, impact}
- Addresses: {Which gaps}
- Bullet draft: "{Achievement-focused bullet}"
- Confidence: {How well fills gap - percentage}
- Quality Score: {Run through bullet quality engine immediately}

### Experience 2: ...
```

**Integration Options:**

After discovery session:
```
"Great! I captured {N} new experiences. For each one:

1. ADD TO CURRENT RESUME - Integrate now
2. ADD TO LIBRARY ONLY - Save for future, not needed here
3. REFINE FURTHER - Think more about articulation
4. DISCARD - Not relevant enough

Let me know for each experience."
```

**Important Notes:**
- Keep truthfulness bar high - help articulate, NEVER fabricate
- Focus on gaps and weak matches, not strong areas
- Time-box if needed (10-15 minutes typical)
- User can skip entirely if confident in library
- Recognize when to move on - don't exhaust user
- Run new bullets through bullet quality engine before integrating

**Output:** New experiences integrated into library, ready for matching

### Phase 3: Assembly Phase

**Goal:** Fill approved template with best-matching content, with transparent scoring

**Inputs:**
- Approved template (from Phase 2)
- Resume library + discovered experiences (from Phase 0 + 2.5)
- Success profile (from Phase 1)

**Process:**

**3.1 For Each Template Slot:**

1. **Extract all candidate bullets from library**
   - All bullets from library database
   - All newly discovered experiences
   - Include source resume and quality_score for each

2. **Score each candidate** (see matching-strategies.md)
   - Direct match (40%): Keywords, domain, technology, outcome
   - Transferable (30%): Same capability, different context
   - Adjacent (20%): Related tools, methods, problem space
   - Impact (10%): Achievement type alignment
   - **NEW: Apply quality multiplier**
     ```
     quality_multiplier:
       quality_score ≥85: 1.0x (no penalty)
       quality_score 70-84: 0.95x
       quality_score 50-69: 0.85x (prefer upgraded version if available)
       quality_score <50: 0.7x (flag, recommend upgrading before use)
     ```

   Overall = ((Direct × 0.4) + (Transfer × 0.3) + (Adjacent × 0.2) + (Impact × 0.1)) × quality_multiplier

3. **Rank candidates by score**
   - Sort high to low
   - Group by confidence band:
     * 90-100%: DIRECT
     * 75-89%: TRANSFERABLE
     * 60-74%: ADJACENT
     * <60%: WEAK/GAP

4. **Present top 3 matches with analysis:**
   ```
   TEMPLATE SLOT: {Role} - Bullet {N}
   SEEKING: {Requirement description}

   MATCHES:
   [DIRECT - 95%] "{bullet_text}"
     ✓ Direct: {what matches directly}
     ✓ Transferable: {what transfers}
     ✓ Metrics: {quantified impact}
     Quality: {score}/100
     Source: {resume_name}

   [TRANSFERABLE - 78%] "{bullet_text}"
     ✓ Transferable: {what transfers}
     ✓ Adjacent: {what's adjacent}
     ⚠ Gap: {what's missing}
     Quality: {score}/100
     Source: {resume_name}

   RECOMMENDATION: Use DIRECT match (95%)
   ALTERNATIVE: If avoiding repetition, use TRANSFERABLE (78%) with reframing
   ```

5. **Handle gaps (confidence <60%):**
   ```
   GAP IDENTIFIED: {Requirement}

   BEST AVAILABLE: {score}% - "{bullet_text}"

   REFRAME OPPORTUNITY: {If applicable}
   Original: "{text}"
   Reframed: "{adjusted_text}" (truthful because {reason})
   New confidence: {score}%

   OPTIONS:
   1. Use reframed version ({new_score}%)
   2. Acknowledge gap in cover letter
   3. Omit bullet slot (reduce allocation)
   4. Use best available with disclosure

   RECOMMENDATION: {Most appropriate option}
   ```

**3.2 Content Reframing:**

When good match (>60%) but terminology misaligned:

**Apply strategies from matching-strategies.md:**
- Keyword alignment (preserve meaning, adjust terms)
- Emphasis shift (same facts, different focus)
- Abstraction level (adjust technical specificity)
- Scale emphasis (highlight relevant aspects)

**Show before/after for transparency:**
```
REFRAMING APPLIED:
Bullet: {template_slot}

Original: "{original_bullet}"
Source: {resume_name}

Reframed: "{reframed_bullet}"
Changes: {what changed and why}
Truthfulness: {why this is accurate}
```

**Checkpoint:**
```
"I've matched content to your template. Here's the complete mapping:

COVERAGE SUMMARY:
- Direct matches: {N} bullets ({percentage}%)
- Transferable: {N} bullets ({percentage}%)
- Adjacent: {N} bullets ({percentage}%)
- Gaps: {N} ({percentage}%)

REFRAMINGS APPLIED: {N}
- {Example 1}
- {Example 2}

GAPS IDENTIFIED:
- {Gap 1}: {Recommendation}
- {Gap 2}: {Recommendation}

OVERALL JD COVERAGE: {percentage}%

Review the detailed mapping below. Any adjustments to:
- Match selections?
- Reframings?
- Gap handling?"

[Present full detailed mapping]

Wait for user approval before generation.
```

**Output:** Complete bullet-by-bullet mapping with confidence scores and reframings

### Phase 4: Generation Phase

**Goal:** Create professional multi-format outputs

**Inputs:**
- Approved content mapping (from Phase 3)
- User's formatting preferences (from library analysis)
- Target role information (from Phase 1)

**Process:**

**4.1 Markdown Generation:**

**Compile mapped content into clean markdown:**

```markdown
# {User_Name}

{Contact_Info}

---

## Professional Summary

{Summary_from_template}

---

## Key Skills

**{Category_1}:**
- {Skills_from_library_matching_profile}

**{Category_2}:**
- {Skills_from_library_matching_profile}

{Repeat for all categories}

---

## Professional Experience

### {Job_Title}
**{Company} | {Location} | {Dates}**

{Role_summary_if_applicable}

- {Bullet_1_from_mapping}
- {Bullet_2_from_mapping}
...

### {Next_Role}
...

---

## Education

**{Degree}** | {Institution} ({Year})
**{Degree}** | {Institution} ({Year})
```

**Use user's preferences:**
- Formatting style from library analysis
- Bullet structure pattern
- Section ordering
- Typical length (1-page vs 2-page)
- **NEW: Use standard dashes (-) for bullets, NOT unicode (•) — ATS safety**
- **NEW: Contact info in body text, NOT in document header — ATS safety**

**Output:** `{Name}_{Company}_{Role}_Resume.md`

**4.2 DOCX Generation:**

**Use document-skills:docx:**

```
REQUIRED SUB-SKILL: Use document-skills:docx

Create Word document with:
- Professional fonts (Calibri 11pt body, 12pt headers)
- Proper spacing (single within sections, space between)
- Clean bullet formatting (proper numbering config, NOT unicode)
- Contact information in BODY TEXT (not header/footer) — ATS requirement
- Single-column layout only — ATS requirement
- No text boxes — ATS requirement
- Appropriate margins (0.5-1 inch)
- Bold/italic emphasis (company names, titles, dates)
- Page breaks if 2-page resume

See docx skill documentation for:
- Paragraph and TextRun structure
- Numbering configuration for bullets
- Heading levels and styles
- Spacing and margins
```

**Output:** `{Name}_{Company}_{Role}_Resume.docx`

**4.3 PDF Generation (Optional):**

**If user requests PDF:**

```
OPTIONAL SUB-SKILL: Use document-skills:pdf

Convert DOCX to PDF OR generate directly
Ensure formatting preservation
Professional appearance for direct submission

NOTE: PDF is less reliably parsed by ATS than DOCX.
Recommend DOCX for online applications, PDF for email/direct submissions.
Mention this to user when generating.
```

**Output:** `{Name}_{Company}_{Role}_Resume.pdf`

**4.4 Generation Summary Report:**

**Create metadata file:**

```markdown
# Resume Generation Report
**{Role} at {Company}**

**Date Generated:** {timestamp}

## Target Role Summary
- Company: {Company}
- Position: {Role}
- IC Level: {If known}
- Focus Areas: {Key areas}

## Success Profile Summary
- Key Requirements: {top 5}
- Cultural Fit Signals: {themes}
- Risk Factors Addressed: {mitigations}

## Content Mapping Summary
- Total bullets: {N}
- Direct matches: {N} ({percentage}%)
- Transferable: {N} ({percentage}%)
- Adjacent: {N} ({percentage}%)
- Gaps identified: {list}

## Reframing Applied
- {bullet}: {original} → {reframed} [Reason: {why}]
...

## Source Resumes Used
- {resume1}: {N} bullets
- {resume2}: {N} bullets
...

## Gaps Addressed

### Before Experience Discovery:
{Gap analysis showing initial state}

### After Experience Discovery:
{Gap analysis showing final state}

### Remaining Gaps:
{Any unresolved gaps with recommendations}

## Key Differentiators for This Role
{What makes user uniquely qualified}

## Library Quality Summary
- Library Health Score: {avg}/100
- Bullets Upgraded This Session: {N}
- Weak Bullets Used (not upgraded): {N}

## ATS Analysis
- ATS Score: {score}/100 ({band})
- Keyword Coverage: {percentage}%
- Critical Fixes Applied: {N}
- Missing Keywords at Submission: {list or "none"}
- Format Safety: {Safe / Minor Issues / At Risk}

## Cover Letter
- Generated: Yes / No
- Tone: {detected tone}
- Word count: {N}
- Hook source: {what was referenced}
- Gap addressed: {gap or "none"}

## Recommendations for Interview Prep
- Stories to prepare
- Questions to expect
- Gaps to address
```

**Output:** `{Name}_{Company}_{Role}_Resume_Report.md`

### Phase 4.5: ATS Simulation ← NEW

**Goal:** Score the resume against ATS parsing requirements before delivery.

**Always run automatically after generation. Never skip.**

```
"Before I hand you these files, let me run an ATS simulation..."

[Run ATS Simulation — see ats-simulation.md for full logic]
```

**Five scoring dimensions (total 100 pts):**
- Keyword Match Score (35 pts) — JD keyword presence and density
- Format Compatibility Score (25 pts) — tables, columns, unicode, headers
- Section Structure Score (20 pts) — standard section names
- Length & Density Score (10 pts) — appropriate page count, bullet count
- Readability Score (10 pts) — date consistency, tense, abbreviations

**Present ATS Report:**
```
ATS SCORE: {total}/100 [{band}]

  Keyword Match:      {score}/35
  Format Safety:      {score}/25
  Section Structure:  {score}/20
  Length & Density:   {score}/10
  Readability:        {score}/10

CRITICAL FIXES: {list}
MISSING KEYWORDS: {list}

Apply fixes? (Y/N)
```

**If YES:** Apply fixes, regenerate files, re-run simulation, show delta.

**Full ATS logic:** See `ats-simulation.md`

**Output:** ATS-optimized resume files + ATS score in Report

### Phase 4.6: Cover Letter Generation ← NEW

**Goal:** Generate a tailored cover letter using research already completed.
No re-research needed — everything is already in memory.

**Trigger immediately after ATS simulation:**
```
"Your resume is ready (ATS Score: {score}/100).

Would you like a matching cover letter?
I already have everything I need — this takes about 2 minutes.

→ YES: Generate now
→ CUSTOM: I want to guide the tone/emphasis  
→ NO: Skip"
```

**What it uses (already in memory):**
- Company mission + cultural signals (Phase 1)
- Top 2 highest-match-score bullets (Phase 3)
- Narrative themes identified for this role (Phase 1)
- Gaps + mitigations (Phase 3)
- User's name + contact (Phase 0)

**Cover Letter Structure (250-350 words):**
- Paragraph 1: Company-specific hook (from research, not generic)
- Paragraph 2: 2 strongest achievements with metrics
- Paragraph 3: Cultural fit + optional gap address
- Paragraph 4: Clean close

**Tone calibration:**
Detect from Phase 1 company culture signals:
startup → direct/energetic | enterprise → professional/measured |
mission-driven → value-aligned | technical → credibility-first

**Gap address rule:**
Address maximum ONE gap. Never apologize — redirect to strength.

**Full cover letter logic:** See `cover-letter-hook.md`

**Outputs:**
- `{Name}_{Company}_{Role}_CoverLetter.md`
- `{Name}_{Company}_{Role}_CoverLetter.docx`

**Present to user:**
```
"Your tailored resume package has been generated!

FILES CREATED:
- {Name}_{Company}_{Role}_Resume.md
- {Name}_{Company}_{Role}_Resume.docx
- {Name}_{Company}_{Role}_CoverLetter.md      [if generated]
- {Name}_{Company}_{Role}_CoverLetter.docx    [if generated]
- {Name}_{Company}_{Role}_Resume_Report.md
{- {Name}_{Company}_{Role}_Resume.pdf (if requested)}

QUALITY METRICS:
- JD Coverage: {percentage}%
- ATS Score: {score}/100
- Direct Matches: {percentage}%
- Newly Discovered: {N} experiences
- Cover Letter: {Generated / Skipped}

Review and let me know:
1. Save to library (recommended)
2. Need revisions
3. Save but don't add to library"
```

### Phase 5: Library Update (CONDITIONAL)

**Goal:** Optionally add successful resume to library for future use

**When:** After user reviews and approves generated resume

**Checkpoint Question:**
```
"Are you satisfied with this resume?

OPTIONS:
1. YES - Save to library
   → Adds resume to permanent location
   → Rebuilds library database
   → Makes new content available for future resumes

2. NO - Need revisions
   → What would you like to adjust?
   → Make changes and re-present

3. SAVE BUT DON'T ADD TO LIBRARY
   → Keep files in current location
   → Don't enrich database
   → Useful for experimental resumes

Which option?"
```

**If Option 1 (YES - Save to library):**

**Process:**

1. **Move resume to library:**
   ```
   Source: {current_directory}/{Name}_{Company}_{Role}_Resume.md
   Destination: {resume_library}/{Name}_{Company}_{Role}_Resume.md

   Also move:
   - .docx file
   - .pdf file (if exists)
   - _Report.md file
   - _CoverLetter.md file (if exists)
   ```

2. **Rebuild library database:**
   ```
   Re-run Phase 0 library initialization
   Parse newly created resume
   Add bullets to experience database
   Update keyword/theme indices
   Tag with metadata:
     - target_company: {Company}
     - target_role: {Role}
     - generated_date: {timestamp}
     - jd_coverage: {percentage}
     - ats_score: {score}
     - success_profile: {reference to profile}
   ```

3. **Preserve generation metadata:**
   ```json
   {
     "resume_id": "{Name}_{Company}_{Role}",
     "generated": "{timestamp}",
     "source_resumes": ["{resume1}", "{resume2}"],
     "ats_score": 87,
     "cover_letter_generated": true,
     "reframings": [...],
     "match_scores": {...},
     "newly_discovered": [...],
     "bullets_upgraded": [...]
   }
   ```

4. **Announce completion:**
   ```
   "Resume saved to library!

   Library updated:
   - Total resumes: {N}
   - New content variations: {N}
   - Newly discovered experiences added: {N}
   - Bullets upgraded: {N}
   - Library health score: {score}/100

   This resume and its new content are now available for future tailoring sessions."
   ```

**If Option 2 (NO - Need revisions):**

```
"What would you like to adjust?"

[Collect user feedback]
[Make requested changes]
[Re-run relevant phases including ATS simulation]
[Re-present for approval]

[Repeat until satisfied or user cancels]
```

**If Option 3 (SAVE BUT DON'T ADD TO LIBRARY):**

```
"Resume files saved to current directory. Not added to library."
```

**Benefits of Library Update:**
- Grows library with each successful resume
- New bullet variations become available
- Upgraded bullets permanently saved
- ATS-optimized formatting becomes the library standard
- Discovered experiences permanently captured
- Self-improving system over time

**Output:** Updated library database + metadata preservation (if Option 1)

## Error Handling & Edge Cases

**Edge Case 1: Insufficient Resume Library**
```
SCENARIO: User has only 1-2 resumes, limited content

HANDLING:
"⚠️ Limited resume library detected ({N} resumes).

This may result in:
- Fewer matching options
- More gaps in coverage
- Less variety in bullet phrasing

RECOMMENDATIONS:
- Proceed with available content (I'll do my best!)
- Consider adding more resumes after this generation
- Experience Discovery phase will be especially valuable

Continue? (Y/N)"
```

**Edge Case 2: No Good Matches (confidence <60% for critical requirement)**
```
SCENARIO: Template slot requires experience user doesn't have

HANDLING:
"❌ GAP: {Requirement}

No matches found with confidence >60%

OPTIONS:
1. Run Experience Discovery - might surface undocumented work
2. Reframe best available ({score}%) - I'll show you the reframing
3. Omit bullet slot - reduce template allocation
4. Note for cover letter - cover letter hook will address this gap

Which approach?"

[Don't force matches - be transparent about gaps]
```

**Edge Case 3: Research Phase Failures**
```
SCENARIO: WebSearch fails, LinkedIn unavailable, company info sparse

HANDLING:
"⚠️ Limited company research available.

What I found:
- {Available info}

What's missing:
- {Missing areas}

Falling back to job description-only analysis.
Result: Template will be optimized for JD but may miss cultural nuances.
Cover letter hook will use JD language only (no company-specific hook).

Do you have additional context about:
- Company culture?
- Team structure?
- Technologies used?

(Optional - you can share or skip)"

[Proceed with best-effort approach]
```

**Edge Case 4: Job Description Quality Issues**
```
SCENARIO: Vague JD, missing requirements, poorly written

HANDLING:
"⚠️ Job description lacks detail in areas:
- {Missing area 1}
- {Missing area 2}

This may limit template optimization and ATS keyword extraction.

Do you have additional context about:
- {Question 1}
- {Question 2}

(Optional - I'll work with what's available)"

[Extract and proceed]
```

**Edge Case 5: Ambiguous Role Consolidation**
```
SCENARIO: Unclear whether to merge roles or keep separate

HANDLING:
"🤔 Ambiguous consolidation decision for {Company}

OPTION A (Consolidated): {Rationale}
OPTION B (Separate): {Rationale}

Both are defensible. Which do you prefer?

(This becomes your preference for similar situations)"

[Remember preference for future]
```

**Edge Case 6: Resume Length Constraints**
```
SCENARIO: Too much good content, exceeds 2 pages

HANDLING:
"⚠️ Content exceeds 2 pages (current: {N} bullets, ~{pages} pages)

PRUNING SUGGESTIONS (ranked by relevance):
Remove:
- {Bullet X}: {score}% match (lowest)
- {Bullet Y}: {score}% match
...

Keep all bullets and accept >2 pages?
OR
Remove {N} bullets to fit 2 pages?

Note: Longer resumes also score lower on ATS length dimension.

Your preference?"

[User decides priority]
```

**Edge Case 7: ATS Score Below 55**
```
SCENARIO: Resume fails ATS simulation with score <55

HANDLING:
"⚠️ ATS Score: {score}/100 — At Risk

This resume may not pass automated screening at large companies.

CRITICAL ISSUES:
{List all critical fixes}

I strongly recommend applying fixes before submission.
This will take approximately {N} minutes.

Apply all critical fixes now? (Y/N)"

[If Y: Apply automatically, regenerate, re-run ATS, show new score]
[If N: Save with warning attached to report]
```

**Edge Case 8: Cover Letter Hook — No Company Research**
```
SCENARIO: Company research failed, minimal Phase 1 data

HANDLING:
Cover letter Paragraph 1 fallback:
→ Lead with role-specific hook (JD language) instead of company hook
→ Flag to user: "I couldn't find enough about {Company} to write 
  a specific hook. I've used the role description instead.
  If you know something specific about this company, tell me and 
  I'll personalize the opening."
```

**Error Recovery:**
- All checkpoints allow going back to previous phase
- User can request adjustments at any checkpoint
- Generation failures (DOCX/PDF) fall back to markdown-only
- ATS simulation failure → proceed without score, flag in report
- Cover letter generation failure → offer markdown version only

**Graceful Degradation:**
- Research limited → Fall back to JD-only analysis
- Library small → Work with available + emphasize discovery
- Matches weak → Transparent gap identification
- Generation fails → Provide markdown + error details
- ATS simulation fails → Proceed without score, note in report
- Cover letter hook fails → Offer manual questions-based approach

## Usage Examples

**Example 1: Internal Role (Same Company)**
```
USER: "I want to apply for Principal PM role in 1ES team at Microsoft.
      Here's the JD: {paste}"

SKILL:
1. Library Build: Finds 29 resumes
2. Bullet Quality Scan: 6 weak bullets found → upgraded 4
3. Research: Microsoft 1ES team, internal culture, role benchmarking
4. Template: Features PM2 Azure Eng Systems role (most relevant)
5. Discovery: Surfaces VS Code extension, Bhavana AI side project
6. Assembly: 92% JD coverage, 75% direct matches
7. Generate: MD + DOCX + Report
8. ATS Simulation: Score 89/100 — 2 minor fixes applied → 94/100
9. Cover Letter: Generated with Microsoft internal culture hook
10. User approves → Library updated

RESULT: Highly competitive application package (resume + cover letter)
```

**Example 2: Career Transition (Different Domain)**
```
USER: "I'm a TPM trying to transition to ecology PM role. JD: {paste}"

SKILL:
1. Library Build: Finds existing TPM resumes
2. Bullet Quality Scan: 3 weak bullets upgraded
3. Research: Ecology sector, sustainability focus, cross-domain transfers
4. Template: Reframes "Technical Program Manager" → "Program Manager,
             Environmental Systems"
5. Discovery: Surfaces volunteer conservation work, graduate research
6. Assembly: 65% JD coverage - flags gaps in domain-specific knowledge
7. Generate: Resume + gap analysis
8. ATS Simulation: Score 72/100 — missing domain keywords → adds to Skills
9. Cover Letter: Addresses career transition gap directly in paragraph 3
                Uses "why now, why this domain" framing

RESULT: Resume + cover letter that bridges technical skills with
        environmental domain, with transition addressed confidently
```

**Example 3: Career Gap Handling**
```
USER: "I have a 2-year gap while starting a company. JD: {paste}"

SKILL:
1. Library Build: Finds pre-gap resumes
2. Bullet Quality Scan: Startup period added as role with strong bullets
3. Research: Standard analysis
4. Template: Includes startup as legitimate role
5. Discovery: Surfaces skills from startup (fundraising, product, team)
6. Assembly: Frames gap as entrepreneurial experience
7. Generate: Resume presenting gap as strength
8. ATS Simulation: Score 81/100 — gap period dates format fixed
9. Cover Letter: Paragraph 3 addresses gap as intentional entrepreneurship

RESULT: Gap becomes strength in both resume and cover letter
```

**Example 4: Multi-Job Batch (3 Similar Roles)**
```
USER: "I want to apply for these 3 TPM roles:
      1. Microsoft 1ES Principal PM
      2. Google Cloud Senior TPM
      3. AWS Container Services Senior PM
      Here are the JDs: {paste 3 JDs}"

SKILL:
1. Multi-job detection: Triggered (3 JDs detected)
2. Library Build: Finds 29 resumes (once)
3. Bullet Quality Engine: Runs once, upgrades 5 bullets across library
4. Gap Analysis: 8 unique gaps after deduplication
5. Shared Discovery: Surfaces 5 new experiences
6. Per-Job Processing (×3):
   - Job 1 (Microsoft): 85% coverage, ATS: 91/100
   - Job 2 (Google): 88% coverage, ATS: 88/100
   - Job 3 (AWS): 78% coverage, ATS: 85/100
7. Cover Letters: All 3 generated with company-specific hooks
8. Batch Summary: 3 resumes + 3 cover letters in 40 minutes

RESULT: Complete application packages for all 3 roles
```

## Testing Guidelines

**Manual Testing Checklist:**

**Test 1: Happy Path**
```
- Provide JD with clear requirements
- Library with 10+ resumes
- Run all phases without skipping
- Verify generated files
- Verify ATS score ≥70
- Verify cover letter generated
- Check library update
PASS CRITERIA:
- All files generated correctly
- JD coverage >70%
- ATS score reported
- Cover letter present
- No errors in any phase
```

**Test 2: Bullet Quality Engine**
```
- Provide library with deliberately weak bullets
- Run Phase 0
- Verify quality scan surfaces weak bullets
- Complete upgrade session
- Verify upgraded bullets score higher
PASS CRITERIA:
- Weak bullets correctly identified
- Upgrade process smooth
- Library health score improves
- Upgraded bullets used in matching
```

**Test 3: ATS Simulation**
```
- Generate resume with formatting issues (add unicode bullets, header contact)
- Run ATS simulation
- Verify issues detected and scored correctly
- Apply fixes
- Verify re-score improves
PASS CRITERIA:
- All format issues detected
- Score accurate
- Fixes applied correctly
- Delta shown after fix
```

**Test 4: Cover Letter Hook**
```
- Run full workflow for a job
- Accept cover letter at Phase 4.6
- Verify hook references specific company research
- Verify achievements match top bullets from Phase 3
- Verify tone appropriate for company culture
PASS CRITERIA:
- Hook is company-specific, not generic
- Achievements match resume content
- Tone calibrated correctly
- Word count 250-350
- No fabricated content
```

**Test 5: Minimal Library**
```
- Provide only 2 resumes
- Run through workflow
- Verify graceful handling
PASS CRITERIA:
- Warning about limited library
- Still produces reasonable output
- Gaps clearly identified
- ATS simulation still runs
- Cover letter still generated
```

**Test 6: Research Failures**
```
- Use obscure company with minimal online presence
- Verify fallback to JD-only
- Verify cover letter fallback to role-specific hook
PASS CRITERIA:
- Warning about limited research
- Proceeds with JD analysis
- Cover letter uses role hook, not company hook
- User notified of fallback
```

**Regression Testing:**
```
After any SKILL.md or module file changes:
1. Re-run Test 1 (happy path)
2. Verify no functionality broken
3. All three new modules must pass their specific tests
4. Commit only if all pass
```
