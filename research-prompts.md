# Research Prompts — Company & Role Intelligence System

## Overview

This file contains the complete research architecture for Phase 1. The goal is not just to read the job description — it is to build a comprehensive **Success Profile** that answers: *"What does the perfect candidate for this exact role at this exact company look like, and how does the user's background map to that?"*

Depth of research here directly determines quality of everything downstream — template, matching, cover letter, ATS keywords.

---

## Research Execution Order

Run all four research modules in sequence. Each builds on the previous.

```
Module 1: Job Description Deep Parse
Module 2: Company Intelligence
Module 3: Role Benchmarking
Module 4: Success Profile Synthesis
```

Never skip a module. If data is unavailable, note the gap and proceed — never assume or fabricate.

---

## MODULE 1 — Job Description Deep Parse

### 1.1 Surface Requirements Extraction

Read the full job description carefully. Extract every stated requirement into one of four buckets:

```
MUST-HAVE (deal-breakers if missing):
  Look for: "required", "must have", "X+ years", "you will need", 
            "minimum qualifications", hard skills listed first

STRONG PREFERENCE (important but not eliminators):
  Look for: "preferred", "ideally", "a plus", "nice to have",
            "bonus points", "we'd love if"

IMPLICIT (unstated but obvious from context):
  Examples: Senior role at startup → assume comfort with ambiguity
            Data role at finance firm → assume regulatory awareness
            PM at B2B SaaS → assume enterprise sales cycle understanding

RED FLAGS (signals that suggest cultural/style fit requirements):
  Look for: "fast-paced", "wear many hats", "self-starter", "no ego",
            "move fast", "scrappy" → these are culture fit signals, not skills
```

### 1.2 Keyword Extraction for ATS

Pull every meaningful term from the JD for the ATS simulation module:

```
HARD SKILLS (tools, technologies, platforms, languages):
  Extract exactly as written — "Python" not "python", "A/B testing" not "AB testing"
  Note: count how many times each appears (frequency = importance signal)

SOFT SKILLS (explicitly named, not implied):
  Only extract if literally written: "stakeholder management", 
  "cross-functional collaboration", "executive communication"
  Do NOT invent soft skills that aren't stated

CERTIFICATIONS / CREDENTIALS:
  Any named cert (PMP, AWS Solutions Architect, CFA, etc.)
  Note if "required" vs "preferred"

DOMAIN TERMS (industry-specific vocabulary):
  Terms that signal insider knowledge: "GTM", "PLG", "0→1", "JTBD",
  "sprint ceremonies", "north star metric", etc.
  These are high-value — using them signals domain fluency

ACTION DOMAINS (what you'll actually DO in the role):
  Extract verbs from responsibilities: "define", "ship", "lead", "analyze",
  "own", "collaborate", "build", "scale" — these inform bullet verb choices
```

### 1.3 Role Archetype Classification

Classify the role into one primary archetype. This shapes the entire resume narrative.

```
ARCHETYPES:

BUILDER (0→1):
  Signals: "greenfield", "build from scratch", "founding team", 
           "establish the function", "0→1", "new product"
  Resume emphasis: Founding experience, scrappiness, breadth, ownership
  Bullet style: What you created, what didn't exist before you

SCALER (1→10):
  Signals: "scale", "grow", "expand", "next level", "Series B/C",
           "existing product", "increase adoption"
  Resume emphasis: Growth metrics, systems thinking, operational rigor
  Bullet style: Before/after numbers, % improvement, scale achieved

OPERATOR (running the machine):
  Signals: "manage ongoing", "maintain", "reliability", "process",
           "efficiency", "cost reduction", "sustain"
  Resume emphasis: Consistency, reliability, process improvement
  Bullet style: Uptime, efficiency gains, cost savings, team performance

STRATEGIST (direction-setter):
  Signals: "vision", "roadmap", "strategy", "executive", "director+",
           "define", "shape", "influence without authority"
  Resume emphasis: Business impact, stakeholder influence, market understanding
  Bullet style: Decisions made, strategies defined, business outcomes

SPECIALIST (deep expertise):
  Signals: Named specific technology, "expert in", "deep knowledge",
           domain-specific certifications required
  Resume emphasis: Depth, specific achievements in specialty, credentials
  Bullet style: Specific technical wins, publications, innovations

COLLABORATOR (glue role):
  Signals: "cross-functional", "partner with", "work across",
           "matrix organization", "coordinate between"
  Resume emphasis: Relationships built, alignment achieved, programs run
  Bullet style: Teams influenced, programs coordinated, alignment created
```

