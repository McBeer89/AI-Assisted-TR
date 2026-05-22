# Section 4: Phase 1 — Build Understanding & Scope

**Goal:** Learn the technique well enough to explain it simply and accurately,
then define exactly what your TRR covers and what it excludes.

Do NOT touch the DDM yet. You are building the foundation that everything else
rests on. Rushing past this phase is the most expensive mistake in the entire
process.

---

## Step 1: Understand the Technique

Read through your collected sources with these questions in mind:

### Basic Understanding

- What is the technique called?
- What tactic(s) does it accomplish (initial access, persistence, defense
  evasion, etc.)?
- What platform(s) does it affect?
- What is the attacker's objective — what do they gain by using this technique?
- Why do attackers choose this technique over alternatives?

**Self-test:** Can you explain this technique to someone in 2-3 sentences
without mentioning any specific tools? If not, keep reading.

### Technical Background

- What system components does this technique interact with? (Services,
  subsystems, kernel components, protocols)
- What APIs, protocols, or mechanisms are involved?
- What are the prerequisites for this technique to work? (Permissions,
  configurations, prior access)
- What is the normal (benign) use of these system components?
- What security controls exist that this technique exploits, bypasses, or
  abuses?
- How does the system process the attacker's action from start to finish?
  (Trace the full pipeline.)

**Self-test:** Can you trace the path from the attacker's initial action to
the final system effect, naming the specific system components involved at
each step? If not, you have gaps to fill.

### How to Fill Gaps Without AI

When working with AI, you'd ask "explain how IIS processes an HTTP request"
and get an immediate walkthrough. Offline, you need to find this yourself:

- **Microsoft documentation** is your primary source for Windows internals.
  Search your saved docs for architecture overviews and service descriptions.
- **Trace backwards from telemetry.** If you know Sysmon Event 1 fires for
  process creation, work backwards: what causes process creation in this
  context? What system component initiates it?
- **Read the PoC code.** Proof-of-concept implementations reveal the actual
  API call chain. Ignore the tool wrapper — focus on what Windows APIs get
  called and in what order.
- **Draw the pipeline.** Even rough sketches on paper help. Map "attacker does
  X → system component A processes it → component B receives it → result Y
  happens." Gaps in your pipeline drawing are gaps in your understanding.

### Write Your Technical Background Notes

As you research, write notes in `Supporting Docs/phase1_research.md`. Use this
structure:

```markdown
# Research: [Technique Name]

## Technique Summary
[2-3 sentences: what it is, what it accomplishes, why attackers use it]

## Technical Background Notes
[Everything you've learned about the underlying technology:
 - System architecture
 - Execution flow / pipeline
 - Relevant APIs and their roles
 - Security contexts and permissions
 - Prerequisites]

## Open Questions
- [?] [Anything you're uncertain about — mark honestly]

## Sources Consulted
- [List with brief notes on what each source contributed]
```

**Tag operations as you encounter them.** As your research surfaces specific
operations, start tagging them:

- `[EIO]` — Essential + Immutable + Observable → likely DDM candidate
- `[TANGENTIAL]` — Attacker-controlled, fails immutability
- `[OPTIONAL]` — Can be skipped, fails essential test
- `[?]` — Uncertain, needs more research

You're not building the DDM yet — you're gathering the raw material for it.

---

## Step 2: Define Your Scope

Once you understand the technique, define boundaries. Scoping decisions made
here determine everything downstream.

### Write a Scope Statement

One precise sentence defining what this TRR covers. If you need more than one
sentence, the scope isn't tight enough.

**Good examples:**
- "File-based web shell execution via IIS on Windows."
- "Clearing Windows Event Logs via the EventLog service API."
- "WMI Event Subscription-based persistence on Windows."

**Bad examples:**
- "Web shells." (Too broad — which platform? Which server? File-based or
  memory-based?)
- "This TRR covers the use of file-based web shells deployed to IIS web
  servers running on Windows, including ASPX, ASP, and PHP web shells that
  execute operating system commands or in-process .NET API calls." (Too long —
  this is a paragraph, not a scope statement. The detail belongs in the
  technique overview or exclusion table.)

### Build the Exclusion Table

