# Section 8: Phase 5 — DDM Exports

**Goal:** Produce the final DDM artifact set — a master DDM and per-procedure
exports — in Arrows.app JSON and PNG formats.

---

## The DDM Artifact Set

Every completed TRR includes:

1. **Master DDM** — all operations, all paths, all telemetry. All arrows in
   black. This is the complete picture.

2. **Per-procedure DDM exports** — one per procedure. Each highlights its
   active path so the reader can see exactly which operations belong to that
   procedure.

3. **PNG exports** — screenshot/export of each DDM for embedding in the TRR
   markdown.

---

## File Naming Convention

```
Master DDM:
  ddm_trr####_platform.json      (Arrows.app JSON, all black arrows)
  ddm_trr####_platform.png       (master image)

Per-procedure exports:
  trr####_platform_a.json        (Arrows.app JSON, red arrows on active path)
  trr####_platform_a.png         (image referenced in TRR)
  trr####_platform_b.json
  trr####_platform_b.png
  trr####_platform_c.json
  trr####_platform_c.png

Examples:
  ddm_trr0016_win.json
  ddm_trr0016_win.png
  trr0016_win_a.json
  trr0016_win_a.png
  trr0016_win_b.json
  trr0016_win_b.png
```

All DDM files go in the `ddms/` directory under your TRR:
```
TRR####/win/ddms/
```

---

## Master DDM Rules

- Contains every operation across all procedures.
- All arrows are black (`#000000`).
- All telemetry annotations are present.
- All branch points are labeled with conditions.
- All prerequisites and sub-operations are included.
- This is the "God view" — the complete model.

---

## Per-Procedure Export Rules

How you produce each per-procedure export depends on whether procedures share
a pipeline.

### Shared Pipeline Procedures

When two or more procedures share a common pipeline and diverge at a branch
point:

- Each per-procedure export contains the **entire DDM** — all nodes, all
  relationships.
- The active procedure's path is highlighted with **red arrows** (`#f44e3b`).
- Inactive paths remain in **black arrows** (`#000000`) for context.
- The reader needs the full picture to see where the divergence happens.

**Example:** Procedures A and B both transit the IIS request pipeline
(Route Request → Match Handler → Execute Code), then diverge — A spawns a
process, B calls an API. Both per-procedure exports show the complete DDM.
Procedure A's export has red arrows on the path through Spawn Process;
Procedure B's export has red arrows on the path through Call .NET API.

### Independent Pipeline Procedures

When a procedure has its own completely separate operation chain with no
shared operations:

- Its per-procedure export contains **only that procedure's operations and
  relationships**.
- The unrelated pipeline is omitted entirely.
- Including unrelated operations would be visual noise with no shared context
  to preserve.

**Example:** If Procedure C in a hypothetical TRR uses an entirely different
mechanism that shares zero operations with Procedures A and B, Procedure C's
export contains only its own operation chain.

### Mixed Cases

A single TRR may have both patterns. The deciding question for each procedure:

> **Does this procedure share any operations with another procedure?**

- **Yes** → include the full DDM in this procedure's export (red on active,
  black on inactive).
- **No** → isolate this procedure's export (only its own operations).

---

## Producing Arrows.app JSON

### If Working in Arrows.app Directly

1. Build or import the master DDM.
2. For each per-procedure export:
   - Select the arrows on the active path.
   - Change their color to `#f44e3b` (red).
   - Export the JSON.
   - Reset arrows to black before doing the next procedure.
3. Export the master DDM JSON with all arrows black.

### If Writing JSON by Hand

The Arrows.app JSON format uses this structure:

```json
{
  "graph": {
    "style": { ... },
    "nodes": [
      {
        "id": "n0",
        "position": { "x": 100, "y": 200 },
        "caption": "Operation Name",
        "style": { ... },
        "labels": [],
        "properties": {
          "key": "value"
        }
      }
    ],
    "relationships": [
      {
        "id": "r0",
        "type": "",
        "style": {
          "arrow-color": "#000000"
        },
        "properties": {},
        "fromId": "n0",
        "toId": "n1"
      }
    ]
  }
}
```

**Key fields for operations (nodes):**
- `caption` — the Action Object name (e.g., "Execute Code")
- `properties` — immutable details (API names, process names, registry paths,
  telemetry labels)
- `style` — circle color. Default black/gray. Green (`#68bc00`) for
  source/attacker operations. Blue (`#009ce0`) for target/victim operations.

**Key fields for relationships (arrows):**
- `fromId` / `toId` — which operations this arrow connects
- `type` — branch condition label (e.g., "if OS command")
- `style.arrow-color` — `#000000` for master/inactive, `#f44e3b` for
  active procedure path

See Appendix A for a more complete JSON reference. The best approach is to
study the JSON from a published TRR's DDM and adapt it.

---

## Producing PNG Exports

**In Arrows.app:** Use the export/download function to save a PNG of each DDM
view.

**If offline without Arrows.app:** You can produce PNGs later when you have
access, or use screenshots of hand-drawn diagrams as placeholders. The JSON
is the authoritative artifact; the PNG is a convenience for TRR readers.

---

## Final File Checklist

After completing all exports, verify:

- [ ] Master DDM JSON exists with all arrows black.
- [ ] Master DDM PNG exists.
- [ ] One per-procedure JSON exists for each procedure, with the active path
      in red (`#f44e3b`).
- [ ] One per-procedure PNG exists for each procedure.
- [ ] Shared-pipeline procedures include the full DDM in their exports.
- [ ] Independent-pipeline procedures include only their own operations.
- [ ] All files follow the naming convention.
- [ ] All files are in the `ddms/` directory.
- [ ] TRR markdown image references match the actual PNG filenames.

---

## You're Done

At this point you have:

- A complete, validated DDM (master + per-procedure exports)
- A discipline-neutral TRR document
- Research notes and sources in Supporting Docs
- A folder structure ready for repository submission

The TRR is your lossless source material. Any team — detection, emulation,
incident response, threat intelligence — can now use it to produce their own
derivative documents.
