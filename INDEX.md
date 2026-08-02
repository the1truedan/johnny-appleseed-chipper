# Public index — Johnny Appleseed + C.H.I.P.P.E.R.S. + C.H.A.I.N.S.

**Purpose:** Hand-off map for contributors and future-you when this design
becomes real code. Nothing here requires private LAN paths or care data.

| Doc | What it is |
|-----|------------|
| [README.md](./README.md) | Public-facing problem statement + seam sketch |
| [docs/PROCESS_TEMPLATES.md](./docs/PROCESS_TEMPLATES.md) | Reusable historical process templates (scan → hash → gate → catalog) |
| [docs/STAGED_CATALOG_LAYOUT.md](./docs/STAGED_CATALOG_LAYOUT.md) | How to stage indexes without walking NFS from agents |
| [docs/DEDUP_LAYERS.md](./docs/DEDUP_LAYERS.md) | L1 structure / L2 content-hash / L3 bees — do not conflate |
| [docs/HANDOFF_CHECKLIST.md](./docs/HANDOFF_CHECKLIST.md) | Pre-public and pre-merge checklist |

## Domain one-liners

- **Johnny Appleseed** — classify materials you **already own** (no crawl).
- **C.H.I.P.P.E.R.S.** — Content Hashing, Inspection, Parsing, Provenance, Extraction, Retrieval, Simplification.
- **C.H.A.I.N.S.** — Custody History Assurance Integrated Network System (tamper-evident receipts, not legal immutability).

## Related public stacks

| Repo / surface | Role |
|----------------|------|
| [ai-gateway](https://github.com/the1truedan/ai-gateway) | LAN model door + observability notes (sanitized) |
| [fast-models](https://github.com/the1truedan/ai-gateway) `deploy/` notes | Shared AI file pool; bees sizing history |

## Status

Design + process templates. Implementation intentionally gated; see HANDOFF
checklist before writing production parsers.