Document what is explicitly out of scope and why. During research, be
exhaustive — capture everything you're excluding. You'll condense this later
when writing the final TRR.

| Excluded Item | Rationale |
|---|---|
| *Example: Memory-only web shells* | *Different essential operations (no file write); separate TRR* |
| *Example: Linux/Apache web servers* | *Different platform, different pipeline architecture* |
| *Example: Specific tools (China Chopper, etc.)* | *Tangential — same essential operations regardless of tool* |

**Exclusion rationale should reference the DDM inclusion test:**
- "Tangential" = attacker-controlled, fails the immutability test
- "Different essential operations" = warrants a separate TRR
- "Same essential operations" = same procedure, not a new exclusion — just
  a different instance

### Build the Essential Constraints Table

What MUST be true for this technique to work?

| # | Constraint | Essential? | Immutable? | Observable? | Telemetry |
|---|-----------|------------|------------|-------------|-----------|
| 1 | *Attacker must have Administrator privileges* | ✅ | ✅ | ❌ | *N/A — not an operation* |
| 2 | *EventLog service must be running* | ✅ | ✅ | ✅ | *Win 7036 (ServiceControl)* |

Not every constraint will map directly to a DDM operation — some are
preconditions (like permissions) rather than operations. That's fine. The
table helps you think through what the technique requires.

### Scoping Decisions That Require Judgment

Some scoping questions don't have obvious answers. Common ones:

- **Should staging/preparation operations be in scope?** If an attacker must
  stage data before exfiltrating, is the staging in scope for the exfiltration
  TRR? General rule: if it's a prerequisite that changes the essential
  operations, include it. If it's a separate technique with its own operation
  chain, exclude it and note the dependency.

- **Should pre-conditions with heavy dependencies be separate procedures?**
  If one execution path requires a specific software client to be installed
  (e.g., a sync folder agent), and another path doesn't — are these the same
  TRR? General rule: if the essential operations diverge because of the
  precondition, they're different procedures within the same TRR.

- **How far down the pipeline do you go?** If the technique involves code
  execution, do you model what happens after code executes? General rule: stop
  at the technique boundary. A web shell executing code is T1505.003; what
  the code does (spawn a process, call an API) is the next technique in the
  chain. But if different post-execution paths define distinct procedures
  within your technique, include them.

**When in doubt, write the question down.** It's better to have an explicit
open question than a silent assumption.

---

## Step 3: Document Telemetry Facts (Not Prescriptions)

As you research, you'll encounter information about what telemetry sources
observe which operations. Record these as technical facts:

**Do this:**
- "IIS logs HTTP requests in W3C Extended Log Format by default, capturing
  the URI, method, status code, and client IP."
- "Sysmon Event ID 1 (ProcessCreate) records process creation including the
  parent process, command line, and image path."
- "Windows Event 1102 fires when the Security log is cleared, recording the
  account that performed the action."

**Do NOT do this:**
- "Enable Sysmon Event 1 to detect this operation." (Prescriptive —
  derivative document territory.)
- "Set audit policy to capture Event 4663." (Enablement guidance — not TRR
  content.)
- Do NOT build tables with "Default State" or "How to Enable" columns.

Telemetry facts go in your Technical Background notes as inline prose. The
detection team will decide what to enable; you're documenting what exists.

---

## Checkpoint: Ready for Phase 2?

Before advancing to DDM construction, verify:

- [ ] You can explain the technique in 2-3 sentences without mentioning tools.
- [ ] You can trace the full operation pipeline from attacker action to system
      effect.
- [ ] Your scope statement is exactly one sentence.
- [ ] Your exclusion table documents what's out and why, referencing the
      inclusion test.
- [ ] Your essential constraints table captures prerequisites.
- [ ] You've tagged operations as `[EIO]`, `[TANGENTIAL]`, `[OPTIONAL]`, or
      `[?]`.
- [ ] All `[?]` items are documented — not resolved by assumption.
- [ ] Your Technical Background notes are written and sourced.

If any `[?]` items remain that are critical to understanding the technique,
you need to resolve them before proceeding. Non-critical uncertainties can be
carried forward and resolved during DDM construction, but flag them clearly.
