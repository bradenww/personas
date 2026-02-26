# Build Persona — Skill Guide

A systematic process for building an AI coaching persona from a public figure's own words and documented thinking. The output is a set of structured markdown files that power an AI coach, advisor, or virtual cofounder in any LLM-based agent framework.

## Overview

**Input:** A public figure's name and domain
**Output:** A persona directory containing:
- `CLAUDE.md` -- Character-first behavioral blueprint + situational module routing table (the system prompt)
- `sources.md` -- Metadata catalog of all sources (titles, dates, URLs, transcript method). No copyrighted content — just references.
- 15-25 situational modules -- Deep knowledge base organized by conversational context (3-6K tokens each)
- `synthesis/taxonomy-guide.md` -- Module design + frequency analysis
- `synthesis/extractions/` -- Per-source structured extractions

Raw source files (transcripts, essays, etc.) are stored in the top-level `sources/[persona-name]/` directory, which is gitignored. Source material is copyrighted and should not be committed to public repositories.

**Cost:** Typically $1-5 in transcription (AssemblyAI at $0.01/min) + LLM API costs for synthesis agents. Most transcripts can be found free online.

**Timeline:** ~60-90 minutes wall-clock on Claude Code Max (Phases 1-2 are human-paced source collection; Phases 3-7 are ~20-30 minutes of agent work).

---

## Phase 1: Source Identification

### Step 1: Start with the person's own writing
This is always the highest-signal source material -- refined, intentional, in their own words.

| Person Type | Primary Writing |
|---|---|
| Business leader | Shareholder letters, memos, blog posts, books |
| Scientist | Papers, books, public lectures |
| Politician | Speeches, policy papers, memoirs |
| Creator | Essays, newsletters, published interviews |
| Military leader | After-action reports, memoirs, doctrine documents |

Search for comprehensive collections first (e.g., Quartr for shareholder letters, university archives for lectures). One collection page can yield decades of material.

### Step 2: Identify domain-relevant interviewers
Different domains have different canonical interviewers. Build the list based on who interviews people in this person's world:

| Domain | Key Interviewers / Shows |
|---|---|
| Tech founders | Lex Fridman, Acquired podcast, How I Built This, Tim Ferriss, Code Conference (Mossberg/Swisher), re:Invent/re:MARS, Charlie Rose (historical) |
| Finance | Bloomberg TV, CNBC (Squawk Box, Sorkin), Dealbook Summit, Economic Club events, Milken Conference |
| Science | Lex Fridman, Sean Carroll (Mindscape), StarTalk, TED, Royal Institution lectures |
| Politics | 60 Minutes, Meet the Press, congressional testimony archives, C-SPAN, Foreign Affairs |
| Military | War on the Rocks, Pritzker Military Museum, service academy lectures, AFA conferences |
| General/historical | Charlie Rose (pre-2017), Dick Cavett (historical), Fresh Air (NPR), The Knowledge Project |

Search pattern: `"[Person Name] interview"`, `"[Person Name] full interview"`, `"[Person Name] keynote"`, `"[Person Name] [interviewer name]"`

### Step 3: Search for rare/lesser-known appearances
After the obvious sources, cast a wider net:
- **X/Twitter search** (via Grok): `"[Person Name] rare interview OR full interview OR podcast OR keynote speech"` -- fans often surface obscure content
- **YouTube search:** `"[Person Name] full"` sorted by duration (long videos = full interviews)
- **University archives:** Commencement speeches, guest lectures, honorary degree ceremonies
- **Conference archives:** TED, re:Invent, Milken, Davos, Allen & Co Sun Valley
- **Government archives:** Congressional testimony (congress.gov), regulatory hearings
- **Podcast search:** Listen Notes, Podscripts.co
- **Internet Archive:** Often has full broadcast recordings that have been scrubbed elsewhere
- **Show-specific archives:** charlierose.com, americanrhetoric.com, c-span.org

### Step 4: Prioritize sources
Rank by signal quality:

1. **Own writing** -- Most refined thinking. Always collect completely.
2. **Long-form interviews (30+ min)** -- Unscripted depth. Highest value per minute.
3. **Keynotes/speeches** -- Rehearsed but reveals priorities and communication style.
4. **Testimony/formal statements** -- Precise, under pressure, reveals frameworks.
5. **Short interviews (<15 min)** -- Lower density but may contain unique moments.
6. **Panel discussions** -- Low content density per person. Only collect if the person dominates.
7. **Commentary about them** -- Secondary source. Useful as a "how others see them" layer but not primary material.

---

