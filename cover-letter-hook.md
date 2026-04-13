# Cover Letter Hook

## Overview

Generates a tailored cover letter immediately after Phase 4 resume generation, using research already completed — no duplicate work. By this point, the skill has built a complete success profile, identified cultural signals, mapped narrative themes, and knows exactly how the user's background aligns (and where it doesn't). The cover letter writes itself from that foundation.

**Core Principle:** The research is already done. The cover letter is just telling the story the resume proves.

---

## When to Run

**Trigger:** Immediately after Phase 4 generation, after ATS simulation.

```
"Your resume is ready (ATS Score: {score}/100).

Would you like a matching cover letter?
I already have everything I need from the research phase — 
this will take about 2 minutes.

→ YES: Generate now
→ CUSTOM: I want to guide the tone/emphasis
→ NO: Skip cover letter"
```

---

## What the Hook Uses (Already Available)

Everything needed is already in memory from earlier phases. No re-research needed.

```
FROM PHASE 0 (Library):
  - User's name and contact info
  - Career narrative arc (progression of roles)
  - Key differentiators (what makes user distinctive)
  - Strongest bullets (highest-scoring content)

FROM PHASE 1 (Research):
  - Company mission, values, culture signals
  - Role requirements (must-have vs nice-to-have)
  - Success profile (what ideal candidate looks like)
  - Terminology map (their language, not generic language)
  - Risk factors (gaps) + mitigations
  - Narrative themes identified for this role

FROM PHASE 2-3 (Template + Assembly):
  - Approved role consolidation decisions
  - Title reframing choices made
  - JD coverage score + gap list
  - Newly discovered experiences (if any)
  - User's strongest 2-3 relevant bullets (highest match score)
```

---

## Cover Letter Structure

Four paragraphs. No fluff. Every sentence earns its place.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PARAGRAPH 1 — THE HOOK (2-3 sentences)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What: Why this role and this company — specifically.
Goal: Prove you've done your research. Show genuine alignment.
Source: Company mission/values from Phase 1 research

DO: Reference something specific about the company 
    (recent initiative, mission, specific product, team focus)
DON'T: "I am writing to apply for the position of..."
DON'T: Generic enthusiasm with no specifics

Template:
"{Company}'s focus on {specific thing from research} 
is exactly the kind of {challenge/mission/problem} I've spent 
{X} years building toward. I'm applying for {Role} because 
{specific reason tied to research — not just "great opportunity"}."

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PARAGRAPH 2 — THE PROOF (3-4 sentences)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What: 2 strongest, most relevant achievements — briefly.
Goal: Make the claim that you can do this job.
Source: Top 2 highest-match-score bullets from Phase 3

DO: Use exact metrics from the resume bullets
DO: Frame in terms of what it means for THIS role
DON'T: List every achievement (that's what the resume is for)
DON'T: Repeat resume bullet verbatim — paraphrase with context

