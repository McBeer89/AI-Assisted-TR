# Section 5: Phase 2 — Build the DDM

**Goal:** Map every essential operation into a Detection Data Model, annotate
telemetry, and discover all alternate execution paths.

This is the analytical core of the entire process. Take your time. The DDM is
the artifact that everything else — procedures, TRR prose, derivative
documents — is built from. Errors here propagate everywhere.

---

## Step 4: Initial Operation Mapping

Start with what you know from Phase 1. Take the operations you tagged as
`[EIO]` and lay them out as a flow.

### How to Map Operations

**If using Arrows.app:** Create a new graph. Add each operation as a node
(circle). Connect them with arrows showing the execution flow. You'll
generate or refine the JSON later.

**If using pen and paper:** Draw circles for operations, arrows for flow.
Label everything. You'll transcribe to Arrows.app JSON when you finalize.

**If transcribing to JSON directly:** Write the Arrows.app JSON structure
by hand or adapt from a reference TRR's DDM JSON. See Appendix A for the
JSON format.

### Naming Every Node

Apply Action Object naming to every operation:

- ✅ "Route Request" — action (route) on object (request)
- ✅ "Execute Code" — action (execute) on object (code)
- ✅ "Spawn Process" — action (spawn) on object (process)
- ❌ "IIS Processing" — vague, no clear action/object
- ❌ "Use cmd.exe" — tool-focused
- ❌ "Detection Point" — not an operation at all

### Modeling Structural Relationships

**Pipeline operations** flow left-to-right. The main execution chain:
attacker action → system processing → technique effect.

**Prerequisites** feed into the pipeline from above or to the side. A file
written to disk days or weeks before the triggering HTTP request is NOT
Step 1 in the pipeline — it's a prerequisite node with an arrow into the
point where the pipeline needs it.

```
Example (correct):

  [Write File]
       |
       v
  [Send HTTP Request] → [Route Request] → [Match Handler] → [Execute Code]


Example (incorrect — prerequisite modeled as inline Step 1):

  [Write File] → [Send HTTP Request] → [Route Request] → [Match Handler] → [Execute Code]
```

The distinction matters because prerequisites have different temporal
relationships and different telemetry windows than inline pipeline operations.

**Sub-operations** use downward arrows from a parent node. If an operation
contains a notable sub-step that produces its own distinct telemetry, model
it below the parent:

```
  [Execute Code]
       |
       v  (sub-operation)
  [Compile ASPX]
```

The sub-operation is a lower abstraction detail — it happens *within* the
parent operation, not *after* it.

**Branch points** occur when the technique can take different paths at a
specific operation. Label each arrow with a condition:

```
  [Execute Code]
       |                    \
       v                     v
  [Spawn Process]      [Call .NET API]
  (if OS command)      (if in-process)
```

Only create branches when the alternate path changes the essential operations.
If two paths use different tools but perform the same essential operations,
they are the same procedure — no branch needed.

### Apply the Inclusion Test to Every Node

As you place each operation, run the test:

1. **Essential?** — Can the technique succeed without this operation?
   If yes → remove it.
2. **Immutable?** — Can the attacker change or avoid this operation?
   If yes → remove it (or decompose to find the immutable core).
3. **Observable?** — Can any telemetry source see this?
   If no → keep it, but note the observability gap.

**Common traps at this step:**

- Including "Enumerate Processes" before process injection — optional, fails
  Essential.
- Including "Upload via WebDAV" — delivery method is tangential, fails
  Immutable.
- Including "Invoke-Mimikatz" — tool name, fails Immutable. The essential
  operation is "Read Process Memory."
- Including a node called "Detection" or "Monitoring" — this isn't an
  operation the attacker performs.

---

## Step 5: Iterative Deepening

For every operation currently in your DDM, work through these questions
one node at a time:

1. **Do I fully understand what is happening at this operation?**
   - What process performs this action?
   - What API or system mechanism does it use?
   - What permissions does it require?

2. **Is this operation specific enough, or does it summarize multiple
   operations that should be split?**
   - "Process Request" might actually be "Route Request → Match Handler →
     Execute Code" — three distinct operations with different telemetry.

3. **Does this operation pass the inclusion test?**
   - Essential? Immutable? Observable?
   - Show your work. Don't just say "yes" — articulate why.

4. **How does this operation cause the next operation?**
   - What is the causal link? System call? Return value? Service
     notification?
   - If you can't explain the causal link, you may have a gap between
     operations.

5. **Is any tangential detail hiding in this operation?**
   - Is a tool name embedded in the operation definition?
   - Is a specific file path or command-line argument baked in?
   - Strip it out. The operation should describe what the system does, not
     what the attacker chose.

### Working Through Uncertainty

If you can't confidently answer all five questions for an operation:

- Mark it with `[?]` in your notes.
- Look for the answer in your source material.
- Try decomposing the operation into smaller steps — the answer often
  becomes clear at a lower abstraction level.