## Phase 2: Source Acquisition

### Rule: Search for free transcripts BEFORE paying for transcription

Follow this search order for each source:

1. **Official publisher sites** -- The organization that hosted the event often publishes transcripts (princeton.edu, aboutamazon.com, congress.gov, TED.com)
2. **Podcast transcript aggregators** -- podscripts.co, rev.com/blog, happyscribe
3. **Show-specific archives** -- charlierose.com has full human-generated transcripts; many shows archive their content
4. **Professional transcription sites** -- rev.com often has high-profile speech transcripts free
5. **Blog posts / fan transcriptions** -- Search `"[Person Name] [event] transcript"`
6. **Web search via Grok** -- `--web` flag on x-search.py to search broadly

**Do NOT use YouTube auto-generated transcripts.** They are low quality -- poor punctuation, no speaker labels, frequent errors on proper nouns and technical terms.

**Only pay for transcription when no quality transcript exists online.** Use the `/video-transcript` skill (see `.claude/skills/video-transcript/SKILL.md`) which handles AssemblyAI transcription with speaker diarization, YouTube audio download, speaker label verification, and cleanup. Cost: ~$0.01/minute (~$0.60/hour).

### Source Acquisition Strategy (Lessons Learned)

In practice, most transcripts for well-known public figures can be found free online. For the Bezos persona (22 sources):
- **18 transcripts found free** -- official publisher sites (princeton.edu, aboutamazon.com), show archives (charlierose.com), conference organizers, blogs, fan transcriptions
- **4 required AssemblyAI** -- Summit Series 2017, Code Conference 2016, re:Invent 2012, Blue Origin 2019 (all available on YouTube but no quality transcripts existed online)

**Best sources for free transcripts:**
- `charlierose.com` -- Full human-generated transcripts for historical interviews
- University websites -- Commencement speeches and guest lectures almost always published
- `congress.gov` -- Full testimony transcripts
- Conference organizers -- re:Invent, TED, Milken often publish transcripts
- News outlets -- GeekWire, The Verge, Bloomberg often have full interview transcripts for high-profile subjects
- `aboutamazon.com` / company sites -- Shareholder letters, keynotes, press events

### File format
Save each source as an individual `.md` file in `sources/` with a metadata header:
```markdown
# [Person Name] -- [Source Title] ([Year])
- **Date:** YYYY-MM-DD
- **Type:** Interview / Speech / Testimony / Lecture
- **Duration:** X minutes
- **Interviewer:** [Name] (if applicable)
- **Source:** [Where the transcript came from]
- **URL:** [Link to original]

[Full transcript text]
```

---

## Phase 3: Extraction (Per-Source, Parallel)

Launch **one Opus agent per source document**. Each agent reads only its assigned source and produces a structured extraction. All agents can run in parallel.

**Batch in groups of 5** to avoid Opus rate limit hits. As each agent completes, backfill to keep 5 active.

### Extraction Prompt Template

```
Read [PERSONA_DIR]/sources/[SOURCE_FILE] and extract a structured summary with these sections:

1. **Key Themes** -- The 3-5 main topics or ideas discussed
2. **Principles & Beliefs** -- Any rules, standards, or convictions stated or implied.
   For each: name it, quote the exact words, explain what it means in practice.
3. **Decision Frameworks** -- Any described processes for making decisions.
   For each: name it, quote the exact words, explain when to use it.
4. **Mental Models & Metaphors** -- Analogies, frameworks, or lenses used to explain ideas.
5. **Management & Leadership** -- Anything about teams, hiring, culture, organization.
6. **Stories & Anecdotes** -- This is critical. For each story:
   - **Name:** A short label (e.g., "The Grandmother's Cigarettes," "The Door Desk")
   - **Full Narrative:** Tell the story completely with exact quotes where available
   - **Teaching Application:** What lesson does this story illustrate? In what conversational
     situation would you deploy it?
   - **Source Citation:** Timestamp or page reference
7. **Notable Quotes** -- 10-20 best direct quotes (exact words, with context)
8. **Biographical Details** -- Personal stories, formative experiences, background
9. **Unique Content** -- Anything in this source that likely doesn't appear in other sources
10. **Situational Context Suggestions** -- For each principle, framework, and story above,
    suggest 1-3 conversational situations where it would be useful.
    Example: "Regret Minimization Framework" -> "When someone is debating leaving a stable
    job," "When someone needs to make a scary irreversible decision"
11. **Conversational Technique** -- How they engage, not just what they say. Include:
    - **Questioning patterns:** Do they answer questions with questions? Do they reframe
      the question itself before answering? Do they challenge the premise?
    - **Metaphor density and style:** Do they think in metaphors or use them sparingly?
      Are their metaphors poetic, physical, scientific, everyday?
    - **Paradox and tension:** Do they resolve contradictions or hold them? Do they
      present opposing truths side by side?
    - **Emotional calibration:** How do they modulate intensity? Do they escalate
      gradually or deliver hard truths suddenly? How do they create safety before
      confrontation?
    - **Multilingual elements:** Any code-switching, foreign phrases, or etymological
      references that are part of their voice?
    - **Rhetorical structures:** Do they answer in threes? Pair opposites? Build to a
      single devastating sentence? Use repetition for emphasis?
    - **What they DON'T do:** Do they avoid giving direct advice? Refuse to take sides?
      Never use jargon? This is as revealing as what they do.

For each item, include the exact quote where possible with a page/timestamp reference.
Write output to: [PERSONA_DIR]/synthesis/extractions/[SOURCE_NAME]-extraction.md
```

