# ATS Simulation Module

## Overview

Simulates how an Applicant Tracking System parses and scores the resume **before a human ever sees it**. Runs automatically at the end of Phase 4, after generation. Produces an ATS Score (0-100) with specific, actionable fixes.

**Core Principle:** A perfectly tailored resume that fails ATS parsing is invisible. This module ensures the resume survives the robot before reaching the human.

---

## When to Run

**Trigger:** Automatically after Phase 4 generation, before presenting files to user.

```
"Before I hand you these files, let me run an ATS simulation to make sure 
this resume actually gets seen by a human..."

[Run ATS Simulation]

"ATS Score: {score}/100 — {band}"
```

Always run. Never skip. Never make it optional — the user doesn't always know to ask for it.

---

## ATS Score Calculation

### Five Scoring Dimensions

**Total Score = Keyword Score + Format Score + Structure Score + Length Score + Readability Score**

---

### Dimension 1: Keyword Match Score (35 points)

**What ATS systems care about most:** Exact and near-exact keyword presence from the JD.

**Process:**

1. Extract all meaningful keywords from JD:
   - Hard skills (tools, technologies, languages, platforms)
   - Soft skills explicitly named ("cross-functional collaboration", "stakeholder management")
   - Certifications or credentials mentioned
   - Role-specific terminology (exact phrases used in JD)
   - Action domains ("built", "led", "architected", "shipped")

2. Scan final resume text for each keyword:
   - EXACT match = full credit
   - STEM match (e.g. "manage" matches "management") = 80% credit
   - SYNONYM match = 50% credit (flag for user to consider exact term)
   - MISSING = 0 credit

3. Weight by JD keyword frequency:
   - Keywords appearing 3+ times in JD = Critical (3x weight)
   - Keywords appearing 2 times = Important (2x weight)
   - Keywords appearing once = Standard (1x weight)

**Scoring:**
```
keyword_score = (weighted_hits / weighted_total) × 35

Band:
  32-35: Excellent keyword coverage
  25-31: Good, minor gaps
  15-24: Moderate — missing some critical terms
  0-14:  Poor — significant keyword mismatch
```

**Output format:**
```
KEYWORD ANALYSIS:
✅ Matched (Critical): Python, Machine Learning, cross-functional
✅ Matched (Important): roadmap, stakeholder, OKR
⚠️  Stem Match: "manage" → JD uses "management" (consider exact term)
⚠️  Synonym: "built" → JD uses "developed" (consider swapping)
❌  Missing (Critical): "A/B testing" — appears 4x in JD, not in resume
❌  Missing (Important): "SQL" — appears 2x in JD
❌  Missing (Standard): "agile", "scrum"

Keyword Score: 27/35
```

---

### Dimension 2: Format Compatibility Score (25 points)

**What ATS systems choke on:** Non-standard formatting that breaks parsers.

**Check each item — deduct points per failure:**

| Format Element | Check | Deduction |
|---|---|---|
| Tables | None present | -8 pts |
| Text boxes | None present | -7 pts |
| Multi-column layout | Single column only | -6 pts |
| Headers/footers with content | Contact info in body, not header | -4 pts |
| Images, icons, graphics | None present (markdown is safe) | -5 pts |
| Non-standard fonts | Uses standard fonts only | -2 pts |
| Horizontal lines as dividers | Used sparingly | -1 pt |
| Unicode bullets (•, ✓, →) | Use standard dash or hyphen | -3 pts |

**For DOCX output specifically:**
- Verify no text boxes were used for sections
- Verify bullets are proper list formatting, not unicode symbols
- Verify no inline images

**Scoring:**
```
format_score = 25 - sum(deductions)
Minimum: 0

Band:
  23-25: ATS-safe formatting
  15-22: Minor issues, likely parseable
  8-14:  Moderate risk — some sections may be skipped
  0-7:   High risk — significant parsing failure likely
```

**Output format:**
```
FORMAT ANALYSIS:
✅ No tables detected
✅ Single-column layout
✅ No text boxes
⚠️  Unicode bullets (•) detected in 3 places → swap to "-" for safety
❌  Contact info in document header → ATS may skip it → move to body

Format Score: 20/25
FIX REQUIRED: Move contact info to body text, replace unicode bullets
```

---

### Dimension 3: Section Structure Score (20 points)

**What ATS systems expect:** Recognizable section names.

**Standard section names ATS parsers look for:**

```
RECOGNIZED:           PROBLEMATIC (rename):
"Experience"          "Career Journey"
"Work Experience"     "Where I've Been"
"Professional Experience" "My Story"
"Education"           "Academic Background" (gray area)
"Skills"              "What I Bring"
"Summary"             "About Me" (gray area)
"Certifications"      "Credentials" (may parse)
"Projects"            "What I've Built" (may fail)
```

**Check:**
1. Identify all section headers in resume
2. Map each to recognized ATS term
3. Flag non-standard names
4. Verify required sections present: Experience, Education, Skills (minimum)

**Scoring:**
```
Per section:
  Standard name = 4 pts
  Near-standard (e.g. "Professional Experience") = 3 pts
  Ambiguous (e.g. "Background") = 2 pts
  Non-standard (e.g. "My Journey") = 0 pts

section_score = min(sum(section_scores), 20)
```

**Output format:**
```
SECTION STRUCTURE:
✅ "Professional Experience" → recognized
✅ "Education" → recognized
✅ "Skills" → recognized
⚠️  "What I've Built" → rename to "Projects" for ATS safety
❌  "About Me" → rename to "Summary" or "Professional Summary"

Section Score: 16/20
```