### 1.4 Seniority Level Analysis

Determine actual seniority expectations (title can be misleading):

```
INDICATORS OF ACTUAL SENIORITY:

IC vs. Manager:
  "you will manage" / "direct reports" / "grow the team" → manager track
  "you will build" / "you will ship" / "hands-on" → IC track
  Both? → player-coach role, note this

Decision-making scope:
  "recommend" → junior/mid (decisions get approved above you)
  "define" / "own" → senior (decisions are yours)
  "shape company direction" → staff/principal/director

Collaboration level:
  "work with your team" → within-team scope
  "partner with product/eng/sales" → cross-functional scope
  "present to executives" / "influence C-suite" → senior/staff scope

Budget/revenue scope:
  Mentioned explicitly → note the figure, match in resume
  Not mentioned → don't invent one

SENIORITY CALIBRATION:
  Entry: Learning, executing assigned work, output-focused
  Mid: Owning problems, some autonomy, output + approach
  Senior: Defining problems, significant autonomy, outcome-focused
  Staff/Principal: Org-wide impact, sets direction, multiplier for others
  Director+: Business outcomes, people development, strategy
```

### 1.5 Implicit Culture Signals

These are not requirements but will influence template tone and cover letter:

```
WORK STYLE SIGNALS:
  "autonomous" → don't mention needing a lot of direction
  "collaborative" → emphasize team contributions alongside personal wins
  "data-driven" → every claim needs a number
  "customer-obsessed" → lead with customer impact in bullets

COMPANY STAGE SIGNALS:
  Startup language ("equity", "ground floor", "build the plane") 
    → emphasize range, speed, ownership
  Enterprise language ("governance", "compliance", "alignment")
    → emphasize process, stakeholder management, reliability

WHAT THEY'RE SOLVING FOR:
  Read the problems described in the JD:
  "We're scaling fast and need someone to..." → they need an operator
  "We're entering a new market..." → they need a builder/strategist
  "Our current process is..." → they need a fixer
  Match the resume narrative to what they're actually trying to solve
```

**Output from Module 1:**
```
JD_PARSE = {
  must_haves: [...],
  strong_preferences: [...],
  implicit_requirements: [...],
  red_flags_culture: [...],
  ats_keywords: {critical: [...], important: [...], standard: [...]},
  role_archetype: "BUILDER|SCALER|OPERATOR|STRATEGIST|SPECIALIST|COLLABORATOR",
  seniority_level: "entry|mid|senior|staff|director+",
  decision_scope: "...",
  work_style_signals: [...],
  core_problem_being_solved: "..."
}
```

---

## MODULE 2 — Company Intelligence

Run these web searches in order. Synthesize findings — don't just list results.

### 2.1 Core Company Research

```
SEARCH 1: Company fundamentals
Query: "{company name} about mission products"
Extract: What they do, who they serve, what problem they solve
Note: Use their exact language — this becomes cover letter vocabulary

SEARCH 2: Company stage and health
Query: "{company name} funding valuation employees 2024 2025"
Extract: Funding stage, headcount, growth trajectory
Note: Shapes cover letter tone (startup energy vs. enterprise stability)

SEARCH 3: Recent news
Query: "{company name} news announcement 2025"
Extract: Latest product launches, partnerships, expansions, challenges
Note: This is GOLD for cover letter hook — reference something current
      Shows you did homework. Most candidates don't.

SEARCH 4: Culture and values
Query: "{company name} culture values employees Glassdoor"
Extract: How they describe themselves, what employees say
Note: Compare stated values vs. actual employee experience
      Use stated values in cover letter, not cynical reality
```