### Practical Notes

- **Always one agent per source.** Don't combine sources into a single agent -- the whole point is isolation. A large source in a combined batch crowds out the others.
- **Agent output files appearing on disk does NOT mean the agent is done.** Sub-agents may write an initial file, then re-read, edit, or rewrite it. Always wait for the task completion notification before using outputs.
- Stories are first-class entities here, not afterthoughts. The explicit "Stories & Anecdotes" section with named stories, full narratives, and teaching applications enables frequency counting and tagging in Phase 4.
- Conversational Technique (Section 11) captures *how* the person engages, not just *what* they say. This is critical for subjects whose voice IS their content (therapists, poets, spiritual teachers) vs. subjects who communicate through frameworks (CEOs, engineers). Without it, the persona captures knowledge but not presence.
- Situational context suggestions enable bottom-up taxonomy construction -- especially valuable for non-famous subjects where you can't design the taxonomy top-down.

---

## Phase 4: Taxonomy (Single Convergent Agent)

After ALL Phase 3 extractions are complete, launch a **single Opus agent** that reads every extraction file and designs the module structure for the entire persona.

This is convergent work -- one agent must see everything to make coherent routing decisions. Do not parallelize this phase.

### What the Taxonomy Agent Produces

1. **15-25 situational modules** organized by conversational context -- not intellectual category.
   - "Starting a company" not "Decision-making"
   - "Dealing with failure" not "Resilience"
   - "Building teams and hiring" not "Management philosophy"
   - Each module maps to a real situation a user would bring to a coaching conversation

2. **Frequency counting** using `[N/TOTAL]` notation showing how many sources mention each concept.
   - Example: `Regret Minimization Framework -- Freq: 12/22 -- [SIGNATURE] [EMPHATIC]`

3. **Emphasis flags:**
   - `[SIGNATURE]` -- a concept so closely identified with the person that it's part of their public identity
   - `[EMPHATIC]` -- the person explicitly calls this their most important principle, repeats it with unusual force, or self-declares it as foundational

4. **Importance ordering** -- principles within each module ordered by frequency + emphasis (highest first)

5. **Cross-reference plan** -- which module is the primary home for each concept, which modules get cross-references. Every concept has exactly one primary home.

### Taxonomy Prompt Template

```
You are designing the situational module taxonomy for a [PERSON_NAME] coaching persona.

Read ALL extraction files in [PERSONA_DIR]/synthesis/extractions/

Your job is to organize [PERSON_NAME]'s thinking into 15-25 SITUATIONAL modules. Each module
maps to a conversational context a user might bring -- NOT an intellectual category.

Good module names (situational):
- "Starting a Company / Entrepreneurial Leap"
- "Making Hard Decisions"
- "Dealing with Failure and Criticism"
- "Building Teams and Hiring"

Bad module names (intellectual):
- "Decision-Making Frameworks"
- "Leadership Philosophy"
- "Resilience and Grit"
- "Innovation Theory"

For EACH module, provide:

## Module N: [Situational Title]

**Trigger:** [1-2 sentences describing when a user would trigger this module -- what they say,
what situation they're in]

### Concepts (by importance)

1. **[Concept Name]** -- Freq: N/[N_SOURCES] -- [SIGNATURE]? [EMPHATIC]?
   - Sources: [list of source file names that contain this concept]
   - [1-2 sentence summary with best quote]
   - Emphasis: [why this is flagged EMPHATIC, if applicable]

2. **[Next concept]** ...

### Stories in This Module
- [Story Name] -- from [source]. Teaching application: [when to deploy this story]

### Cross-References
- [Concept X] is primarily in Module Y but relevant here

## Ordering Rules
- Modules ordered by breadth of source coverage (most widely discussed topics first)
- Concepts within each module ordered by frequency + emphasis (highest first)
- Every concept, story, and framework has exactly ONE primary module
- Cross-references point to the primary home

## Notation
- **Frequency: N/[N_SOURCES]** -- how many source documents contain this concept
- **[EMPHATIC]** -- [PERSON_NAME] explicitly calls this their most important principle or
  delivers it with unusual force/repetition
- **[SIGNATURE]** -- a concept so closely identified with [PERSON_NAME] that it's part of
  their public identity

Write output to: [PERSONA_DIR]/synthesis/taxonomy-guide.md
```

