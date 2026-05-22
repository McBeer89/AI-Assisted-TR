# Appendix C: Repository Structure & Templates

---

## Standard TRR Repository Structure

```
TRR####/
  platform/
    README.md                         ← The TRR document
    ddms/
      ddm_trr####_platform.json       ← Master DDM (all arrows black)
      ddm_trr####_platform.png        ← Master DDM image
      trr####_platform_a.json         ← Procedure A export (red active path)
      trr####_platform_a.png          ← Procedure A image
      trr####_platform_b.json         ← Procedure B export
      trr####_platform_b.png          ← Procedure B image
    images/                           ← Supplementary screenshots/diagrams
    Supporting Docs/                  ← Research artifacts (not in final TRR)
      phase1_research.md              ← Research notes from Phase 1
      source_material/                ← Saved copies of collected sources
    Procedure Lab/                    ← Lab recreation notes
```

### Platform Codes

```
win    Windows
lnx    Linux
mac    macOS
ad     Active Directory
azr    Azure
aws    AWS
gcp    Google Cloud Platform
k8s    Kubernetes
net    Network
```

---

## TRR Markdown Template

Copy this skeleton into `README.md` and fill in each section:

```markdown
# [TRR Name]

| Field | Value |
|-------|-------|
| **TRR ID** | TRR#### |
| **ATT&CK Mapping** | TXXXX.XXX |
| **ATT&CK Tactics** | Tactic1, Tactic2 |
| **Platforms** | Windows |
| **Procedures** | TRR####.WIN.A, TRR####.WIN.B |
| **Contributors** | Name |

## Scope

[One sentence scope statement.]

| Excluded Item | Rationale |
|---|---|
| Item 1 | Reason |
| Item 2 | Reason |
| Item 3 | Reason |

## Technique Overview

[2-4 sentences. What, how, why. No tools. No detection guidance.]

## Technical Background

[Foundational knowledge. OS internals, APIs, protocols, services, security
controls. Telemetry facts inline as prose. No enablement tables. Depth matches
technique complexity.]

## Procedures

| ID | Name | Summary | Distinguishing Operations |
|----|------|---------|--------------------------|
| TRR####.WIN.A | Name | Summary | What's unique |
| TRR####.WIN.B | Name | Summary | What's unique |

### Procedure A: [Name]

[Narrative description in prose. State what is unique about this procedure.
Do not use numbered step lists.]

#### Detection Data Model

![Procedure A DDM](ddms/trr####_win_a.png)

[2-4 sentence DDM description paragraph.]

### Procedure B: [Name]

[Narrative description. If this shares a pipeline with Procedure A, say so
in one sentence and focus on where it diverges.]

#### Detection Data Model

![Procedure B DDM](ddms/trr####_win_b.png)

[2-4 sentence DDM description paragraph.]

## Available Emulation Tests

| ID | Link |
|----|------|
| TRR####.WIN.A | [Test Name](URL) |
| TRR####.WIN.B | [Test Name](URL) |

## References

[Source 1]: URL
[Source 2]: URL
[Source 3]: URL
```

---

## Phase 1 Research Notes Template

Copy this into `Supporting Docs/phase1_research.md`:

```markdown
# Research: [Technique Name]

**Technique ID:** TXXXX.XXX
**Platform:** Windows
**Date Started:** YYYY-MM-DD

## Technique Summary

[2-3 sentences: what it is, what it accomplishes, why attackers use it]

## Technical Background Notes

[Underlying technology — OS internals, APIs, protocols, security controls,
prerequisites. Write everything you learn here. This feeds directly into the
TRR's Technical Background section.]

## Essential Operations Identified

- [EIO] [Operation name] — [description] | Telemetry: [source]
- [EIO] [Operation name] — [description] | Telemetry: [source]
- [TANGENTIAL] [Element] — attacker-controlled: [why]
- [OPTIONAL] [Element] — can be skipped: [why]
- [?] [Operation] — uncertain because: [reason]

## Scope Statement (Draft)

[One sentence.]

## Exclusion Table (Exhaustive — Condense for Final TRR)

| Excluded Item | Rationale |
|---|---|
| | |

## Essential Constraints Table

| # | Constraint | Essential? | Immutable? | Observable? | Telemetry |
|---|-----------|------------|------------|-------------|-----------|
| | | | | | |

## Distinct Execution Paths Found

[Paths discovered in research. What makes each unique at the essential
operation level.]

## Open Questions

- [?] [Question — what you'd need to resolve it]

## Sources Consulted

- [Source]: [What it contributed to your understanding]
```

---

## Procedure Documentation Template

Copy this into `Supporting Docs/procedures.md`:

```markdown
# Procedures: [Technique Name]

## Procedure Table

| ID | Name | Summary | Distinguishing Operations |
|----|------|---------|--------------------------|
| TRR####.WIN.A | | | |
| TRR####.WIN.B | | | |

## Procedure A: [Name]

**Path through DDM:**
[Trace the operation chain from start to finish]

**What makes it unique:**
[Articulate the distinguishing essential operation(s)]

**Telemetry coverage:**
[List telemetry sources that observe this procedure's operations]

## Procedure B: [Name]

**Shared pipeline with:** Procedure A through [operation name]

**Diverges at:** [operation name]

**What makes it unique:**
[Articulate the distinguishing essential operation(s)]

**Telemetry coverage:**
[List telemetry sources that observe this procedure's operations]

## Validation Notes

- [ ] All procedures are distinct (different essential operations)
- [ ] Known tools map to identified procedures
- [ ] No unresolved questions
```

---

## Git Commit Convention

Commit after completing each phase — do not batch multiple phases:

```
Phase 1: "TRR####: Phase 1 — scoping and technical background"
Phase 2: "TRR####: Phase 2 — DDM construction complete"
Phase 3: "TRR####: Phase 3 — procedures identified and validated"
Phase 4: "TRR####: Phase 4 — TRR document drafted"
Phase 5: "TRR####: Phase 5 — DDM exports finalized"
Review:  "TRR####: Post-review revisions"
```

Each commit should represent a discrete, reviewable unit of work.