### 2.2 Team-Specific Research

```
SEARCH 5: The specific team
Query: "{company name} {team name or department} team"
Example: "Stripe payments infrastructure team"
Extract: What this team builds, their recent work, their challenges

SEARCH 6: Hiring manager / team lead (if name visible in JD)
Query: "{hiring manager name} {company name} LinkedIn"
Extract: Their background, what they've built, their priorities
Note: Never stalk. Use ONLY for understanding what they value.
      A hiring manager who built enterprise systems values different
      things than one who built consumer products.

SEARCH 7: Engineering/product blog (if technical role)
Query: "{company name} engineering blog" OR "{company name} tech blog"
Extract: Technologies used, architectural decisions, team culture
Note: Reference a specific post in cover letter for extreme personalization
```

### 2.3 Competitive Context

```
SEARCH 8: Market position
Query: "{company name} vs {competitor} {industry}"
Extract: How they differentiate, what they're fighting for
Note: Helps understand what "winning" looks like for them
      Shapes which of your experiences to emphasize

SEARCH 9: Industry trends affecting them
Query: "{industry} trends 2025" OR "{company name} challenges"
Extract: Tailwinds and headwinds they're navigating
Note: Shows strategic awareness if referenced in cover letter
```

### 2.4 ATS System Detection

```
SEARCH 10: What ATS they use
Query: "{company name} apply Workday" OR "{company name} Greenhouse careers"
Also check: The application URL itself often reveals the ATS
  - jobs.lever.co → Lever
  - greenhouse.io → Greenhouse  
  - myworkdayjobs.com → Workday
  - icims.com → iCIMS
  - taleo.net → Taleo
  - jobvite.com → Jobvite

Note detected ATS in research output — used by ats-simulation.md
```

**Output from Module 2:**
```
COMPANY_INTEL = {
  what_they_do: "...",
  who_they_serve: "...",
  stage: "seed|series_a|series_b|growth|public|enterprise",
  headcount: "...",
  recent_news: [{headline, date, relevance}],
  mission_statement: "exact quote or close paraphrase",
  culture_keywords: [...],
  team_specific_info: "...",
  hiring_manager_background: "...",
  tech_stack_signals: [...],
  ats_system: "Workday|Greenhouse|Lever|Taleo|iCIMS|Unknown",
  cover_letter_hook_material: "specific thing to reference"
}
```

---

## MODULE 3 — Role Benchmarking

**Goal:** Understand what people who actually GET this role look like. Not just the JD — the reality.

### 3.1 LinkedIn Profile Research

```
SEARCH 1: People in this exact role at this company
Query: "site:linkedin.com {job title} {company name}"
Fetch: Top 3-5 profiles
Extract per profile:
  - Education background
  - Previous companies (FAANG? Startup? Consulting? Agency?)
  - Career trajectory (how did they get here?)
  - Years of experience
  - Skills listed
  - Any unusual patterns

SEARCH 2: People who previously held this role
Query: "site:linkedin.com former {job title} {company name}"
Or look at LinkedIn "People also viewed" patterns
Extract: Where do they go after this role? (signals growth trajectory)
```

### 3.2 Competitor Role Research

```
SEARCH 3: Same role at similar companies
Query: "site:linkedin.com {job title} {competitor 1}"
Query: "site:linkedin.com {job title} {competitor 2}"
Extract: Common backgrounds across companies — what's universal for this role?

Purpose: Builds a "role DNA" picture
If 8/10 people in this role came from consulting → consulting experience = strong signal
If 8/10 have a specific certification → credential worth mentioning
If 8/10 came from the same 3 companies → note which companies are feeders
```

