# Branching Questions — Experience Discovery System

## Overview

This file contains the complete conversation system for Phase 2.5 — the experience discovery interview. The goal is to surface real experiences the user has never written down, convert them into strong resume bullets, and fill gaps in the library.

**Core Principle:** Most people undersell themselves not because they lack experience, but because they haven't connected what they've done to what employers value. This interview closes that gap.

**Non-negotiable rule:** Every experience captured must be real. The interviewer's job is to help articulate and frame — never to invent or exaggerate.

---

## Interview Architecture

### Before You Start

Assess which gaps need discovery (from Phase 3 assembly attempt):

```
PRIORITIZE GAPS BY:
1. CRITICAL requirements with <60% match → MUST address in discovery
2. IMPORTANT requirements with <70% match → SHOULD address
3. Bonus requirements with any match → SKIP (not worth time)

THEN ASSESS GAP TYPE:
  TYPE A — Skill gap: "Do you have experience with X at all?"
  TYPE B — Undocumented: "You've probably done this but never wrote it down"
  TYPE C — Framing: "You have this but it's not obvious from your resume"

TYPE B and C gaps are the highest-value targets — 
most people have done more than their resume shows.
```

### Setting the Stage

Open the discovery session with this framing:

```
"Before I finalize your template, I want to run a quick brainstorm.

I've found {N} gaps between what {Company} is looking for and 
what's in your resume library. In my experience, most of these 
aren't real gaps — they're things you've done but never wrote down.

I'll ask you some questions. There are no wrong answers.
Even vague memories are useful — I'll help you turn them into bullets.

This should take about {10-15} minutes.

Ready? Let's start."
```

---

## QUESTION BANK — By Gap Type

### BANK 1 — Technical Skills Gaps

Use when: JD requires a specific tool/technology not in library.

**Level 1 — Opening Probe:**
```
"Have you ever worked with {skill/tool}?
Even in a small way — a side project, a hackathon, 
a class, a work experiment?"
```

**Branch A — YES (strong direct experience):**
```
"Tell me about it. What specifically did you build or do?
→ [Listen]
How big was it? (users/data/scale/team size)
→ [Listen]
What was the result or outcome?
→ [Listen]
Was this in production? For real users?
→ [Listen]
Can we put a number on the impact — time saved, users served, 
performance improvement, cost reduced?"
```
→ Capture as strong bullet. High confidence.

**Branch B — INDIRECT (used adjacent tool/different context):**
```
"You haven't used {skill} directly, but you mentioned {adjacent tool}.
Those are closely related — same underlying concepts.

Walk me through a time you used {adjacent tool} to solve a real problem.
→ [Listen]
What was the scale of that work?
→ [Listen]
What did you learn from it that would apply to {skill}?"
```
→ Capture as transferable experience. Medium confidence.
→ Frame in resume as: "Strong foundation in {adjacent}, 
   experienced with {related concept}"

**Branch C — PERSONAL PROJECT:**
```
"Have you ever explored {skill} outside of work?
Any personal projects, open source contributions, 
side projects, or courses where you applied it?"
→ [If yes]
"Tell me about that project. What problem were you solving?
How far did you get? Is it still running?"
→ Capture as: personal project bullet if substantive
  Flag as: personal/non-professional context
```

**Branch D — NEVER (genuine gap):**
```
"That's useful to know. Let me check one more angle:
Have you ever had to learn a new technology quickly on the job?
→ [If yes]
Tell me about that situation — what did you learn and how fast?
This demonstrates learning agility, which often matters more 
than the specific tool."
```
→ Capture as: learning agility bullet (doesn't fill the tool gap 
   but fills "adaptability" which is often an implicit requirement)

---

### BANK 2 — Leadership & Management Gaps

Use when: JD requires people management, team leadership, or mentorship 
          not documented in library.

**Level 1 — Opening Probe:**
```
"Has anyone ever reported to you directly, even informally?
Or have you led a team without the official title?"
```

