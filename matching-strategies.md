# Matching Strategies — Content Matching Algorithms & Scoring

## Overview

This file defines the complete system for matching bullets from the user's resume library to each slot in the approved template. The goal is not just to find a relevant bullet — it is to find the **best possible truthful representation** of the user's experience for each specific requirement.

**Core Rule:** Every match decision must be explainable. Never select a bullet because it "feels" right. Score it, show the reasoning, and let the user confirm.

---

## The Matching Framework

Every candidate bullet is scored across four dimensions. Scores combine into a **Composite Match Score** that drives selection ranking.

```
COMPOSITE SCORE = 
  (Direct Match × 0.40) +
  (Transferable Match × 0.30) +
  (Adjacent Match × 0.20) +
  (Impact Alignment × 0.10)
  
  × Quality Multiplier (from bullet-quality-engine.md)
```

---

## DIMENSION 1 — Direct Match Score (40 points)

**Definition:** The bullet directly addresses what the template slot is seeking. Same domain, same technology, same problem type, same outcome type.

### Scoring Rubric

```
40 pts — PERFECT DIRECT MATCH:
  All three elements align: domain + technology/method + outcome type
  Example:
    Slot seeks: "Machine learning model deployment experience"
    Bullet: "Deployed 3 production ML models serving 50M users, 
             reducing inference latency by 40%"
  → Domain ✓ (ML), Technology ✓ (deployment/production), 
    Outcome ✓ (latency improvement at scale)

30 pts — STRONG DIRECT MATCH:
  Domain + technology align, outcome type partially matches
  Example:
    Slot seeks: "Cross-functional program management at scale"
    Bullet: "Led 12-team program across eng/product/sales to deliver 
             Q4 roadmap on time"
  → Domain ✓ (program management), Scale ✓ (12 teams), 
    Outcome partial (delivery vs. strategic impact)

20 pts — MODERATE DIRECT MATCH:
  Domain matches, technology/method or outcome partially aligns
  Example:
    Slot seeks: "SQL and data analysis"
    Bullet: "Built automated reporting pipeline reducing manual 
             analysis time by 70%"
  → Domain ✓ (data/analysis), Technology implied (automation),
    Outcome ✓ but technology not explicitly stated

10 pts — WEAK DIRECT MATCH:
  Domain loosely matches, significant gaps in technology or outcome
  Example:
    Slot seeks: "Kubernetes and container orchestration"
    Bullet: "Managed cloud infrastructure for 99.9% uptime"
  → Domain partial (cloud infra), Technology ✗ (no K8s mentioned),
    Outcome ✓ (reliability)

0 pts — NO DIRECT MATCH:
  Fundamentally different domain, technology, and outcome
```

### Direct Match Keyword Scoring

Within the direct match assessment, also score keyword presence:

```
For each JD keyword marked CRITICAL in success profile:
  Exact match in bullet → +3 pts (capped at 40 total for this dimension)
  Stem match → +2 pts
  Synonym → +1 pt
  Missing → 0 pts

For IMPORTANT keywords:
  Exact match → +2 pts
  Stem/synonym → +1 pt
```

---

## DIMENSION 2 — Transferable Match Score (30 points)

**Definition:** The bullet demonstrates the same underlying capability in a different context, domain, or technology. The skill transfers — the environment differs.

### Transferability Assessment

```
30 pts — HIGHLY TRANSFERABLE:
  Core capability identical, only surface context differs
  Examples:
  
  "Led 50-person engineering organization" (at B2B SaaS)
  → transferable to: any senior engineering leadership role
  
  "Reduced infrastructure costs by $2M through architecture redesign"
  → transferable to: any cost optimization role, any cloud/infra role
  
  "Grew product from 0 to 2M users in 18 months"
  → transferable to: any growth/PM/product role

20 pts — MODERATELY TRANSFERABLE:
  Core capability transfers, method or scale differs meaningfully
  Examples:
  
  "Managed $500K budget for internal tool development" 
  → transferable to: product budget ownership (different scale/type)
  
  "Led customer discovery interviews for consumer app"
  → transferable to: B2B customer research (different customer type)

10 pts — WEAKLY TRANSFERABLE:
  Adjacent capability, requires significant reframing to connect
  Examples:
  
  "Built internal dashboards for operations team"
  → weakly transferable to: data visualization/analytics role
  
  "Wrote technical documentation for API"
  → weakly transferable to: technical PM role

0 pts — NOT TRANSFERABLE:
  Different capability, domain, and outcome entirely
```

