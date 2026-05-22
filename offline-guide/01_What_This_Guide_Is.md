# Section 1: What This Guide Is

## Purpose

This is a standalone reference for producing Technique Research Reports (TRRs)
and Detection Data Models (DDMs) following the TIRED Labs methodology — without
AI assistance, without internet access, and without the automated pipeline.

Everything in this guide is derived from the same methodology, prompts, and
workflows used in the AI-assisted and Claude Code automated pipelines. The
analytical process is identical. What changes is who does the mechanical work:
instead of an AI generating DDM JSON, explaining OS internals on demand, and
catching discipline-neutrality violations in real time, you do all of that
yourself.

The tradeoff is speed, not quality. A TRR produced manually using this guide
should be indistinguishable from one produced with AI assistance — it just
takes longer.

## Audience

Someone with foundational security knowledge who wants to produce a TRR. You
don't need prior TRR experience, but you do need to be comfortable reading
technical documentation about operating system internals, APIs, and protocols.
The methodology will teach you how to analyze; your existing technical literacy
is what lets you understand what you're analyzing.

## What You Need Before Starting

### Required

- A text editor (VS Code, Obsidian, Notepad++ — anything you'll actually use)
- Arrows.app access (free, web-based) for DDM diagramming — or pen and paper
  if truly offline
- A printer or local copies of your research sources (gathered before going
  offline — Section 3 covers what to collect)
- This guide

### Strongly Recommended

- A lab environment (VM or dedicated machine) for executing procedures and
  validating telemetry claims
- Access to the TIRED Labs TRR Library (https://library.tired-labs.org) for
  published examples — download a few before going offline
- At least one completed TRR to use as a structural reference while writing
  (TRR0016 and TRR0023 are good models with different complexity levels)

### Not Required but Useful

- The TIRED Labs methodology articles by VanVleet (the Series on Medium) — if
  you can print them, do. They provide the "why" behind every rule in this
  guide.
- A local LLM (via LM Studio, Ollama, etc.) — not "AI-assisted" in the way
  this guide means, but useful for rubber-ducking technical questions if you
  have one available. Not a substitute for the methodology.

## How This Guide Is Structured

The guide follows the same phase-gated progression used in every other TIRED
Labs workflow:

1. **Core Concepts** — internalize before you start
2. **Pre-Research** — gather sources while you still have internet
3. **Phase 1: Understand & Scope** — learn the technique, define boundaries
4. **Phase 2: Build the DDM** — map essential operations
5. **Phase 3: Identify Procedures** — trace distinct execution paths
6. **Phase 4: Write the TRR** — document everything
7. **Phase 5: DDM Exports** — produce the final diagram artifacts

Each phase has a hard stop gate. Do not advance to the next phase until you've
satisfied the checkpoint at the end of the current one. This isn't a
suggestion — the phase gates exist because skipping ahead is the single most
common source of errors in TRR production, whether you're working with AI or
without it.

## One Rule Above All Others

**Accuracy over speed.** If you're uncertain about something, stop and mark it
with a `[?]`. Do not fill gaps with assumptions. An incomplete TRR with honest
question marks is more valuable than a complete one with hidden guesses,
because the question marks tell the next person exactly where to focus. The
guesses just silently corrupt everything downstream.