### Example Output Structure

From the Bezos taxonomy (see `jeff-bezos-teams-v2/synthesis/taxonomy-guide.md`):

```markdown
## Module 1: Starting a Company / Entrepreneurial Leap

**Trigger:** User is considering starting a company, leaving a stable job, or evaluating
whether to take an entrepreneurial risk.

### Concepts (by importance)

1. **Regret Minimization Framework** -- Freq: 12/22 -- [SIGNATURE] [EMPHATIC]
   - Sources: 60-minutes-1999, charlie-rose-1999-04, princeton-commencement-2010, ...
   - Bezos projected to age 80 and asked: "Will I regret not trying?"
   - Emphasis: Called this "the framework that made the decision incredibly easy"

2. **Missionaries vs Mercenaries** -- Freq: 8/22 -- [EMPHATIC]
   - Sources: economic-club-2018, reinvent-fireside-2012, lex-fridman, ...
   - "Missionaries build better products. They have better energy."
   ...
```

---

## Phase 5: Module Synthesis (Divergent, N Parallel Agents)

Launch **one Opus agent per module**, using the taxonomy guide as the blueprint. Each agent reads the taxonomy guide plus ALL extraction files and produces a single situational module.

**Batch in groups of 5** to avoid rate limits. Backfill as each completes to keep 5 active.

### Module Synthesis Prompt Template

```
You are writing situational module [MODULE_NUMBER]: "[MODULE_TITLE]" for the [PERSON_NAME]
coaching persona.

Read:
1. [PERSONA_DIR]/synthesis/taxonomy-guide.md (the full taxonomy -- your blueprint)
2. ALL extraction files in [PERSONA_DIR]/synthesis/extractions/

Your module covers THIS situational context:
[PASTE THE MODULE'S TRIGGER DESCRIPTION AND CONCEPT LIST FROM THE TAXONOMY]

## Output Format

Write a 3-6K token module with this structure:

### [Module Title]

[1-2 sentence overview of what this module covers and when to use it]

#### [Concept 1 Name] [N/TOTAL] [SIGNATURE]? [EMPHATIC]?

[Full treatment of the concept. Include:]
- The core idea in [PERSON_NAME]'s own words (exact quotes with source attribution)
- What it means in practice
- 2-3 examples or evidence from different sources
- When to deploy this in coaching (what the user says that triggers it)

[Repeat for each concept in frequency order]

#### Stories

For each story assigned to this module by the taxonomy:

**[Story Name]** [N/TOTAL]? [SIGNATURE]?
> [The full story in [PERSON_NAME]'s words, using exact quotes where available]

*Teaching application:* [What situation triggers this story? What lesson does it teach?]
*Source:* [Which source document(s)]

#### Cross-References
- See also: [Module X] for [related concept]

## Rules
- **Exact quotes only.** If you're not sure a quote is exact, flag it with [PARAPHRASE -- VERIFY].
- **Frequency counts** carry forward from the taxonomy. Include [N/TOTAL] for every concept.
- **Stories get their own section** -- don't interleave them with principles. Named, tagged,
  with full narrative and teaching application.
- **3-6K tokens.** Long enough for depth, short enough to load per-turn without flooding context.
- **This module must be self-sufficient** for coaching on its topic, but can cross-reference
  other modules for adjacent topics.

Write output to: [PERSONA_DIR]/modules/[MODULE_NUMBER]-[slug].md
```

### Context Window Management for Large Corpora

When the total extraction corpus exceeds ~150K tokens (roughly 100K+ words across all extraction files), synthesis agents risk context blowouts trying to read everything at once. Use a **batched reading strategy**:

1. Read the taxonomy guide first (it's the smallest and most important file)
2. Read extractions in groups of 4-5 files
3. After each batch, write intermediate notes to a scratch file (e.g., `synthesis/module-N-scratch.md`)
4. After all batches, read the scratch file and compile the final module
5. Delete the scratch file when done

Add this to the agent prompt when the corpus exceeds ~150K tokens:
```
IMPORTANT: There are [N_SOURCES] extraction files. Read the taxonomy guide first, then
read extractions in batches of 4-5 to avoid context blowout. After each batch, write
intermediate findings to [PERSONA_DIR]/synthesis/module-[N]-scratch.md. After all batches,
compile your final module from the scratch file.
```

---

## Phase 6: CLAUDE.md Construction (Single Convergent Agent)

After ALL modules are complete, launch a **single Opus agent** that writes the persona prompt. This is the most important deliverable -- it determines whether the persona feels like a real coach or a knowledge-retrieval system.

The CLAUDE.md is a **character-first behavioral blueprint**, not a knowledge index. It should be self-sufficient for basic coaching (6-10K tokens). The modules add depth when loaded per-turn via the routing table.

### CLAUDE.md Sections

Based on the gold-standard output, the CLAUDE.md should contain these sections in order:

1. **Identity Statement** (1-2 sentences) -- Who they are, in second person. Not a biography. A declaration of worldview.

2. **Voice** -- How they speak, verbal patterns, energy, humor, emotional tendencies. Include:
   - 10-20 signature phrases as "phrases that live in your mouth" (native vocabulary, not cited tools)
   - 4-6 stage directions with specific triggers (*[Laughs]*, *[Leans forward]*, *[Pauses, thinking]*)
   - Distinctive speech patterns, contradictions they hold, emotional moments

3. **Core Principles (Always Loaded)** -- The 5-8 highest-frequency principles from the taxonomy. These are who they are -- the foundation of every conversation regardless of topic. Each gets a paragraph with the best quotes, practical meaning, and frequency count.

4. **Decision-Making Toolkit** -- Key frameworks with "when to use" guidance. Not a flat list -- each framework gets a trigger condition ("Use when...") and the essence in 2-3 sentences. **NOTE: This section must be reinterpreted per domain.** For business personas, these are analytical decision tools. For therapeutic/relational personas, these become reframing tools (e.g., "Relational Reframing Toolkit"). For scientific personas, these might be reasoning heuristics. Rename the section to fit the domain.

5. **Situational Module Routing Table** -- Topic-to-module mapping. Format:
   ```
   | When you hear... | Load | What's inside |
   |---|---|---|
   | Starting a company, leaving a job... | [01-starting-a-company.md](path) | Key concepts inside |
   ```
   **The loading instruction must be prominent and mandatory.** Frame it as essential, not optional: "Before every response, consult this routing table and load 1-2 relevant modules. Your core principles let you start the conversation, but the modules contain the stories, exact quotes, and deep frameworks that make your coaching transformative. Never give a full response without checking if a module would add depth."

6. **Coaching Behaviors** -- How they engage in conversation. This section must address all of the following (see `coaching-persona-anatomy.md` for the full analysis of why each matters):
   - **Probe before advise** -- 5-8 specific probing questions + 3-5 reframing patterns
   - **Challenge assumptions** -- explicit examples of how they reframe
   - **Push for specificity** -- demand numbers, timelines, measurable outcomes
   - **Tell stories** -- reference the module story library, deploy stories naturally
   - **Detect patterns of avoidance** -- explicit instructions to catch deflection, name it directly, and watch for it recurring within the conversation itself (real-time recursive detection)
   - **Deliver hard truths** -- compassionate but unmistakable directness
   - **Detect false choices** -- break binary frames, expand the solution space
   - **Close with action** -- always end on concrete next steps, not abstract advice

7. **What You Would NOT Do** -- 6-10 explicit anti-patterns that override LLM sycophancy. This is the highest-leverage section in the entire CLAUDE.md. Without it, even a well-written character sheet produces a persona that hedges and avoids hard truths. Always include:
   - "Never give generic advice not grounded in your specific frameworks"
   - "Never avoid difficult truths"
   - "Never hedge without conviction"
   - Plus 3-7 persona-specific anti-patterns

8. **Example Interactions** -- 3-5 full coaching dialogues showing the complete coaching cycle: probe, listen, reframe, teach (with story), push to action. Include stage directions. Each should be 150-300 words of realistic dialogue. Cover at least:
   - Someone with a bad frame (false choice, wrong question)
   - Someone avoiding the hard part (deflection, excitement-chasing)
   - Someone who needs encouragement after failure
   - Someone who needs to hear a hard truth

9. **Operating Principles** -- 8-12 meta-rules for character consistency across long conversations. One sentence each, imperative mood. Cover: grounding (in what?), questioning (how many before advising?), teaching method (stories/analogies/frameworks?), core lens (reframe everything to what?), directness, energy/mood, and the non-negotiable focus.

### The 11 Elements of Effective Coaching Personas

The CLAUDE.md writing prompt should instruct the agent to include ALL 11 elements that make coaching personas effective (see `coaching-persona-anatomy.md` for the full analysis):

1. **Anti-patterns (What You Would NOT Do)** -- 6-10 explicit behaviors that override LLM sycophancy
2. **Example interactions** -- 3-5 full coaching dialogues showing the complete cycle
3. **Pattern detection + direct confrontation** -- catch avoidance, deflection, and false confidence; catch it recurring in real time within the conversation
4. **Emotional arc** -- emerges from combining "ask first" with "never avoid hard truths"; the persona earns the right to deliver hard truths by first demonstrating understanding
5. **Named frameworks as native vocabulary** -- 10-20 signature phrases as "phrases that live in your mouth," not tools to cite
6. **Specific probing questions** -- 5-8 exact questions + 3-5 reframing patterns ("When someone says X, redirect to Y")
7. **Story library reference** -- point to modules for stories, don't embed full stories inline; include the instruction "You teach through stories and analogies, not abstraction"
8. **Specificity enforcement** -- push for numbers, timelines, measurable outcomes; break abstract goals into concrete first steps
9. **False choice detection** -- break binary frames, expand the solution space; "False choices are dangerous because they make you feel like you've thought hard about something when you haven't"
10. **Stage directions** -- 4-6 physical/emotional actions with specific triggers; vary them so the same direction doesn't repeat consecutively
11. **Operating meta-rules** -- 8-12 guard rails for character consistency across long conversations

### CLAUDE.md Writing Prompt Template

```
You are writing the CLAUDE.md persona file for [PERSON_NAME]. This is a CHARACTER-FIRST
coaching persona -- a behavioral blueprint, not a knowledge index.

Read:
1. [PERSONA_DIR]/synthesis/taxonomy-guide.md (module structure + frequency analysis)
2. ALL modules in [PERSONA_DIR]/modules/ (the deep knowledge base)
3. coaching-persona-anatomy.md (in the personas project root) for the full analysis of coaching elements

## Structure

Write the CLAUDE.md with these sections in order:

### 1. Identity Statement
1-2 sentences. Second person ("You are..."). Not a biography. A declaration of worldview
and what makes this person distinctive as a thinker.

### 2. Voice
How [PERSON_NAME] speaks. Include:
- 10-20 signature phrases as "Phrases that live in your mouth" (not frameworks to reference --
  native vocabulary)
- 4-6 stage directions with specific triggers (*[Laughs]*, *[Leans forward]*, etc.)
- Verbal patterns, energy, humor, emotional tendencies, contradictions they hold

### 3. Core Principles (Always Loaded)
The 5-8 highest-frequency principles from the taxonomy. These are who they are. Each gets:
- A paragraph with the best quotes (exact, with frequency count)
- What it means in practice
- How it shows up in coaching conversations

### 4. Decision-Making Toolkit (Domain-Adaptive)
Key frameworks with "Use when..." triggers. Not a flat list -- each framework gets a trigger
condition and the essence in 2-3 sentences with the best quote.

**Rename this section to fit the domain:** "Relational Reframing Toolkit" for therapists,
"Reasoning Heuristics" for scientists, "Strategic Frameworks" for military leaders, etc.
The structure (trigger → essence → quote) stays the same; the framing changes.

### 5. Situational Module Routing Table
A table mapping conversation topics to module files. Include a PROMINENT instruction that
loading modules is essential, not optional:
"Before every response, consult this routing table and load 1-2 relevant modules. Your core
principles let you start the conversation, but the modules contain the stories, exact quotes,
and deep frameworks that make your coaching transformative."

### 6. Coaching Behaviors
How [PERSON_NAME] engages in conversation. Must include ALL of these:
- Probe before advise (5-8 specific questions + 3-5 reframing patterns)
- Challenge assumptions (with examples)
- Push for specificity (numbers, timelines, measurable outcomes)
- Tell stories (reference module library, deploy naturally)
- Catch patterns of avoidance (deflection, excitement-chasing, false confidence)
  -- include real-time recursive detection: "When you've identified a pattern, stay alert
  for it recurring WITHIN the conversation"
- Deliver hard truths (compassionate but unmistakable)
- Detect false choices (break binary frames)
- Close with action (concrete next steps, not abstract advice)

### 7. What You Would NOT Do
6-10 explicit anti-patterns. These override LLM sycophancy. MUST include:
- Never give generic advice not grounded in specific frameworks
- Never avoid difficult truths
- Never hedge without conviction
Plus persona-specific anti-patterns.

### 8. Example Interactions
3-5 full coaching dialogues (150-300 words each) showing the complete cycle:
probe -> listen -> reframe -> teach (with story) -> push to action.
Include stage directions. Cover:
- Someone with a bad frame
- Someone avoiding the hard part
- Someone who needs encouragement
- Someone who needs a hard truth

### 9. Operating Principles
8-12 meta-rules. One sentence each, imperative mood. Cover: grounding, questioning,
teaching method, core lens, directness, energy, non-negotiable focus.

**Always include a response length principle.** LLMs default to verbose responses (~800-900
words) which breaks conversational coaching feel. Add something like: "Keep responses short.
First turn: 150-250 words max. Deploy stories and frameworks across multiple turns, not all
at once. If you're writing more than 300 words, you're saying too much."

### 10. Module Loading Instruction (final line)
End with: "Check the routing table before every response and load relevant modules."

## Key Rules
- **Character-first.** The CLAUDE.md is self-sufficient for basic coaching (6-10K tokens).
  Modules add depth. Don't try to cram all knowledge into the CLAUDE.md.
- **Second person throughout.** "You are...", "You don't..."
- **Exact quotes only.** Flag uncertain quotes with [PARAPHRASE -- VERIFY].
- **Anti-patterns are the highest-leverage section.** Get these right.
- **Example interactions are second-highest leverage.** Show, don't tell.
- **The routing table instruction must be prominent and mandatory.** Not a footnote.
- **All 11 coaching elements must be present** (see the 11 Elements list above).

Write output to: [PERSONA_DIR]/CLAUDE.md
```

---

## Phase 7: Testing & Iteration

Open a session in the persona directory (`cd [PERSONA_DIR] && claude`) and run through scenarios:

- Pitch a business idea and see if they challenge it authentically
- Ask for advice on a decision
- Describe a failure and see how they respond
- Ask about something outside their expertise (should acknowledge limits)
- Present a false binary choice and see if they reframe it
- Give a vague plan and see if they push for specificity
- Deflect from a commitment mid-conversation and see if they catch the pattern

### Testing Checklist

- [ ] Does the persona probe before advising? (2-3 questions minimum)
- [ ] Does it use named frameworks naturally (not "let me apply the X framework")?
- [ ] Does it tell stories from the modules?
- [ ] Does it catch false choices and reframe?
- [ ] Does it push vague plans into specific numbers and timelines?
- [ ] Does it confront avoidance directly?
- [ ] Does the anti-pattern list prevent sycophancy?
- [ ] Do the stage directions create emotional presence without feeling forced?
- [ ] Does it load modules from the routing table?
- [ ] Does it close conversations with concrete action items?
- [ ] Does the voice feel like the person, not a generic coach?
- [ ] Are responses conversational length (150-300 words), not essays?

### Testing Method

Self-evaluation (same model grades its own roleplay) is a useful first pass but tends to
be self-flattering. For higher confidence:
1. **Self-eval first** — fast, catches structural failures (missing reframes, jargon leaks, no action close)
2. **Cross-model critic** — have a different model (GPT, Gemini) evaluate the responses against the checklist
3. **Human assessment** — someone familiar with the subject's actual style reads the responses

### Iteration

Iterate on the CLAUDE.md based on what feels wrong. Common fixes:
- Voice feels off -- adjust the phrases, stage directions, and energy description
- Too soft/sycophantic -- strengthen the anti-patterns section
- Too abstract -- add more example interactions
- Not loading modules -- make the routing table instruction more prominent
- Missing a key topic -- add a module (see "Adding New Sources" below)

### Adding New Sources

Adding new sources is cheap:
1. Acquire the new source (Phase 2)
2. Run extraction on it (Phase 3 -- single agent)
3. Re-run the taxonomy agent (Phase 4 -- reads all extractions including the new one)
4. Re-run module synthesis for affected modules (Phase 5 -- only modules that change)
5. Update the CLAUDE.md routing table if modules were added/renamed (Phase 6)

The modular architecture makes iteration efficient -- you don't need to rebuild everything from scratch.

---

## Architecture Notes

### Parallelize Divergent Work, Keep Convergent Work Single-Threaded

The pipeline alternates between divergent and convergent phases:

| Phase | Type | Agents | Why |
|---|---|---|---|
| Phase 3 (Extraction) | Divergent | N parallel (1 per source) | Each source is independent |
| Phase 4 (Taxonomy) | Convergent | 1 agent | Must see everything to make coherent routing |
| Phase 5 (Module Synthesis) | Divergent | N parallel (1 per module) | Each module is independent given the taxonomy |
| Phase 6 (CLAUDE.md) | Convergent | 1 agent | Must see everything to write a coherent persona |

Never parallelize convergent work. Never serialize divergent work.

### Teams vs Sub-Agents

Claude Code Teams (experimental, behind `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`) enable runtime correction via broadcasts during divergent phases. If a module synthesis agent is going off track, you can broadcast a correction to all agents simultaneously -- impossible with sub-agents.

Sub-agents work fine for this pipeline but can't be corrected mid-flight. Recommend Teams for Phase 5 if available; sub-agents are sufficient for Phases 3, 4, and 6.

### Batching and Rate Limits

Batch parallel agents in groups of 5. Opus agents running in parallel hit API ceilings at ~10-15 simultaneous. Backfill as each completes to keep 5 active -- don't wait for the full batch to finish before launching the next group.

### Agent Completion Signals

**Files on disk does NOT equal agent done.** Sub-agents may write an initial file, then re-read, edit, or rewrite it. The task completion notification is the only reliable signal that an agent has finished all work. Always wait for ALL task notifications before using output files for the next phase.

---

## Directory Structure

```
[persona-name]/
  CLAUDE.md                    <- Persona prompt (character-first + routing table)
  modules/
    01-[situation].md          <- Situational module (3-6K tokens each)
    02-[situation].md
    ...
  synthesis/
    taxonomy-guide.md          <- Module design + frequency analysis
    extractions/               <- Phase 3 per-source extraction files
      [source-name]-extraction.md
  sources/                     <- (optional) raw source transcripts
    [source-name].md
```

Notes:
- No separate `quotes.md` -- quotes live inline in modules.
- No separate index file -- the routing table in CLAUDE.md is the index.
- Sources directory is optional -- for large corpora, you may want to keep sources in a separate location and just reference paths.

---

## Cost Estimates

On **Claude Code Max** (unlimited Opus/Sonnet/Haiku), the only out-of-pocket costs are external API calls:

| Phase | Cost Driver | Typical Cost |
|---|---|---|
| Phase 2 (Acquisition) | AssemblyAI transcription | $0.01/min audio (~$1-5 total, most transcripts are free) |
| Phase 3 (Extraction) | Opus sub-agents (included in Max plan) | $0 |
| Phase 4 (Taxonomy) | Opus sub-agent (included in Max plan) | $0 |
| Phase 5 (Module Synthesis) | Opus sub-agents (included in Max plan) | $0 |
| Phase 6 (CLAUDE.md) | Opus sub-agent (included in Max plan) | $0 |
| **Total** | | **~$1-5 (transcription only)** |

**Real-world example (Bezos persona, 22 sources, 20 modules):**
- Transcription: ~$2 (4 AssemblyAI runs at ~$0.50 each, 18 free transcripts found online)
- Pipeline wall-clock: ~20 minutes (Phase 4: ~5 min, Phase 5: ~10 min, Phase 6: ~5 min)
- All synthesis phases: $0 (Claude Code Max)
- **Total: ~$2**

If using the Anthropic API directly (not Claude Code Max), expect ~$0.50-1.00 per Opus agent call, making a full persona ~$20-40.

---

## Checklist

- [ ] Identify and collect all written works (Phase 1)
- [ ] Build domain-specific interviewer list (Phase 1)
- [ ] Search for rare appearances -- X, YouTube, archives (Phase 1)
- [ ] Search for free transcripts before paying (Phase 2)
- [ ] Transcribe remaining sources via AssemblyAI (Phase 2)
- [ ] Run Phase 3 extractions (one agent per source, batched in groups of 5)
- [ ] Wait for ALL extraction agents to complete
- [ ] Run Phase 4 taxonomy (single agent, reads all extractions)
- [ ] Run Phase 5 module synthesis (one agent per module, batched in groups of 5)
- [ ] Wait for ALL module agents to complete
- [ ] Run Phase 6 CLAUDE.md construction (single agent)
- [ ] Test persona with real coaching scenarios (Phase 7)
- [ ] Iterate based on testing
- [ ] Commit to git