### Transferability by Role Archetype

Adjust transferability scoring based on role archetype (from research-prompts.md):

```
BUILDER roles: Value range and breadth of transferable experience
  → Weight transferable more generously (+5 pts bonus if highly transferable)

SPECIALIST roles: Transferable carries lower weight than direct
  → Weight transferable more conservatively (-5 pts if not in same specialty)

SCALER roles: Transferability from same scale level is key
  → 0→1M users at any company transfers to 1M→10M context
  → Small team management doesn't transfer to 500-person org

STRATEGIST roles: Strategic thinking transferable regardless of domain
  → Strategy at nonprofit transfers to strategy at tech company
```

---

## DIMENSION 3 — Adjacent Match Score (20 points)

**Definition:** The bullet demonstrates a related skill, tool, or problem space that signals capability even if not directly matching. Useful for showing range and learning potential.

### Adjacency Map

Use this map to determine adjacency score. Adjacent pairs share methodology, mental models, or problem structure:

```
TECHNICAL ADJACENCIES:
  Python ↔ R ↔ Julia (data science languages)
  AWS ↔ GCP ↔ Azure (cloud platforms — same concepts, different UI)
  Kubernetes ↔ Docker Swarm (container orchestration)
  React ↔ Vue ↔ Angular (frontend frameworks)
  PostgreSQL ↔ MySQL ↔ SQLite (relational databases)
  Spark ↔ Flink ↔ Kafka (data streaming)
  TensorFlow ↔ PyTorch (deep learning)
  Terraform ↔ CloudFormation (infrastructure as code)

FUNCTIONAL ADJACENCIES:
  Product management ↔ Program management (different ownership, same rigor)
  Data analysis ↔ Business intelligence (overlapping methods)
  Customer success ↔ Account management (different focus, same relationship skill)
  DevOps ↔ Site reliability engineering (different framing, same goal)
  Growth marketing ↔ Product-led growth (different driver, same outcome)
  UX research ↔ Customer discovery (same method, different context)

DOMAIN ADJACENCIES:
  B2B SaaS ↔ Enterprise software (same buying dynamics)
  Fintech ↔ Finance (domain knowledge transfers)
  Healthcare ↔ Healthtech (regulatory context transfers)
  E-commerce ↔ Marketplace (transaction dynamics transfer)
  Gaming ↔ Consumer apps (engagement mechanics transfer)
```

### Adjacency Scoring

```
20 pts — DIRECTLY ADJACENT:
  Tool/skill/domain is one step removed
  Learning curve: days to weeks
  Example: Has AWS experience → applying to GCP role

12 pts — MODERATELY ADJACENT:
  Related methodology or mental model
  Learning curve: weeks to months
  Example: Has B2B SaaS experience → applying to enterprise software role

6 pts — LOOSELY ADJACENT:
  Same broad category, meaningful differences
  Learning curve: months
  Example: Has consumer product experience → applying to B2B product role

0 pts — NOT ADJACENT:
  Different domain, methodology, and mental models
```

---

## DIMENSION 4 — Impact Alignment Score (10 points)

**Definition:** Does the outcome type of the bullet match what this role values? A cost-savings bullet in a revenue-growth role is weaker than the reverse, even if the method is identical.

### Impact Type Classification

```
CLASSIFY BULLET IMPACT AS:
  Revenue/Growth: "grew ARR by", "acquired X customers", "increased conversion"
  Cost/Efficiency: "reduced costs by", "saved X hours", "improved throughput"
  Quality/Reliability: "achieved 99.9% uptime", "reduced bugs by", "zero incidents"
  Scale: "serving X users", "across N countries", "processing $XM/day"
  Speed: "shipped in X weeks", "reduced time-to-market by", "in half the time"
  People/Org: "grew team from X to Y", "promoted Z engineers", "reduced attrition"
  Strategic: "defined roadmap", "established partnership", "entered new market"
  Customer: "NPS improved by", "customer satisfaction", "reduced churn by"
```

### Impact Alignment Matrix

Map bullet impact type to what the role values (from role archetype + JD):

