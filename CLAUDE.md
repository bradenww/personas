# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A pipeline for building AI coaching personas from public figures' own words. Each persona is a self-contained directory with a `CLAUDE.md` system prompt + `modules/` knowledge base that turns Claude into a direct, challenging, action-oriented coach.

## Using a Persona

```bash
cd persona-name/   # e.g., jeff-bezos, esther-perel
claude
```

Claude Code reads the persona's `CLAUDE.md` automatically. Available personas: jeff-bezos, jeff-bezos-conversational, esther-perel, chris-camillo, matt-mochary, paul-graham, stanley-druckenmiller, jerry-colonna.

## Building a New Persona

Tell Claude: "Build a persona for [Person Name] following BUILD-PERSONA.md"

The pipeline has 8 phases. Phases 1-2 are human-assisted (source collection). Phases 3-7 are automated agent work (~20-30 min on Claude Code Max). Phase 8 is cleanup.

**Key pipeline documents:**
- `BUILD-PERSONA.md` — Full pipeline guide with reusable prompt templates for every phase
- `coaching-persona-anatomy.md` — The 11 elements that make coaching personas effective, ranked by leverage

## Repo Structure

Each persona directory contains only distributable files:
- `CLAUDE.md` — Character-first system prompt (6-10K tokens) with voice, core principles, decision frameworks, coaching behaviors, example interactions, and a situational module routing table
- `modules/` — 15-25 situational modules (3-6K tokens each) with stories, exact quotes, specialized frameworks
- Optional reference files (e.g., `external-links-appendix.md` in matt-mochary)

## What Gets Committed vs. Gitignored

**Committed:** persona `CLAUDE.md`, `modules/`, persona-specific reference files.

**Gitignored (`working-files/`):** All build artifacts — source material (copyrighted), extractions, taxonomy guides, build-progress files, synthesis scratch files. Raw sources live in `working-files/[persona-name]/sources/`.

## Architecture: How Personas Work

1. The persona `CLAUDE.md` contains a **routing table** mapping conversational topics to specific modules
2. During coaching, the persona loads 1-2 modules per turn using the Read tool based on what the user is discussing
3. Core principles in the `CLAUDE.md` carry the conversation; modules add depth (stories, exact quotes, deep frameworks)
4. The 11-element coaching framework (in `coaching-persona-anatomy.md`) drives persona quality — anti-patterns and example interactions are the highest-leverage elements

## Build Pipeline Architecture

Phases 3-7 use parallel Opus agents. Key patterns:
- **Phase 3 (Extraction):** Parallel agents produce structured 11-section extractions per source
- **Phase 4 (Taxonomy):** Single agent designs modules with frequency counting across all extractions
- **Phase 5 (Module Synthesis):** Parallel agents write modules, cross-referencing taxonomy and extractions
- **Phase 6 (CLAUDE.md Construction):** Single agent assembles the persona from all synthesis output
- **Phase 8 (Cleanup):** Moves build artifacts to `working-files/`, keeps persona directories clean
