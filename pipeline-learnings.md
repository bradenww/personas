# Persona Pipeline — Learnings & Improvements

Tracking learnings from each persona build to improve the pipeline over time. Each learning should be actionable — specify what to change in which phase.

---

## From Jeff Bezos v2 (2026-02-28)

### Phase 1: Extraction
1. **Stories must be first-class entities.** Extractions currently embed stories within thematic sections. Future extraction prompts must include an explicit "Stories & Anecdotes" section where each story gets:
   - A short name (e.g., "The Grandmother's Cigarettes")
   - The full narrative with exact quotes
   - The teaching application (what lesson does this story illustrate?)
   - Source citation with timestamp
   - This enables frequency counting and tagging ([SIGNATURE], [EMPHATIC]) just like principles.

2. **Extractors should suggest situational contexts.** Currently extractors produce raw thematic material with no taxonomy suggestions. For non-famous people where we can't design taxonomy top-down, extractors should tag each principle/story with suggested conversational situations where it would be useful (bottom-up taxonomy).

### Phase 2: Taxonomy
3. **Frequency counting works.** The `[N/22]` notation across 22 sources successfully identified which principles matter most. The [SIGNATURE] and [EMPHATIC] flags add qualitative weight. Keep this approach.

4. **20 modules is a good number.** Small enough that each module fits in 3-6K tokens, large enough to cover the full breadth of a person's thinking. Adjust per person but 15-25 is the sweet spot.

### Phase 3: Synthesis (Module Writing)
5. **Stories should have their own section within each module.** Currently stories are interspersed with principles. Each module should have a clearly demarcated "Stories" section at the end with named, tagged stories. This makes them easy to extract and deploy during coaching.

6. **[PARAPHRASE — VERIFY] flags work.** Multiple synthesis agents flagged quotes they couldn't verify. This is good QA. A future improvement: a verification pass agent that checks every flagged quote against source material.

### Phase 4: CLAUDE.md (Persona File)
7. **Character-first CLAUDE.md outperforms knowledge-first.** The Standard Bezos (10K token character sheet, zero doc loading) outperformed Teams Bezos (6K token index + loaded 50-70K of docs). The character sheet carries the conversation; modules add depth.

8. **Anti-patterns are the highest-leverage instruction.** "What You Would NOT Do" prevents LLM sycophancy. Without it, even a well-written character sheet produces a persona that hedges and avoids hard truths. Always include 6-10 explicit anti-patterns.

9. **Example interactions are second-highest leverage.** The model mimics demonstrated behavior more reliably than described behavior. Include 3-5 full coaching dialogues.

10. **Module loading instruction must be prominent and mandatory.** The Standard Bezos had "use the Read tool to load relevant ones" but ignored it because the CLAUDE.md was sufficient. For a modular architecture, the loading instruction must be framed as essential, not optional. Position it as a meta-instruction, not a footnote.

### Pipeline Architecture
11. **Parallelize divergent work, keep convergent work single-threaded.** Taxonomy (convergent) = 1 agent. Module synthesis (divergent) = N parallel agents. CLAUDE.md writing (convergent) = 1 agent. Teams' killer feature is runtime correction via broadcasts during divergent phases.

12. **Batch parallel agents in groups of 5.** Prevents Opus rate limit hits. Backfill as each completes to keep 5 active.

---

## Implemented (BUILD-PERSONA.md v2, 2026-02-28)
- [x] Add story extraction to extraction prompt template
- [x] Add story sections to module format specification
- [x] Add story frequency counting to taxonomy phase
- [x] Write reusable extraction prompt template
- [x] Write reusable synthesis prompt template (taxonomy + module + CLAUDE.md — 4 templates total)
- [x] Integrate coaching-persona-anatomy.md (11 elements) into CLAUDE.md writing prompt

## From Esther Perel (2026-03-01)

### Pipeline Validation
13. **v2 pipeline works end-to-end for non-business domains.** First full run beyond Bezos. 7 phases, ~50 agents, clean execution across relational/therapeutic domain. Template is validated as generalizable.

### Phase 1: Source Collection & Extraction
14. **Essay clustering is the right approach for 50+ written sources.** 138 essays grouped into 10 thematic clusters kept extraction manageable (10 agents vs 138). Each cluster had enough material for rich output without drowning the extractor. Good default for any persona with large written corpus.

### Phase 2: Taxonomy & Phase 3: Synthesis
15. **Convergent phases are the bottleneck.** Extraction (25 agents) and module synthesis (20 agents) parallelized beautifully. Taxonomy (1 agent, 25 inputs) and CLAUDE.md construction (1 agent, 20 inputs) are single-threaded and took the longest. Unavoidable — they need to see everything.

### Phase 4: CLAUDE.md
16. **"Decision-Making Toolkit" must be domain-adaptive.** For Bezos it's analytical decision tools. For Perel it became "Relational Reframing Toolkit." BUILD-PERSONA.md should flag this section as requiring reinterpretation per persona domain, not mechanical fill-in.

17. **Response length guidance must be a default Operating Principle.** All 5 test agents produced responses ~800-900 words — too long for conversational coaching. Added Principle 13 to Perel's CLAUDE.md capping first turns at 150-250 words. This should be baked into the template for every persona.

### Phase 5: Testing
18. **Self-evaluation tests are useful but self-flattering.** All 5 scenarios passed 100%. Partly real quality, partly same model grading its own work. Stronger testing: separate critic agent evaluating responses, or human who knows the subject assessing fidelity.

---

## Implemented (BUILD-PERSONA.md updates, 2026-03-01)
- [x] Build first non-Bezos persona to validate the generalized pipeline (Esther Perel)
- [x] Add response length guidance to CLAUDE.md Operating Principles template
- [x] Add note that "Decision-Making Toolkit" section must be reinterpreted per domain

## Pending Improvements
- [ ] Create verification pass for [PARAPHRASE — VERIFY] flags
- [ ] Add cross-model critic evaluation to Phase 7 testing (not just self-eval)
- [ ] Consider adding essay clustering guidance to Phase 2 for large written corpora
