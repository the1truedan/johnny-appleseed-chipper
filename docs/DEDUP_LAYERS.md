# Dedup layers — do not conflate

| Layer | What it measures | Typical tools | “Savings” meaning |
|-------|------------------|---------------|-------------------|
| **L3 bees** | Btrfs extent physical sharing | bees, `beesstats`, Grafana occupancy | Real pool bytes when extents merge |
| **L2 content-hash** | Same file bytes (sample → SHA256) | deepscan / dedup_engine / dual-verify | *Candidates* for human merge or quarantine |
| **L1 structure** | Dual install roots, husks, path clutter | structure inventory, prune manifests | Cleanup targets, not byte proof |

The **Johnny catalog** (classify → hash envelope → custody receipt) is a
different plane. It is a call graph and an index, not a fourth reclaim
layer. Do not renumber L1–L3 into it.

### Rules of thumb

1. A high **bees hash occupancy** is not “disk full.”  
2. A large **reclaimable GB** from content-hash audit is not bees savings.  
3. Quarantine waves may be L1 until dual-verify promotes paths to L2 proven.  
4. Execute deletes only with a verification manifest (Template B/C).  
5. Pool **compression** (e.g. zstd) is a mount / filesystem property, not a
   cron job and not a reclaim layer.  
6. Same-filesystem `mv` into quarantine does **not** free space until `rm`.
   bees may already share the extents.

### L2 checks the lab actually hit (2026-08)

- **Skip callable venv trees** (`.venv` / `venv`) by default. They are
  environments, not weight twins. Darwin and Linux envs are not
  interchangeable — never treat them as one reclaim group.
- **Ladder:** exact size → 4 MiB head/tail sample → full SHA-256 only on
  multi-member sample groups. Escalate; do not full-hash a whole pool as
  the default path.
- **Confidence gate:** `basename_size` and `sample` are not execute.
  `sha256` is. `dir_tree` is a structure candidate only.
- **Vendor shared libraries:** match `.so`, `.so.N` (`.so.12`), and
  `.dylib`. A literal `endswith(".so")` misses the CUDA / torch copies
  that actually matter.
- **Audit JSON shape:** the apply tool and a staged “reclaim candidates”
  file can look similar and still disagree on field names
  (`canonical` vs `canonical_keep`, `reclaimable` vs `reclaimable_files`,
  wrapper vs no wrapper). Reshape before apply. Do not guess.
- **Two reclaim totals:** summing `reclaimable_bytes` on sha256 groups
  can run a few percent higher than a summary field with the same name.
  Report both. Do not pick one silently.
- **Stage ≠ apply.** A sha256-only filter of the report is a reading
  aid. It does not write a verification manifest and it does not move
  files.

Public ops narrative for bees sizing lives in
[fast-models](https://the1truedan.github.io/fast-models/) and
[ai-gateway](https://the1truedan.github.io/ai-gateway/) after the
sanitized publish wave.
