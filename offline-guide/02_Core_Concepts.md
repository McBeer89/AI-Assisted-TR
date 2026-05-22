# Section 2: Core Concepts Quick Reference

Internalize these before you touch any research material. Every decision you
make during TRR production flows from these concepts. If you find yourself
unsure about a judgment call later, come back here — the answer is almost
always one of these principles applied to your specific situation.

---

## The DDM Inclusion Test

This is the single most important filter in the entire methodology. You will
apply it to every operation you consider putting in a DDM — no exceptions.

An operation belongs in the DDM **only** if it passes all three parts of a
compound filter:

| Test | Question | If It Fails... |
|------|----------|----------------|
| **Essential** | Must this operation happen for the technique to work? | It's optional — remove it. |
| **Immutable** | Can the attacker change or avoid this operation? | It's tangential — remove it. |
| **Observable** | Can any telemetry source theoretically see this operation? | Note the gap, but keep it if Essential + Immutable both pass. |

**All three must pass.** Fail any one → the operation does not belong in the
DDM as-is.

The one exception: an operation that is Essential and Immutable but NOT
Observable stays in the model with the observability gap noted. This is
valuable information — it tells the reader where detection is structurally
impossible with current telemetry.

### What Fails the Filter

Operations that fail typically land in one of two buckets:

**Optional** — can be skipped without breaking the technique. Fails the
Essential test. Example: an attacker *can* enumerate running processes before
injecting into one, but process injection doesn't *require* enumeration.

**Tangential (Attacker-Controlled)** — the attacker chooses this element and
can change it freely. Fails the Immutable test. Examples:

- Specific tools or frameworks (Mimikatz, China Chopper, CobaltStrike)
- Command-line parameters and flags
- File names and paths chosen by the attacker
- Delivery methods (exploit, RDP, WebDAV, stolen credentials)
- Encoding or obfuscation techniques
- Programming language or script variant

**When in doubt, decompose.** If an operation feels important but fails the
test, break it down further. There is often an essential/immutable/observable
core buried inside a vaguely-defined operation. "Run web shell" fails
(tool-focused). "Execute Code" passes — it's the essential operation underneath
that the attacker cannot avoid regardless of what tool they use.

---

## Procedures vs. Instances

- A **procedure** is a recipe — a unique pattern of essential operations.
- An **instance** is a specific execution — one cake made from that recipe.

The key question when evaluating whether something is a new procedure:

> **"Does this change the essential operations, or just the implementation
> details?"**

- Different tools executing the same essential operations = **same procedure**.
  (Mimikatz and a custom C tool both reading LSASS process memory = one
  procedure.)
- Different essential operation paths = **different procedures**. (Spawning a
  child process vs. calling an API in-process = two procedures — the operation
  chain is fundamentally different.)

**Focus on identifying distinct procedures, not cataloging infinite instances.**
There are infinite ways to run a web shell, but only a handful of structurally
distinct operation chains.

---

## Discipline-Neutrality

TRRs are source material for any security team. They document how a technique
works at the essential operation level. A TRR does NOT:

- Prescribe detection strategy
- Recommend specific tools or products
- Assume a particular defensive posture
- Rank operations by "detection value" or "fidelity"
- Use phrases like "primary detection opportunity," "high-fidelity signal,"
  "defenders should," or "this is the best place to detect"

**State technical facts. Let each team draw their own conclusions.**

A detection engineer will read your TRR and decide what to monitor. A red
teamer will read the same TRR and decide what to emulate. An incident
responder will read it and decide what artifacts to look for. The TRR serves
all of them by documenting the technique without bias toward any one team's
perspective.

Detection methods, coverage analysis, blind spot assessments, lab recreation
guides, hunt playbooks, and IR runbooks are all **derivative documents** —
separate files produced by specific teams using the TRR as their source
material.

---

## Action Object Naming

Every operation in the DDM is named as a verb phrase: an action performed on an
object. This keeps operations specific, decomposable, and free of tool
references.

| Good (Action Object) | Bad (Vague / Tool-Focused) |
|---|---|
| Route Request | Handle HTTP |
| Match Handler | Process File |
| Execute Code | Run Web Shell |
| Spawn Process | Use cmd.exe |
| Write Registry Key | Modify System |
| Queue APC | Inject Code |
| Send HTTP Request | Connect to Server |
| Compile ASPX | ASP.NET Processing |
| Open Process Handle | Access Target |
| Read Process Memory | Dump Credentials |

If you can't name an operation as "Action Object," it's probably too vague or
too tool-specific. Decompose it or strip the tool reference.

---

## Tangential vs. Essential: A Quick Self-Test

When you're about to add something to your DDM, run this mental checklist:

1. **Can the attacker skip this entirely and still succeed?** → If yes, it's
   optional. Remove it.
2. **Can the attacker swap this for something else?** → If yes, it's
   tangential. Remove it.
3. **Is this a tool name, filename, flag, or delivery method?** → Almost
   certainly tangential. Remove it.
4. **Is this forced by the underlying technology?** → Now you're getting
   somewhere. This might be essential and immutable.
5. **Can I name this as "Action Object" without mentioning a specific tool?**
   → If not, you haven't found the essential operation yet. Keep decomposing.

---

## Structural Conventions at a Glance

These are covered in detail in Phase 2, but worth previewing:

- **Prerequisites** feed into the pipeline as separate nodes — they are NOT
  Step 1 in a linear chain. (A file written to disk days before the HTTP
  request that triggers it is a prerequisite, not an inline pipeline step.)
- **Sub-operations** use downward arrows from the parent node when a lower
  abstraction layer produces its own telemetry.
- **Branch points** use labeled arrows with conditions
  (e.g., "if OS command" / "if in-process").
- **Telemetry labels** go on the specific operation each source observes —
  never grouped on a single "detection" node.
- **Multi-machine techniques** optionally use green circles for
  source/attacker operations and blue circles for target/victim operations.

---

## The Phase Gate Rule

Do not advance to the next phase until the current phase's checkpoint is
satisfied. This applies whether you're working alone, with AI, or in a team.
The gates exist because:

- Phase 1 errors (bad scoping) cascade into Phase 2 (wrong operations in DDM)
- Phase 2 errors (tangential elements) cascade into Phase 3 (false procedures)
- Phase 3 errors (conflated procedures) cascade into Phase 4 (misleading TRR)

Catching errors at their source is always cheaper than catching them downstream.