- If you truly cannot resolve it with available sources, **leave the `[?]`
  and note what you'd need to resolve it** (lab test, specific documentation,
  expert consultation). Do not fill the gap with a guess.

**Repeat this for every single node.** This is the most time-consuming part
of the entire process. It's supposed to be.

---

## Step 6: Telemetry Identification

For each operation in your DDM, identify what telemetry sources can observe it.

### Where to Look

- **Windows Security Event Log:** Process creation (4688), object access
  (4663), logon events (4624/4625), audit policy changes, etc.
- **Sysmon:** Process creation (1), file create (11), registry events
  (12/13/14), network connections (3), image loads (7), etc.
- **EDR telemetry:** CrowdStrike, Defender for Endpoint, Carbon Black, etc.
  — these often have proprietary event types.
- **Application logs:** IIS W3C logs, SQL Server logs, Exchange logs, etc.
- **ETW (Event Tracing for Windows):** Lower-level telemetry providers that
  some tools consume.

### How to Annotate

Place each telemetry label on the specific operation it directly observes.
Use descriptive labels that include the event name:

- ✅ `Sysmon 1 (ProcessCreate)`
- ✅ `Sysmon 11 (FileCreate)`
- ✅ `Win 4688 (ProcessCreate)`
- ✅ `Win 4663 (SACL)`
- ✅ `Win 1102 (SecurityLogCleared)`
- ✅ `IIS W3C`
- ❌ `Sysmon 1` (no event name)
- ❌ `Event 4688` (ambiguous source)
- ❌ `Process Monitoring` (not a specific telemetry source)

### Telemetry Placement Rules

- Tag telemetry on the operation it directly observes — NOT grouped on a
  single "telemetry" or "detection" node.
- If multiple sources observe the same operation, list them all on that
  operation.
- If no telemetry exists for an operation, note the gap explicitly. This is
  valuable information.

### Important: State Facts, Not Prescriptions

Your DDM annotates what telemetry *exists* and which operation it *observes*.
It does not say:

- Which telemetry to enable
- Which source is "best"
- What detection logic to write
- Which events to prioritize

Those are decisions for the detection team's derivative document.

---

## Step 7: Alternate Path Discovery

For every operation in your DDM, ask:

- **Is there another way to accomplish this same thing?**
- **Can this operation be skipped entirely?**
- **Are there alternative APIs, protocols, or mechanisms?**

If you find an alternate path, apply the procedure-defining question:

> **Does the alternate path change the ESSENTIAL OPERATIONS?**

- **Yes** → This is a different procedure. Add the branch to the DDM with
  labeled arrows showing the conditions.
- **No** → This is the same procedure with different implementation details
  (tangential). Note it in your research notes but do NOT create a new branch
  in the DDM.

### Examples of Genuine Alternate Paths (Different Procedures)

- After code execution: spawning a child process (cmd.exe) vs. calling a
  .NET API in-process → different essential operations, different procedures.
- Clearing an event log via the legacy MS-EVEN RPC interface vs. the modern
  MS-EVEN6 RPC interface → different RPC calls, different transport, different
  procedures.
- Modifying web.config to change handler mappings (new essential operation:
  Write Config) vs. using the default handler → different operation chain,
  different procedure.

### Examples of Tangential Differences (Same Procedure)

- Using .aspx vs. .asp file extension with the same handler type →
  extension is attacker-controlled, same essential operations.
- Using Mimikatz vs. a custom C tool to read LSASS process memory → tool
  is tangential, same essential operations.
- Uploading via WebDAV vs. RDP vs. exploit → delivery method is tangential.

### Discovering Paths You Missed

Sources for alternate paths:

- PoC repositories often implement the technique differently than the
  "textbook" description.
- Procedure examples on the MITRE ATT&CK page may reference different
  mechanisms.
- Vendor blog posts describing incident response findings sometimes reveal
  uncommon execution paths.
- Your own technical understanding — "if this operation uses RPC, is there
  a local API alternative that bypasses the RPC layer?"

When you find a new path, apply Steps 4-6 to the new operations: map them,
deepen them, identify telemetry.

---

## Checkpoint: Ready for Phase 3?

Before advancing to procedure identification, verify:

- [ ] Every operation in the DDM passes the inclusion test (Essential +
      Immutable + Observable, or E+I with an observability gap noted).
- [ ] Every operation uses Action Object naming.
- [ ] No tangential elements remain (tool names, file paths, flags,
      delivery methods).
- [ ] Prerequisites are modeled as prerequisites, not inline pipeline steps.
- [ ] Sub-operations use downward arrows.
- [ ] Branch points are labeled with conditions.
- [ ] Telemetry is annotated on the specific operation each source observes,
      using descriptive labels.
- [ ] All realistic alternate paths have been explored.
- [ ] No `[?]` marks remain on any operation (or if they do, they are
      non-critical and clearly documented).
- [ ] You can trace each path from start to finish and explain every
      operation and causal link.
