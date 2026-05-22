# Appendix B: Common Pitfalls Checklist

A pre-submission checklist of the most common errors in TRR production. Run
through this after you think you're done — before you declare the TRR
complete.

---

## DDM Pitfalls

- [ ] **Tool-focused operations.** Search every node caption for tool names.
      "Mimikatz dumps LSASS" → "Read Process Memory." The tool is tangential;
      the essential operation is reading process memory.

- [ ] **Tangential elements in the DDM.** Search for specific command-line
      flags, filenames, file extensions, delivery methods, encoding choices.
      If the attacker controls it, it doesn't define the procedure.

- [ ] **Instances confused for procedures.** Do you have separate procedures
      for different tools that perform the same essential operations? If yes,
      collapse them into one procedure. Different tools, same operations =
      same procedure.

- [ ] **Missing alternate paths.** Did you ask "is there another way?" for
      every operation? Uncommon paths are still valid procedures if they
      change the essential operations.

- [ ] **Prerequisites modeled as pipeline steps.** Is "Write File" sitting
      as Step 1 in a linear chain before "Send HTTP Request"? If the file
      write happens hours/days before the trigger, it's a prerequisite node
      feeding into the pipeline — not an inline step.

- [ ] **Telemetry grouped on one node.** Is all telemetry dumped on a single
      "detection" or "monitoring" node? Each telemetry source belongs on the
      specific operation it directly observes.

- [ ] **Non-descriptive telemetry labels.** "Sysmon 1" → "Sysmon 1
      (ProcessCreate)." "Event 4688" → "Win 4688 (ProcessCreate)." Always
      include the event name.

- [ ] **Unresolved question marks.** Any `[?]` remaining? Either resolve
      them or document exactly what's needed to resolve them. Do not ship
      silent unknowns.

---

## TRR Writing Pitfalls

- [ ] **Detection-oriented language.** Search the TRR for: "detect,"
      "defender," "should," "opportunity," "fidelity," "signal," "monitor,"
      "alert," "suspicious." Each hit needs scrutiny — is this a technical
      fact or detection guidance? The most persistent problem.

- [ ] **Tool names in prose.** Tool names belong in the References section
      only. "Mimikatz performs..." → "The DRSGetNCChanges operation..."

- [ ] **Verbose procedure narratives.** Are you re-walking the entire shared
      pipeline for each procedure? Say "shares the same pipeline as Procedure
      A through X, then diverges" — then describe only the unique operations.

- [ ] **Phase 1 artifact leakage.** Is your exclusion table still 10+ rows
      from the research phase? Condense to typically 3-5 rows. Drop
      metadata-obvious exclusions, consolidate tangential items.

- [ ] **Multi-sentence scope statement.** The scope statement must be exactly
      one sentence. If it takes more, the scope isn't tight enough.

- [ ] **Technique Overview too long.** 2-4 sentences only. This is a summary,
      not a Technical Background preview.

- [ ] **Telemetry enablement guidance in Technical Background.** No tables
      with "Default State," "Enablement," or "How to Deploy" columns. State
      telemetry facts inline in prose. Deployment is the detection team's
      domain.

- [ ] **Missing or broken DDM image references.** Do the `![...]()` paths
      in your TRR markdown match the actual filenames in `ddms/`?

---

## Structural Pitfalls

- [ ] **Procedures without distinguishing operations.** Can you articulate
      what essential operation(s) make each procedure unique? If you can't
      fill the "Distinguishing Operations" column in the procedure table,
      your procedures may not be genuinely distinct.

- [ ] **Missing Technical Background.** Would a reader with no prior
      knowledge of the underlying technology be lost when they reach the
      Procedures section? If yes, the Technical Background needs more depth.

- [ ] **Uncited technical claims.** Every specific technical assertion
      (API behavior, event ID meaning, protocol detail) should be traceable
      to a reference.

- [ ] **Missing emulation tests.** If Atomic Red Team tests exist for your
      technique, did you include them? Not required, but a missed opportunity
      if they exist and you didn't list them.

---

## Final Sanity Checks

- [ ] **The red team test.** Could a red teamer execute this technique
      using only information in your TRR? If they'd need to look elsewhere
      for how the technique actually works, your Technical Background or
      procedure narratives have gaps.

- [ ] **The detection team test.** Could a detection engineer identify what
      to monitor using only information in your TRR + DDM? If the telemetry
      annotations are incomplete or the operations are too vague, they can't.

- [ ] **The IR test.** Could an incident responder know what artifacts to
      look for? The DDM's operations and telemetry should map to forensic
      evidence.

- [ ] **The discipline-neutrality test.** Read the entire TRR imagining
      you're on a different team than the one you're most familiar with.
      Does the TRR still serve you? If it feels written "for detection
      engineers," it's not neutral enough.
