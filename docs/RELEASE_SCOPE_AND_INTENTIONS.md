# Release scope and intentions

**Release candidate:** 2026-08-04 · design contract and audit vocabulary

Johnny Appleseed is intentionally a design-first repository. This release
does not claim that the ingestion engine, extractors, or custody store are
production implementations. It makes the intended boundaries testable before
code is added.

## Mission

Turn operator-held material into reproducible, reviewable artifacts without
silently expanding scope, trusting untrusted input, or losing the relationship
between an original and its derived forms.

## Development intentions

| Area | Intention | Evidence required before implementation is called ready |
|---|---|---|
| Scope | Accept only declared, operator-authorized inputs | Input manifest with scope and consent class |
| Classification | Identify format and risk before parsing | Deterministic classifier report and quarantine path |
| Derivation | Preserve originals and parent-child lineage | Per-tier hashes and transformation metadata |
| Deduplication | Detect exact identity without auto-merging meaning | Hash report; human merge decision |
| Custody | Make every state transition inspectable | C.H.A.I.N.S. receipt and verifier output |
| Privacy | Keep PHI, credentials, and private paths out of exports | Secret/PHI scan plus export manifest |
| Recovery | Reconstruct the ledger from portable records | Corrupted-chain and restore test |

## Full development trajectory

1. **Contract:** canonical records, status vocabulary, and fixture corpus.
2. **Safe intake:** bounded workers, parser allowlist, quarantine, and resource
   limits for hostile files.
3. **Deterministic transforms:** rendition tiers, content hashes, tool/version
   records, and idempotent reruns.
4. **C.H.A.I.N.S. ledger:** append-only receipts, verification, key rotation,
   export/import, and recovery drills.
5. **Adapters:** local folders and explicitly allowlisted acquisition sources;
   no general crawler or implicit network access.
6. **Audit surfaces:** human-readable reports, machine-readable manifests,
   redaction proofs, retention/deletion records, and M.A.N.A.G.E.R. handoffs.
7. **Scale and review:** golden corpus, fuzzing, performance budgets,
   independent security/privacy review, and only then a production claim.

## M.A.N.A.G.E.R. relationship

Johnny/CHIPPER is the custody-and-ingest section of the wider M.A.N.A.G.E.R.
system. It supplies evidence-bearing artifacts to downstream search,
orchestration, and caregiving modules; it does not decide care, merge records,
or publish material. Every downstream consumer must be able to answer:

- what was received;
- under whose declared scope and consent;
- which transformations ran;
- which bytes and records were produced;
- what was reviewed, rejected, redacted, retained, or deleted.

