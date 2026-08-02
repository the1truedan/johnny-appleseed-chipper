# Dedup layers — do not conflate

| Layer | What it measures | Typical tools | “Savings” meaning |
|-------|------------------|---------------|-------------------|
| **L3 bees** | Btrfs extent physical sharing | bees, `beesstats`, Grafana occupancy | Real pool bytes when extents merge |
| **L2 content-hash** | Same file bytes (sample → SHA256) | deepscan / dedup_engine / dual-verify | *Candidates* for human merge or quarantine |
| **L1 structure** | Dual install roots, husks, path clutter | structure inventory, prune manifests | Cleanup targets, not byte proof |

### Rules of thumb

1. A high **bees hash occupancy** is not “disk full.”  
2. A large **reclaimable GB** from content-hash audit is not bees savings.  
3. Quarantine waves may be L1 until dual-verify promotes paths to L2 proven.  
4. Execute deletes only with a verification manifest (Template B/C).

Public ops narrative for bees sizing lives in **ai-gateway** `docs/ops/bees/`
after the sanitized publish wave.
