# Appendix A: DDM Conventions Cheat Sheet

Quick-reference for all DDM structural and visual conventions.

---

## Node Types

| Element | Visual | Notes |
|---------|--------|-------|
| Standard operation | Black/gray circle | Default for most operations |
| Source/attacker operation | Green circle (`#68bc00`) | Optional — use when multi-machine context matters |
| Target/victim operation | Blue circle (`#009ce0`) | Optional — use when multi-machine context matters |
| Out-of-scope / context-only | Gray circle, lighter shade | Rarely used; only when showing an adjacent operation for context |

## Arrow Types

| Element | Color | When to Use |
|---------|-------|-------------|
| Pipeline flow | Black (`#000000`) | Standard operation-to-operation flow |
| Active procedure path | Red (`#f44e3b`) | Per-procedure exports only — highlights which path this procedure takes |
| Inactive path (context) | Black (`#000000`) | Per-procedure exports — other paths shown for context |
| Prerequisite feed-in | Black (`#000000`) | Prerequisite node → pipeline operation it feeds into |
| Sub-operation (lower layer) | Black (`#000000`), downward | Parent operation → sub-operation below it |

## Naming Convention

Every node caption uses **Action Object** format:

```
[Verb] [Object]

Examples:
  Route Request
  Execute Code
  Spawn Process
  Write File
  Queue APC
  Open Process Handle
  Call RPC
  Receive RPC
  Set Registry Value
  Overwrite File
  Clear Event Log
  Compile ASPX
  Match Handler
  Send HTTP Request
  Read Process Memory
```

## Properties / Tags

Annotate nodes with **immutable technical details** only:

- ✅ API names: `API: NtWriteFile`
- ✅ Process names: `Process: svchost.exe`
- ✅ Service names: `Hosted Service: EventLog`
- ✅ Registry paths: `Key: HKLM\SYSTEM\CurrentControlSet\Services\EventLog\...`
- ✅ RPC details: `RPC: ElfrClearELFA`, `Interface: MS-EVEN`, `OpNum: 12`
- ✅ Permissions: `Permissions: Administrator`
- ❌ Tool names: `Tool: Mimikatz`
- ❌ File names chosen by attacker: `File: evil.aspx`
- ❌ Command-line arguments: `Args: /c whoami`

## Telemetry Labels

Place on the specific operation each source observes. Use descriptive format:

```
✅ Sysmon 1 (ProcessCreate)
✅ Sysmon 3 (NetworkConnect)
✅ Sysmon 7 (ImageLoad)
✅ Sysmon 11 (FileCreate)
✅ Sysmon 12 (RegistryCreate)
✅ Sysmon 13 (RegistryValueSet)
✅ Win 4688 (ProcessCreate)
✅ Win 4663 (SACL)
✅ Win 4657 (requires SACL)
✅ Win 1102 (SecurityLogCleared)
✅ Win 4689 (ProcessTerminate)
✅ IIS W3C
✅ CS EndOfProcess
✅ CS EventLogCleared
✅ CS RegSystemConfigValueUpdate

❌ Sysmon 1           (missing event name)
❌ Event 4688         (ambiguous — which log?)
❌ Sysmon EID 11      (non-standard format)
❌ Process Monitoring  (not a telemetry source)
```

## Structural Conventions Summary

```
PREREQUISITES — feed into the pipeline, not inline:

    [Write File] ──────────┐
                           v
    [Send Request] → [Route Request] → [Execute Code]


SUB-OPERATIONS — downward from parent:

    [Execute Code]
          |
          v
    [Compile ASPX]


BRANCHES — labeled arrows:

    [Execute Code]
          |                  \
          v                   v
    [Spawn Process]     [Call .NET API]
    "if OS command"     "if in-process"


MULTI-MACHINE — green source, blue target:

    (green) [Call RPC] ────→ (blue) [Receive RPC]
```

---

## Arrows.app JSON Structure Reference

**Important:** This reference is based on actual DDM JSON files from published
TRRs (TRR0016, TRR0023, TRR0027). If anything here contradicts what you see
when you export from Arrows.app, trust the app's output — Arrows.app's format
may evolve.

### Top-Level Structure

The JSON has three top-level keys — **no `"graph"` wrapper**:

```json
{
  "style": { ... },
  "nodes": [ ... ],
  "relationships": [ ... ]
}
```

### Graph-Level Style Block

This sets defaults for the entire diagram. Nodes and relationships can
override these with their own `"style"` objects.

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 2,
    "border-color": "#000000",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 16,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 1,
    "label-font-size": 16,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 5,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 16,
    "property-font-weight": "normal"
  },
  "nodes": [],
  "relationships": []
}
```

### Node Format

```json
{
  "id": "n0",
  "position": { "x": 75, "y": 50 },
  "caption": "Execute Code",
  "labels": ["Sysmon 1 (ProcessCreate)"],
  "properties": {
    "Process": "w3wp.exe"
  },
  "style": {}
}
```

**Key fields:**
- `id` — unique node identifier (e.g., `"n0"`, `"n1"`, `"n2"`)
- `caption` — the Action Object name displayed inside the circle
- `labels` — **array of strings** for telemetry annotations, displayed as
  pills outside the node
- `properties` — key-value pairs for immutable technical details (API names,
  process names, registry paths), displayed outside the node
- `style` — node-level overrides. Empty `{}` inherits graph defaults.

**Colored nodes (multi-machine techniques):**
- Green source: `"style": { "border-color": "#68bc00" }`
- Blue target: `"style": { "border-color": "#009ce0" }`

### Relationship Format

```json
{
  "id": "n0",
  "fromId": "n0",
  "toId": "n1",
  "type": "if OS command",
  "properties": {},
  "style": {}
}
```

**Key fields:**
- `id` — unique relationship identifier
- `fromId` / `toId` — which nodes this arrow connects
- `type` — branch condition label displayed on the arrow. Empty string `""`
  for unlabeled arrows.
- `style` — relationship-level overrides. Empty `{}` inherits graph defaults.

**Red arrows for per-procedure exports:**
```json
"style": {
  "arrow-color": "#f44e3b"
}
```

Only set `arrow-color` on relationships in the active procedure's path.
Leave all other relationships with empty `style: {}` (they inherit the
graph-level black default).

### Minimal Working Example

Two operations connected by a labeled arrow:

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 2,
    "border-color": "#000000",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 16,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 1,
    "label-font-size": 16,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 5,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 16,
    "property-font-weight": "normal"
  },
  "nodes": [
    {
      "id": "n0",
      "position": { "x": 0, "y": 0 },
      "caption": "Execute Code",
      "labels": [],
      "properties": {},
      "style": {}
    },
    {
      "id": "n1",
      "position": { "x": 300, "y": 0 },
      "caption": "Spawn Process",
      "labels": ["Sysmon 1 (ProcessCreate)"],
      "properties": {
        "Process": "w3wp.exe → cmd.exe"
      },
      "style": {}
    }
  ],
  "relationships": [
    {
      "id": "n0",
      "fromId": "n0",
      "toId": "n1",
      "type": "if OS command",
      "properties": {},
      "style": {}
    }
  ]
}
```

The best way to learn the format is to import a published TRR's DDM JSON
into Arrows.app and study its structure. The JSON files from TRR0016,
TRR0023, and TRR0027 in the TIRED Labs repository are good references.
