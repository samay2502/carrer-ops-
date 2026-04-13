# Bullet Quality Engine

## Overview

Scans every bullet in the resume library during Phase 0 initialization, scores each on five quality dimensions, flags weak bullets, and co-writes upgraded versions. The library improves with every session — compounding value over time.

**Core Principle:** The skill is only as good as the raw material it pulls from. A weak bullet reframed is still a weak bullet. Fix the source, not just the application.

---

## When to Run

**Trigger:** End of Phase 0 (Library Initialization), after library is built and indexed, before Phase 1 begins.

**Always run silently first.** Only surface results if weak bullets are found.

```
[Phase 0 complete — library built]
[Running bullet quality scan...]

IF all bullets score ≥70: proceed silently, no interruption
IF any bullet scores <70: surface report to user
```

---

## Bullet Quality Scoring

Each bullet is scored across five dimensions. **Total = 100 points.**

---

### Dimension 1: Action Verb Strength (25 points)

**What makes an action verb strong:** Specific, high-agency, outcome-oriented. Weak verbs are vague, passive, or responsibility-focused rather than achievement-focused.

**Verb Tiers:**

```
TIER 1 — STRONG (25 pts):
Architected, Spearheaded, Engineered, Launched, Drove, Scaled, 
Delivered, Negotiated, Secured, Pioneered, Transformed, Rebuilt, 
Reduced, Increased, Generated, Closed, Shipped, Deployed, Designed,
Established, Implemented (when paired with outcomes)

TIER 2 — ACCEPTABLE (15 pts):
Managed, Led, Built, Created, Developed, Improved, Analyzed,
Collaborated, Coordinated, Supported, Executed, Maintained,
Contributed, Assisted (for older/junior roles)

TIER 3 — WEAK (5 pts):
Helped, Worked on, Was responsible for, Participated in,
Involved in, Assisted with, Tried to, Attempted

PASSIVE (0 pts):
"Was tasked with", "Duties included", "Responsible for",
any bullet starting with a noun instead of a verb
```

**Scoring:**
```
action_score = tier_score (25, 15, or 5)
passive = 0, flag as CRITICAL fix
```

---

### Dimension 2: Metric / Quantification (30 points)

**The most important dimension.** Numbers make bullets concrete and credible.

**Metric Types:**

```
HARD METRIC — full credit (30 pts):
  Numbers: "reduced latency by 40%", "grew revenue by $2.3M"
  Scale: "across 12 teams", "serving 4M users", "50+ stakeholders"
  Time: "in 6 weeks", "3x faster than previous approach"
  Rank/position: "#1 in region", "top 5% of cohort"

SOFT METRIC — partial credit (18 pts):
  Relative: "significantly reduced", "doubled throughput"
  Comparative: "outperformed baseline", "above target"
  Scope without number: "enterprise-wide", "company-wide initiative"

NO METRIC — low credit (5 pts):
  Pure description with no quantification or scale signal
```

**Special Cases:**
- Confidential numbers: "Reduced churn by [confidential %]" → 18 pts (better than nothing)
- Range: "$1M-$3M revenue impact" → 25 pts
- Implied metric ("the only engineer on the team") → 18 pts

---

### Dimension 3: Outcome / Impact Clarity (25 points)

**What was the result? Why did it matter?**

```
CLEAR OUTCOME (25 pts):
  States what happened as a result of the work.
  Example: "...reducing customer churn by 18% and saving $400K annually"
  
  Structure hint: [Action] + [What] + [Result/Impact]

PARTIAL OUTCOME (15 pts):
  Action and what, but result implied or vague.
  Example: "Led migration of 40 microservices to Kubernetes"
  (migration happened, but no stated outcome)

TASK DESCRIPTION (5 pts):
  Describes what was done with no result at all.
  Example: "Worked on the backend API for the payments service"

NO OUTCOME (0 pts):
  Pure responsibility statement.
  Example: "Responsible for backend development"
```

---

### Dimension 4: Specificity (15 points)

**Generic bullets don't differentiate. Specific bullets stick.**

```
HIGHLY SPECIFIC (15 pts):
  Named technologies, real context, concrete details.
  "Migrated legacy Oracle DB to Postgres on AWS RDS, 
   cutting infra costs by 35% and eliminating 12hr monthly downtime"

MODERATELY SPECIFIC (10 pts):
  Some specificity but missing key context details.
  "Migrated database to cloud, reducing costs significantly"

GENERIC (5 pts):
  Could apply to almost anyone in that role.
  "Improved system performance and reduced costs"

BOILERPLATE (0 pts):
  Reads like a job description, not an achievement.
  "Developed and maintained software applications"
```

---

### Dimension 5: Length & Readability (5 points)

```
OPTIMAL (5 pts): 80-130 characters, one clear sentence
ACCEPTABLE (3 pts): 50-79 or 131-160 characters
TOO SHORT (1 pt): <50 characters — not enough substance
TOO LONG (0 pts): >160 characters — will wrap and confuse readers
TWO-SENTENCE (0 pts): Bullets should be one sentence only
```

---

## Scoring Formula & Quality Bands

```
bullet_score = action_score + metric_score + outcome_score + 
               specificity_score + length_score

QUALITY BANDS:
  85-100: STRONG    — use as-is
  70-84:  GOOD      — use with minor polish
  50-69:  WEAK      — upgrade before matching
  0-49:   POOR      — must upgrade or exclude

```

