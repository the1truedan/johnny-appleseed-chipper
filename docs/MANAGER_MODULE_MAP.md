# M.A.N.A.G.E.R. module map (design sketch)

How **Johnny Appleseed**, **C.H.I.P.P.E.R.S.**, and **C.H.A.I.N.S.** are meant to
sit next to other M.A.N.A.G.E.R. surfaces. This is architecture intent — not a
claim that every row is implemented in this repository.

Host names below use **roles** only (`desk-host`, `gpu-host`, `nas-host`).

## The three layers

| Layer | Job |
|-------|-----|
| **Johnny Appleseed** | Classify materials the operator **already holds** (no open-web crawl). |
| **C.H.I.P.P.E.R.S.** | Content hash, inspect, parse when safe, emit stable artifacts + indexes. Unknown binaries → metadata only. |
| **C.H.A.I.N.S.** | Hash-linked custody receipts (actor, time, scope, destination, prev hash). **Tamper-evident bookkeeping**, not a court seal. |

Rendition ladder (each tier records parent digest):

```text
00_original → 10_assured → 20_extracted → 30_cleaned → 40_simplified →
45_markdown → 50_accessible → 60_index → 70_exports
```

## Module interactions (planned or partial)

| M.A.N.A.G.E.R. surface | Interaction |
|------------------------|-------------|
| **Ingestion / personal-docs staging** | Johnny classifies drops; CHIPPER content-hash dedups; CHAINS receipt on promote into a vault/catalog. |
| **A.I.D.A. / OCR / form assist** | Scanned JPG/PDF enter the format-adapter matrix; OCR/text becomes a child digest with parser version; high-stakes fields stay human-reviewed. |
| **PDF documents** | Original bytes hashed → extract/OCR child → cleaned/markdown child → CHAINS event with scope/consent. Optional sidecar receipt id (never claim legal immutability). |
| **Creative render pipelines** | Adjacent live pattern: append-only render receipts (hash-linked) for generative jobs — same *shape* as custody, separate chain id from care materials. |
| **Storage / model pools** | Catalog indexes and content-hash reclaim sit beside bees-style physical dedup; do not conflate file-hash reclaim with block-level storage reclaim. |
| **Local build dependencies** | Manifest scan (`package.json`, `requirements.txt`, `go.mod`, …) feeds SBOM/vuln-style inspection when built; results are indexed artifacts, not auto-upgrades. |
| **Tool version board** | Coding CLIs and host tools recorded as `{tool_id, version, path, manager, probed_at}` for future env-mutation receipts. |
| **brew / apt / asdf / Windows / macOS / Linux updates** | **Workstation inventory earmark only** — report staged versions; no automatic system update install from this design. Prefer explicit operator approval per host role. |
| **Model versioning** | Weight/sidecar hashes + vendor metadata freshen (report-only queues); promote only after human review and hash verify. |
| **Cited web / document history** | Allowlisted acquisition records URL, retrieve time, redirects, headers, license decision; originals never overwritten; re-index policies keep history. |
| **Archival of versioning** | Append-only receipts and index waves; old receipt lines are not rewritten in place (see process templates). |
| **LLM gateway / coding agents** | Outside the file custody chain, but LLM-call anomalies can feed the **same** audit spine as another event class (policy, not a second product name). |
| **Public release trees** | Template F sanitize: strip secrets, replace LAN facts with roles, keep process truth. |

## PDF chain-of-custody (story)

1. Drop or allowlisted acquire of PDF bytes.  
2. CHIPPER: raw content hash + MIME assure tier.  
3. Extract/OCR → child digest + tool/parser version.  
4. Clean / simplify / markdown tiers as needed.  
5. CHAINS event: actor, UTC, scope/consent, allowed destination, `prev_hash`.  
6. Index document points at digests; export tiers never replace `00_original`.

## Tool / OS update custody attribute (staged shape)

When an environment mutation is recorded later:

```json
{
  "tool_id": "codex",
  "version": "0.x.y",
  "path": "/usr/local/bin/codex",
  "manager": "homebrew",
  "probed_at": "2026-08-02T00:00:00Z",
  "host_role": "desk-host"
}
```

`manager` enum (staged): `homebrew` | `asdf` | `apt` | `npm` | `cargo` | `pip` | `local` | `url` | `winget` | `choco` | `snap` | `fluxdown` | …  
See [PACKAGE_MANAGER_CATALOG.md](./PACKAGE_MANAGER_CATALOG.md).

## What this repo does *not* claim

- No production extractor sandbox ships here yet (docs-first by design).  
- CHAINS is not legal certification, identity proof, or immutable blockchain storage.  
- No automatic publication or cross-host copy.  
- Care/PHI materials stay out of public mirrors.

## See also

- [PROCESS_TEMPLATES.md](./PROCESS_TEMPLATES.md) — Templates A–F including public sanitize  
- [HANDOFF_CHECKLIST.md](./HANDOFF_CHECKLIST.md)  
- [STAGED_CATALOG_LAYOUT.md](./STAGED_CATALOG_LAYOUT.md)  
- [DEDUP_LAYERS.md](./DEDUP_LAYERS.md)  
- [../README.md](../README.md)  