---

### Dimension 4: Length & Density Score (10 points)

**What matters:** Resume length signals seniority appropriateness. Over-dense resumes get truncated.

**Rules:**

```
Experience level → Expected length:
  0-3 years:   1 page (> 1 page = risk of truncation)
  4-8 years:   1-2 pages
  9+ years:    2 pages max (3+ = almost always truncated)
  Executive:   2 pages (CVs are different)

Bullet count per role:
  Most recent role: 4-6 bullets (optimal)
  Previous roles: 2-4 bullets
  Older roles (5+ years ago): 1-2 bullets max

Bullet length:
  Optimal: 1-2 lines (80-120 characters)
  Too short (<50 chars): not enough substance
  Too long (>150 chars): may wrap awkwardly, ATS may truncate
```

**Scoring:**
```
Page length appropriate = 5 pts
Bullet count per role appropriate = 3 pts
Bullet length appropriate = 2 pts

length_score = sum of above
```

**Output format:**
```
LENGTH & DENSITY:
✅ 2 pages — appropriate for 10 years experience
✅ Most recent role: 5 bullets — optimal
⚠️  Role from 2018: 6 bullets — trim to 2-3 (older roles should be brief)
⚠️  2 bullets exceed 150 characters — consider splitting or trimming

Length Score: 7/10
```

---

### Dimension 5: Readability Score (10 points)

**What ATS systems parse better:** Clean, consistent text patterns.

**Check:**

```
Consistency checks:
- Date format consistent? (MM/YYYY or YYYY or "Month YYYY" — pick one)
- Bullet format consistent? (all start with action verb = good)
- Tense consistent? (past roles in past tense, current in present)
- Abbreviations defined? ("NLP" ok if common; "MRDQ" needs spelling out)

Red flags:
- Unexplained gaps visible in dates → flag for user awareness
- Job-hopping signal (3+ roles in 2 years) → flag
- Dates missing from any role → ATS may mis-sort experience
```

**Scoring:**
```
Date format consistent = 3 pts
Bullet format consistent = 3 pts
Tense consistent = 2 pts
No unexplained abbreviations = 2 pts

readability_score = sum of above
```

**Output format:**
```
READABILITY:
✅ Dates consistent (MM/YYYY format)
✅ All bullets start with action verbs
⚠️  Tense inconsistency: Current role uses past tense in 2 bullets → fix
⚠️  "MRDQ" used without definition — spell out on first use

Readability Score: 7/10
```

---

## Final ATS Report

Present to user as a clean summary after all five dimensions run:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ATS SIMULATION REPORT
{Name} → {Role} at {Company}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

OVERALL ATS SCORE: {total}/100  [{band}]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DIMENSION SCORES:
  Keyword Match:      {score}/35
  Format Safety:      {score}/25
  Section Structure:  {score}/20
  Length & Density:   {score}/10
  Readability:        {score}/10
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SCORE BANDS:
  85-100: Excellent — high ATS pass probability
  70-84:  Good — likely to pass most systems
  55-69:  Moderate — may fail stricter ATS
  0-54:   At Risk — needs fixes before submission

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CRITICAL FIXES (do before submitting):
  1. {Fix 1 — specific, actionable}
  2. {Fix 2}
  ...

RECOMMENDED IMPROVEMENTS:
  1. {Improvement 1}
  2. {Improvement 2}
  ...

KEYWORDS TO ADD:
  Critical missing: {list}
  Suggested swaps:  {original} → {exact JD term}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Apply fixes? (Y/N)
→ YES: I'll update the resume and regenerate files
→ NO: Resume saved as-is with this report attached
```

---

## Fix Application Process

If user says YES to applying fixes:

1. Apply all CRITICAL fixes automatically (no re-confirmation needed)
2. For RECOMMENDED improvements, ask:
   ```
   "For recommended improvements — apply all, or pick specific ones?
   [List each with Y/N choice]"
   ```
3. For missing keywords:
   ```
   "I can add these missing keywords to your Skills section:
   {list of keywords}
   
   Or if you'd prefer, I can integrate them naturally into bullet text.
   Which approach? (Skills section / Bullets / Both / Skip)"
   ```
4. After fixes applied: re-run ATS simulation automatically
5. Show delta:
   ```
   "ATS Score improved: {old_score} → {new_score} (+{delta} points)"
   ```
6. Regenerate files with fixed version

---

## ATS Score in Generation Report

Add ATS score to the `_Report.md` file:

```markdown
## ATS Analysis
- ATS Score: {score}/100 ({band})
- Keyword Coverage: {percentage}%
- Critical Fixes Applied: {N}
- Missing Keywords at Submission: {list or "none"}
- Format Safety: {Safe / Minor Issues / At Risk}
```

---

## Company-Specific ATS Notes

During Phase 1 research, if company is known to use a specific ATS, note it:

```
Known ATS systems and their quirks:
- Workday: Strict about section names, parses PDFs less reliably than DOCX
- Greenhouse: Generally permissive, handles modern formatting better
- Taleo (Oracle): Very strict — single column, standard sections, avoid PDFs
- Lever: Modern parser, handles more formatting
- iCIMS: Conservative — treat like Taleo
- SAP SuccessFactors: Use DOCX, not PDF; strict section naming
- ADP: Conservative — plain formatting safest
- Jobvite: Moderate — handles standard formatting well

If company ATS unknown: apply conservative rules by default
```

If detected, add note to ATS report:
```
"⚠️  {Company} likely uses {ATS System}. 
Specific recommendation: {tailored advice for that ATS}"
```
