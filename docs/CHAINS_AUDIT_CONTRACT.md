# C.H.A.I.N.S. audit contract

**C.H.A.I.N.S.** means **Canonical Hashes, Acquisition, Integrity, Narrative,
Scope**. It is a plain, verifiable custody vocabulary—not a blockchain, court
seal, identity proof, or legal-compliance certification.

## Receipt shape

Each accepted transition should emit one canonical JSON receipt containing:

```json
{
  "receipt_version": "0.1",
  "event_id": "uuid-or-equivalent",
  "occurred_at_utc": "trusted UTC timestamp",
  "actor": {"kind": "human|service|worker", "id": "declared id"},
  "scope": {"source": "declared source", "consent_class": "local-private"},
  "input": {"sha256": "...", "size": 0, "media_type": "..."},
  "operation": {"name": "ingest|verify|derive|quarantine|export|delete", "version": "..."},
  "output": [{"sha256": "...", "tier": "10_assured"}],
  "parent_receipt": "previous receipt hash or null",
  "decision": {"status": "accepted|quarantined|rejected|deleted", "reason": "..."}
}
```

Canonical serialization, hash algorithm, and timestamp rules must be specified
before the first implementation. A receipt is useful only if an independent
verifier can reproduce its hash from the exported canonical bytes.

## Audit invariants

- Originals are immutable by policy; derived outputs name their parent digest.
- No receipt is silently edited; corrections append a new event.
- A missing, duplicated, or malformed parent link fails verification loudly.
- Quarantine and rejection are first-class outcomes, not missing data.
- Consent/scope is carried forward and may only narrow, never silently widen.
- Export manifests exclude credentials, PHI, private paths, and unapproved
  personal assets by default.
- Deletion records the decision and receipt linkage without retaining deleted
  content.
- “Verified” means cryptographically and structurally reproducible, not true,
  safe, licensed, or legally admissible.

## M.A.N.A.G.E.R. transparency modules

The future implementation should expose these read-only audit views:

1. **Intake ledger:** what entered and under what declared scope.
2. **Lineage graph:** original → rendition → export relationships.
3. **Decision log:** accept, quarantine, reject, merge-review, retain, delete.
4. **Integrity verifier:** reproducible chain checks with explicit failures.
5. **Disclosure report:** what can leave the private boundary and why.
6. **Recovery report:** whether the ledger and artifacts can be restored.

These views are transparency aids for M.A.N.A.G.E.R.; they are not substitutes
for human review, consent, security controls, or legal counsel.

