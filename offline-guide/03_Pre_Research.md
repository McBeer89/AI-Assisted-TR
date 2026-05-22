# Section 3: Pre-Research — Gather Your Sources

This is the one phase you MUST complete while you still have internet access.
Everything after this section can be done fully offline — but only if you
collect the right materials now.

The goal is to assemble a self-contained research packet for your chosen
technique. Think of it as packing a bag before a trip: once you're offline,
you can't go back for what you forgot.

---

## Step 1: Choose Your Technique and Platform

Before gathering anything, decide:

- **Which ATT&CK technique?** (e.g., T1505.003, T1055.004, T1070.001)
- **Which platform?** (e.g., Windows, Linux, Azure) — TRRs are scoped to a
  single platform unless the technique is inherently cross-platform.

Write these down. They anchor everything that follows.

---

## Step 2: Collect Primary Sources

Work through this checklist in order. Save each item locally (PDF, HTML save,
or copy-paste to a text file).

### MITRE ATT&CK Page

- [ ] The technique page itself:
      `https://attack.mitre.org/techniques/TXXXX/` (or `/TXXXX/XXX/` for
      sub-techniques)
- [ ] Note: tactic(s), platforms, data sources, permissions required
- [ ] Read the procedure examples table — these show real-world usage but
      remember, specific tools are tangential. You're looking for patterns in
      *what operations are performed*, not which tools perform them.
- [ ] Save or note the references listed at the bottom of the page — these
      often point to the best primary sources.

### Atomic Red Team Tests

- [ ] Check for emulation tests:
      `https://github.com/redcanaryco/atomic-red-team/tree/master/atomics/TXXXX`
- [ ] Save the YAML for any tests that match your platform scope.
- [ ] These are useful for lab validation later, not for defining procedures.
      Atomic tests are instances, not procedures.

### Microsoft Documentation (for Windows techniques)

- [ ] Relevant API documentation (e.g., NtWriteFile, CreateRemoteThread,
      WMI class methods)
- [ ] Service architecture documentation (e.g., how EventLog service works,
      how IIS processes requests, how the WMI infrastructure is structured)
- [ ] Protocol specifications if relevant (e.g., MS-EVEN6 for event log
      RPC, MS-DRSR for DCSync)
- [ ] Security audit documentation (which audit policies generate which
      events, what SACL configurations are needed)

### Sysmon Documentation

- [ ] Sysmon event ID reference — you'll need this for telemetry mapping.
      The Sysinternals documentation page or a community cheat sheet works.
- [ ] Note which Sysmon events are relevant to the operations you expect to
      encounter.

### Security Vendor Research

Search for blog posts and technical analyses from:

- [ ] Elastic Security Labs
- [ ] Red Canary
- [ ] CrowdStrike (Falcon Overwatch, threat reports)
- [ ] Mandiant / Google Threat Intelligence
- [ ] SpecterOps
- [ ] Microsoft Threat Intelligence

**What you're looking for:** Deep technical breakdowns of how the technique
works at the OS level, not just "we saw APT-X use this tool." The best sources
explain the underlying mechanisms, not just the tool behavior.

### GitHub and Proof-of-Concept Repositories

- [ ] Search GitHub for proof-of-concept implementations.
- [ ] These help you understand the *mechanics* of the technique — what APIs
      get called, what operations are essential.
- [ ] Remember: the specific code is tangential, but the operation chain it
      reveals may be essential.

### Conference Talks and Papers

- [ ] DEF CON, Black Hat, BlueHat, SO-CON, SchmooCon presentations often
      contain deep technical analysis.
- [ ] Save slides and/or recordings if available.

---

## Step 3: Use the TRR Source Scraper (Optional, Recommended)

If you have the TRR Source Scraper tool available, run it before going offline.
It automates Steps 2 above by querying MITRE, Atomic Red Team, and DuckDuckGo
across multiple source categories.

```bash
cd tools/trr-source-scraper
pip install -r requirements.txt

# Full run with enrichment
python trr_scraper.py T1505.003 --name "Web Shell" --platform windows

# Quick scan without metadata enrichment
python trr_scraper.py T1505.003 --no-enrich

# Offline mode (MITRE + Atomic only, no web search)
python trr_scraper.py T1003.006 --no-ddg
```

The scraper produces a structured markdown research brief with source links,
relevance scores, and coverage gap analysis. Use its output as your collection
checklist — it tells you what categories have good coverage and where you need
to manually find more sources.

**Important:** The scraper finds sources. It does not analyze them. You still
need to read everything it collects and apply the methodology yourself.

---

## Step 4: Download Reference TRRs

Before going offline, download at least two completed TRRs from the TIRED Labs
library (https://library.tired-labs.org) to use as structural references:

- [ ] One TRR with a **shared pipeline** (procedures branch from a common
      operation chain) — TRR0016 (MSHTA) is a good example.
- [ ] One TRR with **independent pipelines** (procedures have completely
      separate operation chains) — TRR0023 (Clearing Windows Event Logs)
      demonstrates this pattern.
- [ ] Save both the TRR markdown AND the DDM JSON/PNG files.

Having real examples in front of you while writing is more valuable than any
amount of abstract guidance.

---

## Step 5: Save the Methodology References

If you don't already have local copies, save:

- [ ] This offline guide (all sections)
- [ ] `TRR_Research_Methodology_Guide.md` — the step-by-step analytical
      process
- [ ] `TECHNIQUE-RESEARCH-REPORT-OUTLINE.md` — the TRR section structure and
      writing guidance
- [ ] VanVleet's methodology articles if possible (Threat Detection
      Engineering Series on Medium)

---

## Step 6: Organize Your Research Packet

Create this folder structure before you start Phase 1:

```
TRR####/
  platform/
    README.md                    ← will become the TRR
    ddms/                        ← DDM JSON and PNG files
    images/                      ← supplementary screenshots/diagrams
    Supporting Docs/             ← research notes, scoping docs
      phase1_research.md
      source_material/           ← saved copies of all collected sources
    Procedure Lab/               ← lab recreation notes (if lab available)
```

Replace `####` with your TRR number (or `XXXX` as a placeholder if you don't
have one assigned yet).

---

## Checkpoint: Ready to Go Offline?

Before disconnecting, verify:

- [ ] You have the MITRE ATT&CK page for your technique saved locally.
- [ ] You have at least 3-5 substantive technical sources saved (not just
      tool documentation — you need sources that explain the underlying
      system mechanics).
- [ ] You have Atomic Red Team tests saved (if any exist for your technique).
- [ ] You have relevant API/protocol documentation saved.
- [ ] You have at least one completed TRR as a structural reference.
- [ ] You have this guide and the methodology references saved.
- [ ] Your folder structure is set up.

If any of these are missing, collect them now. Once you're offline, you work
with what you have.
