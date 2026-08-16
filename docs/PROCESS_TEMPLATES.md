# Process templates (historical → reusable)

These templates capture **how** the lab actually operated inventory and
dedup work (2026-07 → 2026-08), abstracted so a public repo can teach the
pattern without private hosts, PHI, or absolute NFS paths.

Use them as runbooks when implementing Johnny / CHIPPER, or when filing a
catalog wave for any large model/cache pool.

---

## Template A — Structure inventory (safe probe)

**Goal:** Know what *kinds* of trees exist without unbounded `find` on NFS.

1. **Allowlist tops only** (e.g. `models/`, `pinokio/`, `github/`, `work/`).  
2. Run a **bounded** structure probe (depth cap, timeout, no follow-mount).  
3. Emit a portable index JSON: path class, file counts by extension, size
   buckets — **not** full recursive listings of 10k LoRA sidecars.  
4. Store under a **catalog** prefix, never rewrite originals.  
5. Record operator, UTC time, tool version, host **role** (desk / gpu / nas).

**Anti-pattern:** Agent runs unbounded `find` across a storage-pool mount
from a desk-host NFS client.

**Outputs:** `structure_inventory_<date>.json` + short markdown scorecard.

---

## Template B — Content-hash dual ladder (L2)

**Goal:** Find byte-identical or likely duals without hashing every file full
SHA256 up front.

```text
exact size → sample_hash (4 MiB head/tail) → full SHA256 only on multi-member sample groups
```

| Confidence | Meaning | Auto-delete? |
|------------|---------|--------------|
| basename + size | Weak | No |
| sample match | Escalate | No (unless explicit allow) |
| **sha256** | Proven dual | Only after verification-manifest |
| dir_tree | Same relative paths + sizes | No — structure candidate |

Skip callable venv trees (`.venv` / `venv`) unless the operator asked for
an env index. Do not hash them as weight twins.

**Gate:**

1. Report-only scan → catalog JSON.  
2. Optional **stage** (Template G): keep sha256 groups only, for reading.  
3. Dual-verify on quarantine / candidate set (readable on all required hosts).  
4. Prefer same-filesystem **quarantine** before permanent `rm`.  
5. Manifest must say `verified=true` **and** `all_hosts_readable=true`
   before any execute path. Sample-only is not enough.

**Outputs:** `_ai_data_dedup_audit.json` (or equivalent), dual-verify receipt,
optional quarantine wave id.

---

## Template C — Quarantine dual-verify (CHIPPER-style job)

**Goal:** Promote “looks dual” → “proven dual on all hosts.”

1. Stage **tools** next to the job (scripts are not on the data pool —
   a console that only sees the pool will not find them).  
2. Point the job at an **ai-root** mount *inside* the storage runtime.  
3. Wave directory (hidden ok): `.dedup_quarantine/<wave>/`.  
4. Coverage target (e.g. 95%) + max GB per run for safety.  
5. Emit machine-readable verify JSON + human markdown summary.  
6. Only then allow apply/purge tools with `--verification-manifest`.  
7. If the staged JSON used different field names than the apply tool,
   reshape first. Similar is not the same schema.  
8. Vendor shared-lib risk: match `.so`, `.so.N`, `.dylib` — not a
   literal `.so` suffix.

**Outputs:** `quarantine_dual_verify_<wave>.json` + `.md`.

---

## Template D — bees physical dedupe ops (L3)

**Goal:** Extent-level sharing on Btrfs (not file-level merge advice).

1. Track **hash table occupancy**, not “disk full.”  
2. Size the fingerprint table to pool Data used + host RAM (no swap → careful).  
3. Grow path: stop bees process only → rename old table → fresh empty file →
   set `BEES_HASH_SIZE` → restart → expect occupancy near 0 then climb.  
4. Never set format-allow flags for a resize.  
5. Grafana: bees up, occupancy ratio, btrfs used vs logical, cron heartbeats.

**Do not conflate** with Template B reclaimable-GB candidates.

**Historical note (public):** 2026-08-01 lab grew bees table **1 G → 2 G → 4 G**
on a ~1.5 TiB-used pool when occupancy hit ~100% / ~99%. See ai-gateway
`docs/ops/bees/` after public push.

---

## Template E — Domain map + staged catalog (Johnny write-through)

**Goal:** Stable catalog paths for humans and later Orchard/Postgres.

```text
work/catalog/johnny-chipper/
  structure/<date>/
  dedup/<date>/
  models-storyboard/<date>/
  quarantine-verify/<wave>/
  receipts/<date>/
```

Rules:

- Indexes and receipts only in git-facing or public docs; **not** model weights.  
- Each wave is append-only; never rewrite an old receipt in place.  
- Link C.H.A.I.N.S. receipt id when custody chain exists.

---

## Template F — Public handoff sanitize

Before any public mirror:

1. Strip `.env`, keys, tokens, cookies, session dumps.  
2. Replace LAN IPs and hostnames with **roles** (`desk-host`, `gpu-host`, `nas-host`).  
3. Redact person names / care identifiers outside deliberate public story.  
4. Keep **process truth** (incidents, ladders, dashboards as JSON templates).  
5. License + short README so outsiders know status (design vs shipped).

---

## Template G — Stage reclaim candidates (read-only)

**Goal:** Narrow a report-only L2 audit to what a human should actually
look at, without pretending the files moved.

1. Keep **`confidence=sha256` groups only**.  
2. Write a staged JSON under the catalog prefix.  
3. If `reclaimable_bytes` summed from groups disagrees with a summary
   field, **report both** and flag a gap over ~2%. Do not pick a winner.  
4. Do **not** emit a verification manifest.  
5. Do **not** call apply / purge from this step.

This sits between Template B (report) and Template C (dual-verify).
It is a reading aid.

---

## Mapping to modules (when code lands)

| Template | Module |
|----------|--------|
| A | Johnny Appleseed (classify + structure) |
| B–C | C.H.I.P.P.E.R.S. (hash / inspect / simplify reports) |
| D | Ops / storage plane (bees) — adjacent, not CHIPPER itself |
| E | Catalog + future Orchard SoR |
| F | Release engineering for all public *-release trees |
| G | Read-only sha256 filter of an L2 report (not an apply) |