### 3.3 Role DNA Synthesis

After reviewing profiles, synthesize into patterns:

```
ROLE DNA TEMPLATE:

Common education backgrounds:
  Most frequent: [degree/field]
  Notable outliers: [unconventional backgrounds that still made it]

Common previous companies:
  Feeder companies: [list]
  What this signals: [what those companies are known for]

Typical career trajectory:
  "[Role A] → [Role B] → [This Role]" is the common path
  Alternative paths observed: [...]

Years of experience range:
  Most have [X-Y] years
  Outliers with [Z] years exist (suggests range is flexible)

Skills that appear consistently:
  Technical: [list]
  Functional: [list]
  Industry: [list]

What this person typically does NEXT:
  [Promotion path or lateral moves observed]
  This tells the hiring manager what THEY want to grow into

Gaps/unusual patterns:
  [Anything that contradicts the JD requirements]
  [e.g. "JD requires 5 years but everyone in role has 3"]
```

### 3.4 Terminology Map

Build a direct translation between the user's background language and the target company/role language:

```
TERMINOLOGY MAP:

USER'S TERM          → THEIR TERM
"product manager"    → "PM" (if that's what they use internally)
"sprints"            → "cycles" (if they use different agile terminology)
"customers"          → "users" OR "clients" (depends on company)
"revenue"            → "ARR" OR "GMV" (depends on business model)
"team lead"          → "tech lead" OR "EM" (depends on company structure)
"machine learning"   → "AI/ML" OR "applied ML" (depends on their usage)

INSTRUCTION: When matching bullets in Phase 3, prefer their terminology.
             When writing cover letter, exclusively use their vocabulary.
             Terminology alignment signals cultural fit subconsciously.
```

**Output from Module 3:**
```
ROLE_BENCHMARKING = {
  role_dna: {
    common_backgrounds: [...],
    feeder_companies: [...],
    typical_trajectory: "...",
    experience_range: "X-Y years",
    consistent_skills: [...]
  },
  terminology_map: {
    user_term: their_term,
    ...
  },
  competitive_differentiators: "What user has that typical candidates don't",
  gaps_vs_typical_hire: "Where user falls short vs. typical candidate"
}
```

---

## MODULE 4 — Success Profile Synthesis

**Goal:** Combine all research into a single, actionable document that drives every downstream decision.

### 4.1 Success Profile Template

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SUCCESS PROFILE: {Job Title} at {Company}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ROLE SUMMARY:
  Archetype: {BUILDER / SCALER / OPERATOR / STRATEGIST / SPECIALIST}
  Seniority: {Level with scope description}
  Core problem they're hiring to solve: {1-2 sentences}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
WHAT THEY NEED (Priority Ordered)

