# Section 7: Phase 4 — Write the TRR

**Goal:** Produce the complete Technique Research Report — the lossless,
discipline-neutral reference document that any security team can use as
source material for their own work.

Write the TRR as `README.md` in your TRR's platform directory.

---

## TRR Structure

Follow this section order exactly. Each section has specific rules.

### 1. TRR Name

- Specific and descriptive.
- Does not have to mirror ATT&CK naming.
- Well-known names can be included in parentheses.

**Examples:**
- "Roasting Kerberos Service Tickets (Kerberoasting)"
- "Clearing Windows Event Logs"
- "WMI Event Subscription"
- "Signed Binary Proxy Execution with MSHTA"

### 2. Metadata

Include at the top of the document:

```markdown
| Field | Value |
|-------|-------|
| **TRR ID** | TRR#### |
| **ATT&CK Mapping** | TXXXX.XXX |
| **ATT&CK Tactics** | Tactic1, Tactic2 |
| **Platforms** | Windows |
| **Procedures** | TRR####.WIN.A, TRR####.WIN.B |
| **Contributors** | Your Name |
```

### 3. Scope Statement

**One sentence.** If you need more than one sentence, the scope isn't tight
enough. This is the sentence from Phase 1 — refined, not expanded.

Follow the scope statement with the **exclusion table**. Condense the
exhaustive Phase 1 exclusion table to its publication form:

- **Typically 3-5 rows** — but include more if the technique genuinely
  warrants it. The row count is a guideline, not a hard cap.
- **Drop rows obvious from metadata.** Other sub-techniques under the same
  parent ATT&CK ID don't need exclusion rows — the reader can see from the
  ATT&CK Mapping field that you're covering .003, not .001 or .002.
  Cross-platform variants don't need rows if the Platforms field already says
  "Windows."
- **Drop generic boilerplate.** "Specific tools are excluded because
  tangential" applies to every TRR ever written. Don't waste a row on it
  unless there's a specific tool-related scoping decision worth calling out.
- **Consolidate tangential items.** If you're excluding five different
  attacker-controlled elements for the same reason, combine them into one row:
  "Specific tools, file names, delivery methods — tangential."

**Example of a good condensed exclusion table:**

| Excluded Item | Rationale |
|---|---|
| Memory-only / fileless web shells | Different essential operations (no file write); separate TRR |
| Non-IIS web servers (Apache, Nginx) on Windows | Different request pipeline architecture |
| Specific tools, file names, delivery methods | Tangential — attacker-controlled implementation details |

### 4. Technique Overview

2-4 sentences. What the technique is, how it works at a high level, and why
attackers use it. Accessible to a non-technical reader.

**Rules:**
- Match VanVleet's published style: brief and direct.
- Do NOT re-explain scope or repeat exclusion rationale.
- Do NOT mention specific tools.
- Do NOT include detection guidance.

This section answers "what is this?" for someone who has never encountered
the technique.

### 5. Technical Background

The foundational knowledge section. A reader with no prior knowledge of the
underlying technology should be able to understand the procedures after
reading this section.

**What to include:**
- Relevant OS internals, services, and subsystems
- How the system processes the attacker's action (the pipeline)
- Relevant APIs and their roles
- Security contexts, permissions, and privilege requirements
- Relevant protocols and their structure
- Telemetry facts stated inline in prose

**What NOT to include:**
- Detection strategy or recommendations
- Tables with "Default State" or "How to Enable" columns
- Instructions for deploying telemetry
- Ranking of operations by detection value
- References to specific attacker tools

**Depth should match complexity.** A technique that abuses a simple API gets
a shorter Technical Background than one that involves multiple interacting
services, RPC protocols, and kernel components. Don't pad; don't skim.

**Telemetry in Technical Background:** State what exists as fact.

✅ "Windows Event 1102 fires when the Security log is cleared, recording
the account name and logon ID of the caller."

❌ "Defenders should monitor for Event 1102 to detect this technique."

The first is a technical fact. The second is detection guidance. Only the
first belongs in a TRR.

### 6. Procedures

Start with the **procedure summary table:**

```markdown
| ID | Name | Summary | Distinguishing Operations |
|----|------|---------|--------------------------|
| TRR####.WIN.A | Name | Summary | What's unique |
| TRR####.WIN.B | Name | Summary | What's unique |
```

Then write each procedure as its own subsection.

#### Per-Procedure Content

For each procedure, include:

**1. Narrative description.** Write in prose — NOT a numbered step list.
Describe the operation chain, explaining what happens and why at each step.