Template:
"At {Company}, I {achievement 1 — condensed with metric}.
 This {connects to role requirement because...}.
 More recently, {achievement 2 — condensed with metric},
 which {connects to this company's specific challenge}."

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PARAGRAPH 3 — THE FIT (2-3 sentences)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What: Why you fit this company culturally and why you want THIS role.
Goal: Differentiate from equally qualified candidates.
Source: Cultural signals + narrative themes from Phase 1

DO: Use company's own language and values (from terminology map)
DO: Address the narrative theme identified for this role
DO: If gaps exist, address ONE proactively (strength framing)
DON'T: List soft skills ("I'm a team player, hard worker...")

Template:
"What draws me specifically to {Company} is {cultural signal
 from research — specific, not generic}. {Optional: one-sentence
 gap address — 'While my background is in X, I've consistently...'}
 I'm looking for {what this role offers that matters to you}."

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PARAGRAPH 4 — THE CLOSE (1-2 sentences)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What: Clear call to action. No hedging.
Goal: Sound confident, not desperate.

DO: Express interest in conversation, not just the job
DON'T: "I would be honored and grateful for the opportunity..."
DON'T: "Please find my resume attached" (unnecessary)

Template:
"I'd welcome the opportunity to discuss how my experience 
in {specific area} could contribute to {specific team/goal at company}.
{Optional: something that shows you know what they're working on}."
```

---

## Tone Calibration

Before generating, detect appropriate tone from Phase 1 research:

```
COMPANY CULTURE SIGNALS → TONE:

Startup / growth stage:
  Signals: "move fast", "wear many hats", "scrappy", "build from 0→1"
  Tone: Direct, energetic, shows bias for action, comfortable with ambiguity

Enterprise / established:
  Signals: "scale", "process", "cross-functional alignment", "governance"
  Tone: Professional, measured, emphasizes collaboration and proven track record

Mission-driven / nonprofit:
  Signals: "impact", "equity", "community", "purpose"
  Tone: Genuine, value-aligned, specific about why the mission matters to you

Technical / engineering-led:
  Signals: Engineering blog culture, open source, technical depth emphasis
  Tone: Show technical credibility early, specifics over generalities

Creative / brand-led:
  Signals: Design quality, taste, aesthetic, consumer-facing
  Tone: More expressive, demonstrates taste and cultural awareness

APPLY TONE by adjusting:
  - Vocabulary choices (their words, not generic words)
  - Sentence rhythm (shorter/punchier for startups, fuller for enterprise)
  - What achievement to lead with (impact vs. process vs. growth)
```

---

## Gap Handling in Cover Letters

If resume has gaps (JD coverage <75% or identified risk factors from Phase 1):

```
RULE: Address maximum ONE gap in the cover letter.
      Addressing multiple signals you know you're under-qualified.

GAP ADDRESSING FRAMEWORK:
  1. Don't apologize for the gap
  2. Redirect to transferable strength
  3. Show learning trajectory if applicable

TEMPLATES BY GAP TYPE:

Technical skill gap:
  "While my hands-on {missing skill} experience has been in adjacent 
   contexts, my track record of rapidly adopting new technologies — 
   including {example} — means this gap has a short half-life."

Domain/industry gap:
  "My background is in {your domain} rather than {their domain}, 
   but the core challenge of {shared problem} is one I've tackled 
   directly at scale — and the tools transfer."

Seniority gap (applying above current level):
  "Though my current title is {X}, the scope of my work has 
   consistently been at the {target} level — {specific proof point}."

Career transition gap:
  "I'm making a deliberate move from {domain} to {new domain} 
   because {specific genuine reason — must be honest}."
```

---

## Generation Process

**Step 1: Assemble context block from memory**
```
Pull from session memory:
- company_name, role_title
- top_cultural_signal (1 specific thing from research)
- top_2_bullets (highest match-score bullets from Phase 3)
- narrative_theme (from Phase 1 success profile)
- gap_to_address (if any — choose most important only)
- tone_profile (from calibration above)
- user_name, contact_info
```

**Step 2: Generate draft**
```
Apply structure + tone + context
Target length: 250-350 words (3/4 page max)
Never exceed 400 words
```

**Step 3: Present with score**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
COVER LETTER DRAFT
{Name} → {Role} at {Company}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

{Full letter text}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
WORD COUNT: {N} words
TONE: {detected tone}
APPROACH USED: {what was emphasized and why}

RESEARCH USED:
  Hook source: "{specific thing referenced}" (from Phase 1 research)
  Proof bullets: {bullet 1 source}, {bullet 2 source}
  Cultural signal: "{signal used}" (from {source})
  Gap addressed: {gap} or "None — coverage was strong"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

How does this feel?
→ LOOKS GOOD — generate files
→ ADJUST TONE — more {formal/casual/direct/warm}
→ DIFFERENT EMPHASIS — change what achievements to lead with
→ TWEAK PARAGRAPH — {specify which one and what to change}
→ START OVER — I'll ask you a few quick questions instead
```

---

## Revision Options

**Tone adjustment:**
```
User: "More direct / less formal"
→ Shorten sentences, cut filler phrases,
  lead with achievement not context

User: "Warmer / more personal"
→ Add one sentence about personal connection to company mission
  Use "I" more actively, less passive constructions

User: "More technical credibility"
→ Add one specific technical term from resume in paragraph 2
  Reference technical challenge explicitly
```

**Emphasis change:**
```
User: "Lead with the leadership angle, not the technical one"
→ Swap paragraph 2 bullet order
  Reframe hook around team/org challenge rather than technical one

User: "I want to address the career transition more directly"
→ Add gap address sentence to paragraph 3
  Strengthen "why now, why this domain" in paragraph 1
```

**Quick questions mode (if user selects "Start Over"):**
```
I'll ask 3 quick questions to make this more personal:

1. What drew you to {Company} specifically? 
   (One genuine thing — not "great culture" or "exciting work")

2. Which of your achievements are you most proud of that 
   feels most relevant to this role?

3. Is there anything you want the hiring manager to understand 
   about your background that the resume doesn't fully capture?

[Generate from answers]
```

---

## Output Files

Generate alongside resume files:

**Markdown version:**
```
{Name}_{Company}_{Role}_CoverLetter.md
```

**DOCX version:**
```
{Name}_{Company}_{Role}_CoverLetter.docx

Formatting:
- Same header as resume (name + contact)
- Date: {current date}
- Recipient: "Hiring Manager" (unless name known from research)
- Body: single-spaced, 11pt Calibri, 1-inch margins
- Closing: "Best regards," or "Sincerely," based on tone
- Signature block: {Name}
```

**Include in Generation Report (`_Report.md`):**
```markdown
## Cover Letter
- Generated: Yes / No
- Tone: {detected tone}
- Word count: {N}
- Hook source: {what was referenced}
- Gap addressed: {gap or "none"}
- Revisions made: {N}
```

---

## Library Integration

If cover letter is saved to library:

```
Save: {Name}_{Company}_{Role}_CoverLetter.md → resume library
Tag with:
  - target_role, target_company
  - tone used
  - achievements featured
  - gap_addressed
```

Future use: when applying to similar roles, the skill can reference 
past cover letters as tone/structure templates.

---

## Multi-Job Mode Integration

In multi-job mode (from SKILL.md), cover letters are generated per-job:

```
After all resumes approved:
"Generate cover letters for all {N} jobs?

SHARED ELEMENTS (used for all):
  - Your career narrative
  - Core achievements

UNIQUE PER JOB:
  - Company-specific hook
  - Tone calibration
  - Gap handling

Generate all {N} cover letters? (~1 min each)"
```

Batch generates all cover letters using per-job research from Phase 3.
Each is unique — same person, different emphasis per company.