```
ROLE ARCHETYPE → MOST VALUED IMPACT TYPES:

BUILDER: Speed, Scale, Strategic → 10 pts
         Revenue/Growth → 8 pts
         Cost/Efficiency, Quality → 5 pts

SCALER: Scale, Revenue/Growth → 10 pts
        Speed, Quality → 8 pts
        Cost/Efficiency → 6 pts

OPERATOR: Cost/Efficiency, Quality, Reliability → 10 pts
          Speed, People/Org → 7 pts
          Revenue/Growth → 4 pts

STRATEGIST: Strategic, Revenue/Growth → 10 pts
            People/Org, Scale → 8 pts
            Cost/Efficiency → 5 pts

SPECIALIST: Quality, Reliability, Technical depth → 10 pts
            Speed, Scale → 7 pts
            Revenue/Growth → 5 pts

COLLABORATOR: People/Org, Strategic → 10 pts
              Customer, Revenue/Growth → 7 pts
              Cost/Efficiency → 5 pts
```

---

## Composite Score Calculation Examples

### Example 1 — Strong Match

```
TEMPLATE SLOT: Senior PM role (SCALER archetype)
SEEKING: "Experience scaling a B2B SaaS product from 10 to 100 enterprise clients"

CANDIDATE BULLET:
"Grew enterprise customer base from 8 to 94 accounts in 24 months by 
 redesigning onboarding flow and launching dedicated CSM program, 
 increasing NRR from 105% to 142%"

SCORING:
  Direct Match: 38/40
    Domain ✓ (B2B SaaS), Scale ✓ (similar range), Outcome ✓ (growth + NRR)
    Keywords: "enterprise" exact match (+3), "SaaS" implied, "onboarding" adjacent

  Transferable: 28/30
    Core capability (enterprise growth) highly transferable
    Same business model, same customer type, highly similar scale

  Adjacent: 18/20
    Customer success / NRR management directly adjacent to product ownership
    Strong signal of business understanding

  Impact Alignment: 10/10
    Revenue/Growth + Scale → both top-valued for SCALER archetype

COMPOSITE SCORE:
  (38 × 0.40) + (28 × 0.30) + (18 × 0.20) + (10 × 0.10)
  = 15.2 + 8.4 + 3.6 + 1.0
  = 28.2 / 40 → normalized to 100 = 91%

BAND: DIRECT (90-100%)
```

### Example 2 — Transferable Match

```
TEMPLATE SLOT: Same senior PM role
SEEKING: "Experience with enterprise sales cycles and procurement processes"

CANDIDATE BULLET:
"Partnered with sales team on 23 enterprise deals >$500K, creating 
 custom ROI frameworks that reduced average deal cycle from 6 to 4 months"

SCORING:
  Direct Match: 22/40
    Domain partial (sales-adjacent, not sales itself), Method ✓ (enterprise deals),
    Outcome ✓ (cycle reduction) — candidate partnered with sales, didn't run sales

  Transferable: 26/30
    Business context fully transferable (enterprise, large deals, procurement)
    Core insight (deal acceleration) transfers strongly

  Adjacent: 15/20
    Sales enablement adjacent to sales cycle knowledge
    ROI frameworks = strong signal of business/commercial acumen

  Impact Alignment: 8/10
    Revenue/Growth → high value for SCALER, indirect but clear

COMPOSITE SCORE:
  (22 × 0.40) + (26 × 0.30) + (15 × 0.20) + (8 × 0.10)
  = 8.8 + 7.8 + 3.0 + 0.8
  = 20.4 / 40 → normalized to 100 = 79%

BAND: TRANSFERABLE (75-89%)
NOTE: Strong despite not being a direct match — candidate partnered on deals,
      understands the cycle without owning it. Good reframing opportunity.
```

---

## Content Reframing System

When a bullet scores >60% but terminology or emphasis doesn't align with the JD, apply reframing. **Reframing changes presentation, never facts.**

### Reframing Type 1 — Keyword Alignment

Preserve meaning, substitute their terminology from the terminology map:

```
ORIGINAL: "Built customer journey maps for the onboarding flow"
TARGET ROLE USES: "user research", "discovery", "UX insights"

REFRAMED: "Conducted user research and discovery sessions to map 
           onboarding experience, surfacing 7 friction points"

CHANGE: "customer journey maps" → "user research and discovery"
        Added "surfacing 7 friction points" to add outcome
TRUTHFUL BECAUSE: Journey mapping IS a form of user research/discovery
```

### Reframing Type 2 — Emphasis Shift

Same facts, different focus — highlight the aspect most relevant to this role:

```
ORIGINAL: "Led migration of monolith to microservices architecture,
           reducing deployment time from 4 hours to 12 minutes"

FOR TECHNICAL ROLE: Emphasize the technical achievement
  REFRAMED: "Architected monolith-to-microservices migration using 
             Docker + Kubernetes, achieving 20x deployment speed improvement"

FOR MANAGEMENT ROLE: Emphasize the leadership achievement
  REFRAMED: "Led 8-engineer migration initiative across 18 months,
             delivering 20x deployment speed improvement on schedule"

CHANGE: Same achievement, different hero (architecture vs. leadership)
TRUTHFUL BECAUSE: Both aspects actually happened
```

