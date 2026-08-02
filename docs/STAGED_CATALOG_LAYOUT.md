# Staged catalog layout

Portable layout for indexes produced by process templates. Paths are **relative
to a work root** you choose (local disk, NAS `work/`, or object storage).

```text
johnny-chipper/
├── INDEX.md                         # this wave’s table of contents
├── structure/
│   └── YYYY-MM-DD/
│       ├── inventory.json
│       └── scorecard.md
├── dedup/
│   └── YYYY-MM-DD/
│       ├── audit.json               # report-only ladder results
│       └── notes.md
├── models-storyboard/
│   └── YYYY-MM-DD/
│       ├── lora_inventory.json      # paths + roles, not weight blobs
│       └── gaps.md
├── quarantine-verify/
│   └── <wave-id>/
│       ├── dual_verify.json
│       └── dual_verify.md
└── receipts/
    └── YYYY-MM-DD/
        └── chains_receipt.jsonl     # optional C.H.A.I.N.S. lines
```

## Multi-host law (abstract)

| Role | Walks | Writes indexes to |
|------|-------|-------------------|
| **nas-host** | Allowlisted tops on the AI pool | Primary catalog SoR |
| **gpu-host / desk-host** | Local disks / staging only | Same SoR via API or sync |
| **agents** | Never unbounded NFS `find` | Read catalogs only |

## What never goes in git

- `.safetensors` / checkpoints / raw caches  
- `.env` and API keys  
- Care / PHI material  
- Full recursive directory dumps of 10k+ LoRA sidecar trees  