**Key rule: State what is UNIQUE about this procedure.**

- If Procedure B shares the same pipeline as Procedure A through a certain
  point, say so in one sentence and focus on where it diverges. Do NOT
  re-narrate shared operations.
- "This procedure shares the same pipeline as Procedure A through the
  Execute Code operation. It diverges when the executed code calls a .NET
  API directly rather than spawning a child process."
- Then describe only the divergent operations in detail.

**2. Per-procedure DDM diagram.** Reference the DDM image file:
`![Procedure A DDM](ddms/trr####_win_a.png)`

**3. Brief DDM description paragraph.** 2-4 sentences summarizing what the
diagram shows — the operation chain, branch points, and telemetry highlights.
This is for readers who want a quick summary without studying the diagram.

#### Discipline-Neutrality Traps in Procedure Writing

These phrases do NOT belong in TRR procedure narratives:

- ❌ "This operation is the primary detection opportunity"
- ❌ "This provides a high-fidelity detection signal"
- ❌ "Defenders should focus on this step"
- ❌ "This is the best place to detect"
- ❌ "This operation is difficult to detect" (state the telemetry facts
  instead — if no telemetry observes it, the reader can draw that conclusion)
- ❌ "This detection is inherently suspicious" (classification belongs in the
  Detection Methods derivative document)

**What to write instead:** State the technical facts. Name the operation,
explain what happens, note what telemetry observes it.

✅ "The EventLog service (svchost.exe hosting wevtsvc.dll) receives the RPC
call and processes the clear request. Windows Event 1102 records this action
in the Security log before the log is cleared."

The detection engineer will read this and decide it's a high-fidelity signal.
The TRR doesn't need to tell them that — the facts speak for themselves.

### 7. Available Emulation Tests

A table linking procedure IDs to known emulation tests. Not required, but
include if you found relevant Atomic Red Team tests or equivalent during
research.

```markdown
| ID | Link |
|----|------|
| TRR####.WIN.A | [Atomic Test Name](URL) |
| TRR####.WIN.B | [Atomic Test Name](URL), [Other Test](URL) |
```

### 8. References

All sources used in research. Every technical claim in the TRR should be
traceable to a reference.

```markdown
[Source Name]: URL
[Source Name]: URL
```

---

## Writing Quality Rules

### Be Concise

- Technique Overview: 2-4 sentences.
- Procedure narratives: as short as possible while covering all unique
  operations. If a procedure shares a pipeline with another, say so in one
  sentence and focus on the divergence.
- Technical Background: as long as the complexity warrants, but no padding.

### Be Precise

- Name specific APIs, event IDs, service names, registry keys.
- Avoid "the system does something" — say what the system does, which
  component does it, and through what mechanism.

### Be Neutral

- No detection recommendations.
- No tool references in prose (references section only).
- No "defenders should" or "attackers typically."
- State facts. Let teams draw conclusions.

### Common Writing Mistakes

**Phase 1 artifact leakage:** Copying the exhaustive 10+ row exclusion table
from your research notes into the final TRR. Condense it.

**Verbose shared pipeline narration:** Re-walking the entire shared pipeline
for each procedure instead of saying "shares the same pipeline as Procedure A
through X, then diverges."

**Detection language creep:** This is the most persistent problem. Every
time you write a sentence, ask: "Would this sentence be different if I were
writing for a red teamer instead of a detection engineer?" If yes, you've
drifted from discipline-neutral.

**Tool references in prose:** "Mimikatz performs DCSync by calling
DRSGetNCChanges." Rewrite as: "The DRSGetNCChanges operation retrieves
replication data from the domain controller."

---

## Checkpoint: TRR Complete?

Before advancing to DDM exports, verify:

- [ ] All eight sections are present and complete.
- [ ] Scope statement is exactly one sentence.
- [ ] Exclusion table is condensed (typically 3-5 rows).
- [ ] Technique Overview is 2-4 sentences with no tools or detection guidance.
- [ ] Technical Background is sufficient for a reader with no prior knowledge.
- [ ] Telemetry facts are stated inline as prose — no enablement tables.
- [ ] Procedure narratives describe unique operations only, not re-walked
      shared pipelines.
- [ ] No discipline-neutrality violations (search for "detect," "defender,"
      "should," "opportunity," "fidelity," "signal").
- [ ] No tool names in prose (references section only).
- [ ] All technical claims are sourced.
- [ ] DDM image references match the filenames you'll produce in Phase 5.