### Reframing Type 3 — Abstraction Adjustment

Move up or down the specificity ladder to match role expectations:

```
TOO SPECIFIC (move up for generalist/strategy roles):
  ORIGINAL: "Optimized PostgreSQL query performance using EXPLAIN ANALYZE,
             reducing p99 latency from 2.3s to 180ms for the billing service"
  
  REFRAMED: "Identified and resolved critical database performance bottleneck,
             improving API response times by 12x for core billing system"

TOO GENERIC (move down for technical/specialist roles):
  ORIGINAL: "Improved system performance significantly"
  
  REFRAMED: "Reduced database query latency by 12x through query 
             optimization and indexing strategy"

TRUTHFUL BECAUSE: The details still exist in the original bullet
                  Only the presentation level changed
```

### Reframing Type 4 — Scale Emphasis

When scale is the differentiator or the requirement:

```
ORIGINAL: "Built data pipeline for the analytics team"

SCALE REFRAME (if role values scale):
  "Built real-time data pipeline processing 4TB/day, 
   enabling analytics for 3 business units"

BUSINESS IMPACT REFRAME (if role values business outcomes):
  "Built data pipeline reducing analytics team's reporting 
   time from 3 days to 2 hours per cycle"

TRUTHFUL BECAUSE: Add actual numbers from experience — 
                  never invent, only surface what's real
```

### Reframing Rules — Non-Negotiable

```
ALWAYS ALLOWED:
  ✅ Change terminology to match their vocabulary
  ✅ Emphasize different aspects of the same achievement
  ✅ Add context that was omitted from original bullet
  ✅ Move abstraction level up or down
  ✅ Reorder elements within a bullet
  ✅ Add a metric that was left out (if you know the real number)

NEVER ALLOWED:
  ❌ Change the metric (87% cannot become 90%)
  ❌ Imply direct ownership of work done in partnership
  ❌ Claim a technology was used if it wasn't
  ❌ Upgrade scope ("I helped" cannot become "I led")
  ❌ Invent an outcome that didn't happen
  ❌ Change the company, date, or title
```

---

## Bullet Selection Strategy — Anti-Repetition Rules

When assembling the full resume, apply these rules to avoid repetition:

### Rule 1 — No Duplicate Themes Across Consecutive Bullets

```
BAD (3 consecutive bullets all about cost reduction):
  • Reduced cloud costs by $2M through rightsizing
  • Cut operational expenses by 30% via automation
  • Saved $500K annually by renegotiating vendor contracts

GOOD (varied themes):
  • Reduced cloud costs by $2M through rightsizing (efficiency)
  • Shipped real-time fraud detection system, blocking $8M in losses (technical)
  • Led cross-functional team of 12 to deliver Q3 roadmap (leadership)
```

### Rule 2 — No Duplicate Technology Mentions (Unless Required)

```
If "Python" appears in bullet 1, don't mention Python again until 3 bullets later.
Exception: If role explicitly requires Python + the slot specifically needs Python proof.

Same rule applies to: specific company names, specific team names, 
specific product names.
```

### Rule 3 — Impact Type Variety

```
Across all bullets for one role, ensure at least 3 different impact types:
  Not all efficiency bullets
  Not all technical bullets
  Not all growth bullets
  
  Mix: 1-2 technical/product + 1-2 scale/growth + 1 leadership/collaboration
       (adjust based on role archetype)
```

### Rule 4 — Recency Weighting

```
For the SAME bullet appearing in multiple source resumes (different versions):
  Always prefer the version from the most recent resume
  More recent = likely more refined, updated metrics, better framing

For roles where recency matters to the JD:
  Front-load with recent experiences
  Deprioritize bullets from roles 5+ years ago
  Unless the old role is uniquely relevant
```

---

## Gap Handling — When No Good Match Exists

When the best available bullet scores <60% for a template slot:

### Gap Type 1 — Skill Gap (Never Done This)