---

## Phase 0 Quality Scan Report

After scoring all bullets, present summary:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BULLET QUALITY SCAN — LIBRARY REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total bullets scanned: {N}

STRONG (85+):   {N} bullets ({%}) ✅
GOOD (70-84):   {N} bullets ({%}) ✅
WEAK (50-69):   {N} bullets ({%}) ⚠️
POOR (<50):     {N} bullets ({%}) ❌

Library Health Score: {avg_score}/100
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

{IF all strong/good}:
"Your library is in great shape — all bullets are strong.
 Proceeding to research phase."

{IF weak/poor bullets found}:
"Found {N} bullets that could be significantly stronger.
 Upgrading these now will improve match quality for every
 future resume — not just this one.
 
 Quick upgrade session? (~5-8 min for {N} bullets)
 → YES: Let's upgrade them now
 → NO: Proceed with existing bullets
 → LATER: Upgrade after this resume is done"
```

---

## Upgrade Process

If user selects YES:

**For each weak/poor bullet, show:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BULLET UPGRADE: {Role} at {Company}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CURRENT BULLET (Score: {score}/100):
"{original bullet text}"

ISSUES:
  ❌ Weak action verb: "Helped" → needs stronger verb
  ❌ No metric: no numbers or scale
  ⚠️  No outcome: describes task, not result

WHAT I NEED FROM YOU:
  To write a strong version, help me fill in the blanks:

  1. What specifically did you do? (be concrete)
  2. What was the scale or scope? 
     (team size / users affected / budget / timeframe)
  3. What was the result? What got better?
     (%, $, time saved, problem solved)

  (Skip any you don't remember — I'll work with what you give me)
```

**After user responds, generate upgrade:**

```
UPGRADED VERSION:
"{strong rewritten bullet}"
Score: {new_score}/100 ✅

Changes made:
  ✅ Verb upgraded: "Helped" → "Architected"
  ✅ Metric added: "{user's number}"
  ✅ Outcome explicit: "{result stated}"

OPTIONS:
  1. USE THIS VERSION — replace in library
  2. TWEAK IT — adjust wording
  3. KEEP ORIGINAL — not worth changing
  4. WRITE MY OWN — I'll rephrase from scratch
```

**If user skips (can't remember details):**

```
"No problem — I'll flag this bullet as LOW CONFIDENCE in matching.
 It can still be used but won't rank as highly as stronger bullets.
 
 If you remember the details later, we can upgrade it in Phase 5."
```

---

## Batch Upgrade Mode

If 5+ weak bullets found, offer batch mode to save time:

```
"You have {N} weak bullets. I can either:

A) GUIDED MODE — upgrade one at a time with questions (thorough, ~8 min)
B) DRAFT MODE — I write best-guess upgrades based on context, 
                you approve/edit each (~3 min)
C) SKIP — proceed without upgrading

For DRAFT MODE, I'll make reasonable assumptions about scale and outcome
based on the company context. You correct what I get wrong.

Which approach?"
```

**Draft Mode process:**
- Claude writes upgraded version using company context + common sense
- Marks each with: "⚠️ ASSUMED: [what was assumed] — please verify"
- User confirms, edits, or rejects each

---

## Library Update After Upgrades

When upgrades are confirmed:

1. Replace original bullets in library database (not in source files unless user requests)
2. Keep original as `original_text` field for reference
3. Tag upgraded bullets: `quality_upgraded: true`, `upgraded_date: {date}`
4. Save upgraded versions to source markdown file only if user explicitly requests:
   ```
   "Save these upgrades back to your source resume files?
   → YES: I'll update {resume_name}.md with the new versions
   → NO: Upgrades live in this session only (you'll see them again 
         if you add those resumes to future sessions)"
   ```

---

## Quality Score in Generation Report

Add library health to `_Report.md`:

```markdown
## Library Quality Summary
- Library Health Score: {avg}/100
- Bullets Upgraded This Session: {N}
- Weak Bullets Used (not upgraded): {N} — see flagged bullets below
- Flagged Bullets: {list with scores}
```

---

## Recurring Library Improvement

Each time the skill runs and user upgrades bullets, track progress:

```
IF library metadata exists from previous session:
  Compare current health score to previous
  
  "Library Health: {prev_score} → {new_score} (+{delta})
   Improvement since last session: {N} bullets upgraded"
```

This makes the system visibly self-improving — the user sees their library getting stronger over time.

---

## Quick Reference: Common Upgrade Patterns

```
PATTERN: "Helped with X"
→ "Engineered X, resulting in {outcome}"

PATTERN: "Responsible for X"
→ "Owned and delivered X, achieving {metric}"

PATTERN: "Worked on X team"
→ "Contributed to X team's {project}, driving {result}"

PATTERN: "Managed a team of engineers"
→ "Led {N}-person engineering team delivering {project} in {timeframe}"

PATTERN: "Improved system performance"
→ "Reduced {specific metric} by {%} through {method}, enabling {outcome}"

PATTERN: "Created dashboards for stakeholders"
→ "Built {N} executive dashboards tracking {KPIs}, adopted by {N} teams 
   for {decision type} decisions"
```