**Branch A — FORMAL MANAGEMENT:**
```
"How many people? For how long?
→ [Listen]
What did you do as their manager — did you run 1:1s, 
set goals, do performance reviews?
→ [Listen]
Tell me about someone you helped grow or develop.
What happened to them?
→ [Listen]
Was there a moment where your leadership made a measurable difference
to a project or outcome?"
```
→ Capture as formal leadership bullet with team size and outcome.

**Branch B — INFORMAL LEADERSHIP:**
```
"Have you ever been the go-to person for a junior team member?
Someone who came to you for guidance even though you weren't their manager?
→ [If yes]
Tell me about that. How did you help them?
Did you mentor them formally or informally?
How did it go?"
```
→ Capture as mentorship bullet.
→ Frame as: "Mentored {N} junior engineers..." or "Served as technical 
   lead and informal mentor to..."

**Branch C — PROJECT LEADERSHIP:**
```
"Have you ever led a project where you had to coordinate people
who didn't report to you? 
Maybe across teams, or with contractors, or with a vendor?
→ [If yes]
Walk me through that. How many people? How long? What was the outcome?
Did it ship on time? Under budget? What was the impact?"
```
→ Capture as cross-functional leadership bullet.
→ Frame as: "Led cross-functional team of {N}..."

**Branch D — TEACHING / KNOWLEDGE SHARING:**
```
"Have you ever run a training session, workshop, or lunch-and-learn?
Created onboarding materials? Written internal documentation 
that others used?
→ [If yes]
Tell me about it. How many people attended or used it?
Did you get any feedback on whether it helped?"
```
→ Capture as knowledge sharing / enablement bullet.

---

### BANK 3 — Scale & Impact Gaps

Use when: Library bullets lack quantification or role requires 
          demonstrated impact at scale.

**Level 1 — Opening Probe:**
```
"Looking at your library, I see strong experience but 
I'm not seeing numbers in several places.
Let's try to find some.

Think about your last role at {Company}. 
What's the biggest outcome you're proud of?"
```

**Metric Discovery Sequence:**
```
USERS / CUSTOMERS:
"How many users or customers did your work affect?
Even a rough number — thousands? Millions?
Was this B2B (fewer, larger) or B2C (more, smaller)?"

REVENUE / BUSINESS IMPACT:
"Did your work have any connection to revenue — 
either growing it, protecting it, or saving costs?
Any dollar figure you can point to, even approximately?"

TIME / EFFICIENCY:
"Did anything you built save people time?
How much time, for how many people, how often?"

SCALE / VOLUME:
"What was the scale of the systems you worked on?
Requests per second? Data processed? Transactions per day?"

TEAM / ORG SIZE:
"How many engineers, PMs, or stakeholders did you work with?
What was your span of influence?"

IMPROVEMENT / BEFORE-AFTER:
"Was there a before-state and an after-state you can describe?
What was bad before you, and what was better after?"
```

**If user genuinely doesn't know the numbers:**
```
"That's okay — can we estimate?
If your work reduced manual processing time, roughly how long 
did it used to take vs. now?
[Or] If your team had X engineers and everyone used your tool daily, 
what would the time savings be?

Estimated ranges are acceptable on a resume — for example,
'reduced processing time by an estimated 60%' is honest and credible
if you explain your reasoning."
```

---

### BANK 4 — Cross-Functional & Stakeholder Gaps

Use when: JD requires collaboration across teams, executive communication,
          or stakeholder management not visible in library.

**Level 1 — Opening Probe:**
```
"Tell me about a time you had to get something done 
with people who didn't report to you — and the stakes were real."
```

**Branch A — EXECUTIVE EXPOSURE:**
```
"Have you ever presented to senior leadership — VP level or above?
→ [If yes]
What was the context? What were you presenting?
How many people in the room? What happened as a result?
Did your presentation change a decision or unlock resources?"
```
→ Capture as: "Presented {X} to {VP/C-suite/board}, resulting in..."