```
IDENTIFY: User has no experience in this area at all

PROCESS:
1. Check if experience discovery was run — if not, suggest it
2. If discovery was run and nothing found, this is a genuine gap
3. Assess severity: Is this a CRITICAL or IMPORTANT requirement?

IF CRITICAL requirement:
  "❌ Critical Gap: {requirement}
   Best available match: {score}% — {bullet text}
   
   This is a must-have requirement with no strong match in your library.
   
   OPTIONS:
   A. Use best available ({score}%) with honest framing
   B. Address directly in cover letter with learning trajectory
   C. Consider whether this application is the right fit at this time
   D. Run experience discovery — you may have unlocked experience here
   
   I recommend {A or B based on severity} because {reason}."

IF IMPORTANT requirement:
  "⚠️ Gap: {requirement}
   Best available: {score}% — {bullet text}
   I'll use best available and flag this for cover letter."
```

### Gap Type 2 — Partial Match (Done Something Adjacent)

```
IDENTIFY: User has adjacent experience that partially covers the requirement

PROCESS:
1. Score the partial match
2. Identify which aspects transfer and which don't
3. Create strongest possible honest reframing
4. Show confidence before and after reframing

PRESENTATION:
  "PARTIAL MATCH: {requirement}
   
   ORIGINAL (score: {X}%):
   '{original bullet}'
   
   REFRAMED (score: {Y}%):
   '{reframed bullet}'
   
   WHAT TRANSFERS: {specific aspects}
   WHAT'S MISSING: {honest gap}
   
   REFRAMING IS TRUTHFUL BECAUSE: {explanation}
   
   Use reframed version? (Y/N)"
```

### Gap Type 3 — Outdated Experience (Did This, But Long Ago)

```
IDENTIFY: User did this but it was 5+ years ago, may be outdated

PROCESS:
1. Use the experience with date context
2. If technology/method has changed significantly, flag it
3. Suggest supplementing with a more recent adjacent bullet

PRESENTATION:
  "⚠️ Dated Experience: {requirement}
   
   Found relevant experience but from {year} ({N} years ago).
   Technology/practice may have evolved since then.
   
   OPTION A: Use this experience, acknowledge recency gap in interview
   OPTION B: Pair with a more recent adjacent bullet to show continued relevance
   OPTION C: Note in cover letter with framing around keeping skills current
   
   What's your preference?"
```

---

## Match Presentation Format

For every template slot, present the analysis in this format:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLOT: {Role} — Bullet {N}
SEEKING: {What this slot needs, in plain English}
JD REQUIREMENT TYPE: {Critical / Important / Bonus}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

MATCH 1 — {BAND} ({score}%)
"{bullet text}"
  Direct:      {score}/40 — {brief reason}
  Transferable: {score}/30 — {brief reason}
  Adjacent:    {score}/20 — {brief reason}
  Impact:      {score}/10 — {brief reason}
  Quality:     {score}/100 (×{multiplier})
  Source:      {resume_name.md}
  
  {IF REFRAMING AVAILABLE:}
  REFRAME OPTION:
    Original:  "{original}"
    Reframed:  "{reframed}"
    Why safe:  "{truthfulness explanation}"
    New score: {improved score}%

MATCH 2 — {BAND} ({score}%)
"{bullet text}"
  [same structure]

MATCH 3 — {BAND} ({score}%)
"{bullet text}"
  [same structure]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RECOMMENDATION: Match 1 ({score}%)
REASON: {Why this is the best choice for this specific slot}
ALTERNATIVE: Match 2 if avoiding repetition of {theme/keyword}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Full Resume Assembly Checklist

Before finalizing the assembled resume, run this check:

```
COVERAGE CHECK:
  □ All CRITICAL requirements have a bullet addressing them
  □ Most IMPORTANT requirements addressed
  □ No slot left empty without explanation

QUALITY CHECK:
  □ No two consecutive bullets have the same impact type
  □ No technology mentioned more than twice in same role section
  □ All bullets start with a strong action verb (Tier 1 preferred)
  □ Every bullet has at least one metric
  □ No bullet exceeds 160 characters

NARRATIVE CHECK:
  □ First bullet of most recent role is strongest overall
  □ Resume tells a coherent story when read top-to-bottom
  □ Theme from success profile is visible throughout
  □ Title reframing is consistent with bullet content

ATS CHECK:
  □ All CRITICAL ATS keywords appear at least once
  □ IMPORTANT keywords appear in natural context
  □ No keyword stuffing (same keyword max 3 times total)
  □ Skills section includes all JD-specified technical skills

TRUTHFULNESS CHECK:
  □ Every metric is real
  □ Every reframing has been verified as accurate
  □ No scope inflation (solo work not presented as team lead)
  □ No company/title/date alterations
```
