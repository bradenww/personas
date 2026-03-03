# Stanley Druckenmiller Persona -- Source Catalog

**Date compiled:** 2026-03-02 (updated 2026-03-03)
**Total sources acquired:** 25 files (23 original + 2 new + 1 upgraded)
**Estimated total words:** ~95,000

---

## Primary Sources (Full Transcripts)

| # | Source | Date | Type | Words | Acquisition Method |
|---|--------|------|------|-------|--------------------|
| 1 | Lost Tree Club Speech (Ken Langone Q&A) | 2015-01-18 | Speech + Q&A | ~9,200 | Free transcript (GitHub Gist) |
| 2 | Norges Bank "In Good Company" (Nicolai Tangen) | 2024-11-05 | Podcast | ~8,700 | YouTube auto-captions (cleaned) |
| 3 | Economic Club of New York (Scott Bessent) | 2019-06-03 | Speech + Interview | ~8,600 | Official PDF transcript |
| 4 | CNBC Squawk Box | 2023-11-01 | TV Interview | ~5,000 | CNBC official transcript |
| 5 | CNBC Squawk Box | 2024-05-07 | TV Interview | ~4,500 | CNBC official transcript |
| 6 | CNBC Delivering Alpha | 2022-09-28 | Conference | ~4,200 | CNBC official transcript |
| 7 | New Market Wizards (Jack Schwager) | 1992 | Book Chapter | ~3,600 | Compiled key excerpts |
| 8 | Goldman Sachs "Talks at GS" | 2021-02 | Video Interview | ~3,200 | Livewire Markets mirror |
| 9 | Pittsburgh Quarterly Profile | ~2015 | Written Profile | ~3,000 | WebFetch |
| 10 | Sohn Conference "The Endgame" | 2016-05-04 | Conference | ~3,000 | Scribd (Playwright) |
| 11 | CNBC Delivering Alpha | 2014-07-16 | Conference | ~3,000 | CNBC official transcript |
| 12 | Sohn Conference Notes | 2022-05-04 | Conference | ~3,000 | Multiple note-taker compilations |
| 13 | Acquirers Multiple Archive | Various | Quote Collection | ~2,700 | WebFetch |
| 14 | Real Vision / Kiril Sokoloff Notes | 2018-09-06 | Interview Notes | ~2,300 | Macro-Ops summary (paywalled original) |
| 15 | CMG Lost Tree Analysis | 2015 | Third-party Analysis | ~2,100 | WebFetch |
| 16 | Inside Philanthropy Q&A | 2023 | Written Q&A | ~1,900 | WebFetch |
| 17 | Palantir / Karp Conversation (JPMorgan CEO Forum) | 2024 | Conference | ~1,900 | WebFetch (notes) |
| 18 | USC Marshall Center Keynote | 2023-05-01 | Speech | ~1,800 | Official PDF (pdftotext) |
| 19 | Bridgespan Philanthropy Profile | ~2020 | Written Profile | ~1,500 | WebFetch |
| 20 | CNBC Squawk Box (COVID) | 2020-06-08 | TV Interview | ~750 | Partial transcript |
| 21 | Sohn Conference Notes | 2023-05 | Conference | ~670 | Note-taker compilations |
| 22 | Bowdoin College Talk | 2024 | Campus Talk | ~570 | Bowdoin Orient article |
| 23 | DealBook Conference (IBM/Amazon segment) | 2015 | Conference | ~500 | Partial transcript |

### Added 2026-03-03 (v2 Upgrade)

| # | Source | Date | Type | Words | Acquisition Method |
|---|--------|------|------|-------|--------------------|
| 24 | Morgan Stanley "Hard Lessons" (Iliana Buzali) | 2026-01-30 | Podcast | ~5,300 | AssemblyAI via Art19 RSS |
| 25 | How Leaders Lead (David Novak) | 2022-09-22 | Podcast | ~12,800 | AssemblyAI via Apple Podcasts/yt-dlp |
| — | Norges Bank v2 (replaces #3) | 2024-11-05 | Podcast | ~8,800 | AssemblyAI (speaker-diarized, replacing auto-captions) |

## Not Acquired (Identified but Paywalled/Unavailable)

- **Real Vision / Kiril Sokoloff Full Interview** (2018-09-06) — Behind Real Vision paywall. Only notes acquired.
- **CNBC Squawk Box Sept 2020, May 2021** — Partial transcripts only; video available for transcription.
- **CNBC Inauguration Day Interview** (2025-01-20) — Not yet transcribed.
- **Bloomberg, Fox Business, and other one-off appearances** — Lower signal, not prioritized.

## Extraction Summary

19 structured extractions were produced from 25 source files (smaller sources combined):
- Each extraction follows an 11-section template
- Combined extractions: philanthropy sources, secondary sources, Sohn 2016 + DealBook 2015
- v2 upgrade: Norges Bank re-extracted from speaker-diarized transcript (corrected quotes, +8 new quotes, +2 principles, +2 mental models)
- Total extraction output: ~85,000 words
- Extractions live in `synthesis/extractions/`
- Integration log: `synthesis/integration-log-2026-03-03.md`

## Notes

- Druckenmiller has **never written a book or published investor letters**. His entire public corpus is spoken word (interviews, speeches, conferences).
- The Norges Bank transcript is auto-captioned (no punctuation, no speaker labels) — marked for potential AssemblyAI upgrade.
- The New Market Wizards source is compiled key quotes/excerpts, not the full chapter (copyright).
- Several "notes" sources (Sohn, Real Vision, Palantir/Karp) are third-party summaries, not verbatim transcripts.
