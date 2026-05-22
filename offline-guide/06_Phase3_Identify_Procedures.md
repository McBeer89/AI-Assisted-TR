# Section 6: Phase 3 — Identify Procedures

**Goal:** Trace every distinct execution path through your DDM, name each one,
validate the model, and confirm that your procedures represent genuinely
different operation chains.

---

## Step 8: Trace and Name Distinct Procedures

Examine your completed DDM and trace every unique path from start to finish.

### What Defines a Procedure

A procedure is a distinct path through essential operations. Two paths are
distinct if they diverge at any point — even if they converge later.

**Same procedure:** Different tools, same essential operations.
**Different procedure:** Different essential operations at any point in the
chain.

### Assigning IDs

```
Format:   TRR####.PLATFORM.LETTER
Examples: TRR0000.WIN.A
          TRR0000.WIN.B
          TRR0000.WIN.C

Where:
  TRR####   = TRR number (use XXXX as placeholder if unassigned)
  PLATFORM  = WIN, LNX, MAC, AD, AZR, AWS, GCP, K8S, NET, etc.
  LETTER    = A, B, C, etc. — one per procedure
```

### Build the Procedure Table

For each procedure, fill in:

```markdown
| ID | Name | Summary | Distinguishing Operations |
|----|------|---------|--------------------------|
| TRR####.WIN.A | Descriptive Name | One-sentence summary | What makes this path unique |
| TRR####.WIN.B | Descriptive Name | One-sentence summary | What makes this path unique |
```

**The "Distinguishing Operations" column is critical.** It forces you to
articulate exactly what essential operation(s) make each procedure unique.
If you can't fill this column, your procedures may not be genuinely distinct.

### Naming Procedures

Choose descriptive names that capture what makes each procedure unique at
the operation level, not at the tool level.

**Good names:**
- "OS Command Execution" (distinguishing operation: Spawn Process)
- "In-Process API Execution" (distinguishing operation: Call .NET API)
- "Handler Remapping via Configuration" (distinguishing operation: Write Config)
- "Legacy EventLog RPC" (distinguishing operation: ElfrClearELF via MS-EVEN)
- "Modern EventLog RPC" (distinguishing operation: EvtRpcClearLog via MS-EVEN6)

**Bad names:**
- "Mimikatz Method" (tool name)
- "Common Approach" (not descriptive)
- "Method 1" (meaningless)

---

## Step 9: Validate the Model

Before writing any TRR prose, answer every one of these validation questions.
If the answer to any question is "no," go back and fix the DDM before
proceeding.

### Completeness Check

- [ ] Can an attacker execute this technique using ONLY the operations in
      the DDM? (If your DDM is missing an operation the attacker must
      perform, the model is incomplete.)
- [ ] Does every operation pass the inclusion test? (Essential + Immutable +
      Observable, or E+I with gap noted.)
- [ ] Are there any tangential elements that slipped through? (Tool names,
      file paths, flags, delivery methods.)
- [ ] Have all realistic alternate paths been explored?
- [ ] Are procedures distinct — different essential operations, not just
      different tools?
- [ ] Are scoping decisions documented with rationale?

### Accuracy Check

- [ ] Are technical details correct? (Verified against documentation, not
      assumed.)
- [ ] Are there any hidden assumptions? (If you wrote "this probably works
      like..." anywhere, that's an assumption.)
- [ ] Does the model match real-world implementations? (Check against PoC
      code, vendor reports, Atomic Red Team tests.)
- [ ] Are references cited for technical claims?
- [ ] Are prerequisites modeled correctly (not inline with pipeline)?
- [ ] Are sub-operations at the right abstraction level?

### Utility Check

- [ ] Could a red teamer understand how to execute the technique from this
      model?
- [ ] Could a detection engineer identify what to monitor?
- [ ] Could an incident responder understand what artifacts to look for?
- [ ] Are no environment-specific assumptions baked in?
- [ ] Are both common and uncommon procedures covered?

### The "Known Tools" Cross-Check

Take your list of known tools/implementations for this technique (from the
MITRE ATT&CK procedure examples, vendor reports, and PoC repos). For each
one:

1. Trace its execution through your DDM.
2. It should map to one of your identified procedures.
3. If a known tool doesn't map to any procedure, either:
   - You missed a procedure (go back and add it), OR
   - The tool uses the same essential operations as an existing procedure
     (confirm this and note it).

If multiple tools all map to the same procedure, that's a good sign — it
means your DDM is capturing the essential operations correctly, independent
of tooling.

---

## Checkpoint: Ready for Phase 4?

Before advancing to TRR writing, verify:

- [ ] Every procedure has a unique ID, descriptive name, and clearly
      articulated distinguishing operations.
- [ ] The procedure table is complete.
- [ ] All validation questions answered "yes."
- [ ] The known-tools cross-check passes — every known implementation maps
      to an identified procedure.
- [ ] No unresolved `[?]` marks remain in the DDM.
- [ ] You can walk through each procedure's path and explain every operation,
      every causal link, and every telemetry annotation.