**Branch B — CROSS-TEAM PROGRAMS:**
```
"Have you ever been the person who held a multi-team project together?
Not necessarily the official PM — but the person who made sure 
everyone was aligned and moving?
→ [If yes]
How many teams? What was at stake?
What was the hardest part of the coordination?
What was the outcome?"
```
→ Capture as cross-functional program leadership bullet.

**Branch C — CONFLICT / ALIGNMENT:**
```
"Has there been a situation where two teams or stakeholders 
disagreed on something important, and you had to help resolve it?
→ [If yes]
Walk me through that.
What was the disagreement?
How did you navigate it?
What was the outcome?"
```
→ Capture as stakeholder alignment / influence bullet.
→ High value signal for senior roles — "navigating ambiguity" is a 
  real senior skill.

---

### BANK 5 — Strategy & Ownership Gaps

Use when: JD requires strategic thinking, product ownership, 
          roadmap definition, or business acumen not in library.

**Level 1 — Opening Probe:**
```
"Has there been a moment where you saw a problem or opportunity 
that no one else was working on, and you decided to own it yourself?"
```

**Branch A — SELF-DIRECTED INITIATIVE:**
```
"Tell me about that.
→ [Listen]
What made you decide to pick this up?
How did you get others to buy in?
What did you actually do?
What was the outcome?"
```
→ Capture as initiative/ownership bullet.
→ Frame as: "Identified and led unprompted initiative to..."

**Branch B — ROADMAP / PRIORITIZATION:**
```
"Have you ever had to decide between competing priorities 
when you couldn't do everything?
→ [If yes]
What was the situation?
How did you make the decision?
What framework or criteria did you use?
What happened as a result?"
```
→ Capture as prioritization / product judgment bullet.

**Branch C — CUSTOMER / MARKET INSIGHT:**
```
"Have you ever talked directly to customers or users 
to understand their problems?
→ [If yes]
How many conversations? Over what period?
What did you learn that changed something — 
a product decision, a feature direction, a strategy?
Can you point to a specific outcome that came from that insight?"
```
→ Capture as customer research / product discovery bullet.

---

### BANK 6 — Unique & Differentiating Experiences

Use when: Looking for experiences that set user apart from typical candidates.

**These questions surface unexpected gold:**

```
QUESTION 1 — THE FOUNDER QUESTION:
"Have you ever started something from zero?
Could be a product, a team, a program, a process, a community —
anything that didn't exist before you decided to create it."

QUESTION 2 — THE DIFFICULT SITUATION:
"Tell me about the hardest professional situation you've navigated.
Not necessarily your biggest win — the hardest thing to deal with.
What happened? What did you do? What would you do differently?"
(This surfaces resilience, judgment, and maturity — hard to fake)

QUESTION 3 — THE EXTERNAL FOOTPRINT:
"Have you done anything outside your day job that's professionally relevant?
Speaking at a conference, writing articles, contributing to open source,
advising a startup, volunteering with a technical skill?"
(Often huge differentiators that people forget to include)

QUESTION 4 — THE RECOGNITION QUESTION:
"Has your work ever been recognized externally?
Awards, publications, being cited, getting acquired, press coverage?"
(Often forgotten entirely)

QUESTION 5 — THE NUMBERS NOBODY KNOWS:
"What's a number from your work that would surprise people?
A scale, a result, a speed, a metric — something that sounds impressive 
but you've never said out loud because it felt obvious to you?"
(Most powerful bullet mining question — people genuinely forget their wins)
```

---

## Experience Capture Template

For every experience surfaced, capture it in this structure immediately:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISCOVERED EXPERIENCE #{N}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RAW (what user said):
  "{their words, verbatim}"

