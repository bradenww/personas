# AI Coaching Personas

A pipeline for building AI coaching personas from a public figure's own words. Each persona is a character-first `CLAUDE.md` system prompt + modular knowledge base that turns Claude into a direct, challenging, action-oriented coach — not a chatbot that quotes famous people.

## Using an Existing Persona

Each persona is a self-contained directory with a `CLAUDE.md` and a set of modules. To start a coaching session:

```bash
cd jeff-bezos    # or esther-perel, etc.
claude
```

That's it. Claude Code reads the `CLAUDE.md` automatically and becomes the persona. The system prompt contains everything needed for basic coaching. During conversation, the persona loads 1-2 situational modules from its `modules/` directory for deeper material (stories, exact quotes, specialized frameworks).

### Available Personas

| Persona | Domain | Sources | Modules |
|---------|--------|---------|---------|
| [Jeff Bezos](jeff-bezos/) | Business, strategy, leadership | ~42 (24 shareholder letters 1997-2020, 18 interviews/speeches) | 20 |
| [Jeff Bezos (Conversational)](jeff-bezos-conversational/) | Same as above, shorter responses | Same as Jeff Bezos | 20 |
| [Esther Perel](esther-perel/) | Relationships, desire, relational therapy | 153 (138 essays, 15 interview transcripts) | 20 |

### What to Expect

These personas don't just answer questions. They coach:
- Probe with 2-3 questions before offering perspective
- Reframe your question when the premise is wrong
- Catch patterns of avoidance, deflection, and false confidence
- Push vague plans into specific commitments
- Deliver hard truths after earning the right through understanding
- Close with one concrete thing you can do

## Building a New Persona

New personas are built using Claude Code and the pipeline guide. The process has 7 phases — source collection is human-paced (~30-60 min), agent work is ~20-30 minutes on Claude Code Max.

### Quick Start

```bash
cd /path/to/this/repo
claude
```

Then tell Claude:

> Build a persona for [Person Name] following BUILD-PERSONA.md

Claude will read the pipeline guide and walk through each phase. You'll help with source collection (Phase 1-2) — finding essays, interviews, and transcripts. Phases 3-7 are automated: Claude spawns parallel agents for extraction, taxonomy, synthesis, CLAUDE.md construction, and testing.

### What the Pipeline Produces

```
persona-name/
  CLAUDE.md                    # Character-first system prompt (6-10K tokens)
  sources.md                   # Source catalog — titles, dates, URLs (no copyrighted content)
  modules/
    module-01-topic-name.md    # Situational modules (3-6K tokens each)
    module-02-topic-name.md
    ...
  synthesis/
    taxonomy-guide.md          # Module design + frequency analysis
    extractions/               # Per-source structured extractions
  tests/                       # Phase 7 test scenario results

sources/                       # ← gitignored, NOT committed
  persona-name/
    essays/                    # Raw source material (copyrighted)
    interviews/
```

Raw source files (transcripts, essays) are stored in the top-level `sources/` directory, separated by persona. This directory is gitignored — source material is copyrighted and should not be committed to public repositories. Each persona's `sources.md` provides a complete catalog with URLs so sources can be re-collected.

### Pipeline Phases

1. **Source Identification** — Find written works, interviews, speeches, rare appearances
2. **Source Acquisition** — Collect transcripts (free transcripts first, AssemblyAI for the rest)
3. **Extraction** — Parallel Opus agents produce structured extractions per source (11 sections each)
4. **Taxonomy** — Single agent designs 15-25 situational modules with frequency counting
5. **Module Synthesis** — Parallel agents write full modules with stories, frameworks, quotes
6. **CLAUDE.md Construction** — Single agent writes the persona using the 11 coaching elements
7. **Testing & Iteration** — Automated test scenarios, iterate on voice and behavior

### Key Documents

| Document | Purpose |
|----------|---------|
| [BUILD-PERSONA.md](BUILD-PERSONA.md) | Full pipeline guide with reusable prompt templates for every phase |
| [coaching-persona-anatomy.md](coaching-persona-anatomy.md) | The 11 elements that make coaching personas effective, ranked by leverage |
| [pipeline-learnings.md](pipeline-learnings.md) | Improvement tracking across persona builds |

## What Makes This Different

Most AI persona implementations are knowledge-retrieval systems: "What would X say about Y?" This pipeline produces personas that **coach**. The difference comes from the [11-element coaching framework](coaching-persona-anatomy.md), reverse-engineered from real coaching conversations:

1. **Anti-patterns** — Override LLM sycophancy ("never avoid hard truths" is the single highest-leverage instruction)
2. **Example interactions** — The model mimics demonstrated behavior more reliably than described behavior
3. **Pattern detection** — Catch avoidance, deflection, false confidence, including recursive real-time detection
4. **Emotional arc** — Earn directness through demonstrated understanding
5. **Named frameworks as native vocabulary** — Signature phrases as how they *think*, not tools they *apply*
6. **Specific probing questions** — Exact questions for exact situations
7. **Story library** — Named, frequency-counted, with teaching applications
8. **Specificity enforcement** — Push vague into concrete
9. **False choice detection** — Break binary frames
10. **Stage directions** — Physical/emotional presence with triggers
11. **Operating meta-rules** — Character consistency across long conversations

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (for using existing personas and building new ones)
- Claude Code Max recommended for building (unlimited Opus agents for parallel pipeline work)
- [AssemblyAI](https://www.assemblyai.com/) API key for transcription (~$0.01/min) — only needed when free transcripts aren't available

## License

MIT