CRITICAL — must demonstrate all of these:
  1. {Requirement + why it's critical}
  2. {Requirement + why it's critical}
  3. {Requirement + why it's critical}

IMPORTANT — should demonstrate most of these:
  1. {Requirement}
  2. {Requirement}
  3. {Requirement}

BONUS — nice to have, differentiating if present:
  1. {Requirement}
  2. {Requirement}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
COMPANY CONTEXT

Mission: "{exact or paraphrased mission}"
Stage: {funding/size}
Recent development: "{specific recent thing — for cover letter hook}"
Culture signals: {what they value, how they work}
ATS System: {detected or Unknown}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NARRATIVE THEMES

The story this resume needs to tell:
"{2-3 sentence narrative that connects user's background 
  to this role's needs — the throughline}"

Key themes to reinforce throughout resume:
  Theme 1: {e.g. "technical depth with business impact"}
  Theme 2: {e.g. "builder who has scaled what they built"}
  Theme 3: {e.g. "cross-functional leader, not just IC contributor"}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TERMINOLOGY MAP

Use these terms in resume and cover letter:
  {user term} → {their term}
  {user term} → {their term}
  ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RISK FACTORS & MITIGATIONS

Risk 1: {Gap or mismatch identified}
  Mitigation: {How to address — reframing, omission, cover letter}

Risk 2: {Gap or mismatch}
  Mitigation: {How to address}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
USER'S COMPETITIVE POSITION

Strengths for this role:
  ✅ {Strength 1 — specific to this role}
  ✅ {Strength 2}
  ✅ {Strength 3}

Gaps vs. ideal candidate:
  ⚠️  {Gap 1 — severity: high/medium/low}
  ⚠️  {Gap 2 — severity: high/medium/low}

Unique differentiators (what most candidates won't have):
  ⭐ {Something distinctive in user's background}
  ⭐ {Another differentiator}

Overall fit assessment: {Strong / Moderate / Stretch}
If Stretch: {Specific advice on how to frame application}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ATS KEYWORD LIST

Critical (must appear in resume):
  {keyword 1}, {keyword 2}, {keyword 3}...

Important (should appear):
  {keyword 1}, {keyword 2}...

Suggested placement:
  Skills section: {list}
  Summary: {list}
  Bullet text (naturally integrated): {list}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### 4.2 Checkpoint with User

Present the success profile and ask for validation:

```
"Here's what I found. This is the intelligence that will drive 
your entire resume and cover letter.

{Present full success profile}

A few things I want to confirm with you:

1. CORE PROBLEM: I believe they're hiring to solve: {problem}
   Does that match your understanding?

2. NARRATIVE: The story I want to tell is: {narrative}
   Does this feel right for your background?

3. RISKS: I've flagged {N} risks:
   {list risks}
   Any of these you can address that I'm not aware of?

4. DIFFERENTIATORS: Your unfair advantages for this role are:
   {list}
   Anything I'm missing that would be relevant here?

Confirm or correct any of these before I build the template."
```

### 4.3 Research Fallback Tiers

When specific research data is unavailable, fall back gracefully:

```
TIER 1 (Full research — ideal):
  JD + company website + LinkedIn profiles + recent news
  → Full success profile with all sections complete

TIER 2 (Partial research):
  JD + company website only (LinkedIn blocked/sparse)
  → Full success profile, role benchmarking section marked "Limited"
  → Terminology map based on JD language only

TIER 3 (JD only):
  JD available, company research failed
  → Reduced success profile
  → ATS keywords from JD only
  → Cover letter hook uses role language, not company-specific reference
  → Flag to user: "Research was limited — cover letter will be less personalized"

TIER 4 (Minimal JD):
  Vague JD with few requirements
  → Ask user for context:
    "This job description is quite vague. Can you tell me:
     1. What's the main problem this role solves?
     2. Is there anything about the team/company you already know?
     3. Have you spoken to anyone there?"
```

---

## Quick Reference: Research Red Flags

Things to flag to the user immediately if encountered:

```
🚨 CRITICAL FLAGS:

"5+ years required" + user has 2 → flag as high-risk stretch application

Role requires specific certification user doesn't have (e.g. CFA, PMP, MD)
→ Flag immediately: "This role requires {cert}. Without it, application 
  may be automatically filtered. Do you have this or equivalent experience?"

Job posting is 60+ days old → might already be filled, proceed with caution

Company has recent mass layoffs in this department 
→ Flag: "Research shows recent layoffs in this area. Proceed with awareness."

Job description is nearly identical to 3 other postings from same company
→ Might be a ghost posting or bulk hire

⚠️  MODERATE FLAGS:

All bullets in JD use "you will manage X people" but user has never managed
→ Flag: management requirement, suggest addressing in cover letter

Heavy emphasis on a specific tool user has never used (appears 4+ times)
→ Flag: "X appears 4 times — this seems critical. Do you have any exposure?"

"Startup experience required" + user only has enterprise background
→ Flag as cultural fit risk — address in cover letter
```