CONTEXT:
  Where: {Company / project / context}
  When: {Approximate year or role}
  Duration: {How long}
  Scope: {Scale — team size / users / budget / other}

GAPS ADDRESSED:
  {Which template slot(s) this fills}
  Confidence: {High / Medium / Low}

DRAFTED BULLET:
  "{Strong action verb + what + metric + outcome}"

QUALITY SCORE:
  Action verb: {Tier 1/2/3}
  Metric present: {Y/N + what}
  Outcome clear: {Y/N + what}
  Specificity: {High/Medium/Low}
  Estimated score: {X}/100

INTEGRATION OPTIONS:
  □ Add to current resume
  □ Add to library only
  □ Needs more detail before using
  □ Discard

VERIFICATION:
  □ User confirmed all details are accurate
  □ Numbers are real (not estimated without basis)
  □ Scope is not inflated
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Bullet Drafting From Discovery

After capturing raw experience, co-write the bullet:

### Step 1 — Show the draft

```
"Here's a draft based on what you told me:

'{drafted bullet}'

Does this accurately capture what happened?"
```

### Step 2 — Improve if needed

```
IF metric is vague: "Can we make the number more specific?
  You said 'significant improvement' — do you remember roughly
  what percentage or how much time?"

IF outcome is missing: "What was the result — what got better,
  faster, cheaper, or bigger because of this?"

IF verb is weak: "Let me try a stronger opening — instead of 
  'helped with', what if we say 'led' or 'built' or 'designed'?
  Which feels most accurate to your actual role?"

IF scope is unclear: "How would you characterize the scale of this?
  Was this for a small team, the whole company, external customers?"
```

### Step 3 — Finalize and confirm

```
"Final version:
'{final bullet}'

This is honest and accurate? Nothing overstated?
[Wait for confirmation]

Quality score: {X}/100
I'll add this to your library and the current resume."
```

---

## Pacing and User Management

### Time Management

```
SESSION BUDGET:
  1-3 gaps: ~8-10 minutes
  4-6 gaps: ~12-15 minutes
  7+ gaps: ~15-20 minutes (offer break)

PER GAP BUDGET: ~3-4 minutes maximum
  If no gold found after 2-3 branches: move on
  "I don't think we're going to find something strong here — 
   let's move to the next gap."

FATIGUE SIGNALS:
  Short answers, "I don't know", "not really" repeatedly
  → Check in: "Are you doing okay? Want to take a quick break 
    or push through?"
  → If user seems done: "Let's wrap up — we've found 
    {N} good experiences. That's valuable. Let me work with what we have."
```

### Handling "I Don't Know" or "Not Really"

```
WHEN USER SAYS "I DON'T KNOW":
  "That's okay — let me ask it differently:
  [Try a different angle from the same bank]"

  If still nothing after 2 attempts:
  "No problem — this might just be a genuine gap.
  I'll note it and we can address it in the cover letter or 
  in how you talk about it in interviews."

WHEN USER UNDERSELLS:
  "Wait — what you just described actually sounds significant.
  You said you 'helped with' the migration — but you designed 
  the architecture, right? That's not helping, that's leading.
  Can we be more precise about your actual role?"

WHEN USER OVERSELLS (rare but important to catch):
  "I want to make sure we're being accurate here — 
  you mentioned you 'led' this initiative, but earlier you said 
  your manager set the direction. Was your role more execution 
  than strategy? That's fine — let's be precise."
```

### Ending the Session

```
"We've completed the discovery session.

FOUND: {N} new experiences
  ✅ {Experience 1} — adds {X}% coverage improvement
  ✅ {Experience 2} — fills {gap name}
  ✅ {Experience 3} — library enrichment

STILL GAPS: {N} remaining
  ⚠️  {Gap 1} — will address in cover letter
  ⚠️  {Gap 2} — best available match will be used

Your library is now richer. These experiences are saved 
permanently for future applications too.

Ready to move to assembly? [Y/N]"
```
