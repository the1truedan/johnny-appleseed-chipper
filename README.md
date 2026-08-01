# Johnny Appleseed + C.H.I.P.P.E.R.S. + C.H.A.I.N.S.

**Status:** design notes only. No product code ships here yet — on purpose.

## What this is about

How do you take files you **already have** (scans, exports, archives — not
scraped off the open web) and turn them into something searchable, de-duplicated,
and honest about where each copy came from?

This repo sketches three cooperating layers:

- **Johnny Appleseed** — figure out *what kind of thing* a file is before
  you pour energy into parsing it.
- **C.H.I.P.P.E.R.S.** — hash, inspect, parse when safe, and emit stable
  artifacts + index records. Unknown binaries get metadata only — we do not
  “open everything and hope.”
- **C.H.A.I.N.S.** — a simple, hash-linked receipt log so you can see *what
  was ingested when*. That is **tamper-evident bookkeeping**, not a court
  seal, blockchain, or identity proof. Please do not market it as more than
  it is.

## Design principles

- No crawling or scraping — only materials the operator already holds.
- Same inputs should produce the same digests and derived tiers.
- A human still decides merges, license calls, and anything high-stakes.

## Module seam (interface sketch)

```python
batch = IngestionEngine().ingest(materials)
verified = IngestionEngine().verify(batch)
```

Callers acquire bytes and choose scope/consent; the engine returns
artifacts, portable index documents, and optional custody receipts — no
direct network, filesystem, or database writes happen inside the module
itself. Future search stores and extractors are meant to consume those
returned values through adapters, rather than reaching into this seam.

## Rendition contract

```
00_original → 10_assured → 20_extracted → 30_cleaned → 40_simplified →
45_markdown → 50_accessible → 60_index → 70_exports
```

Every tier records its parent digest and transformation. Inapplicable
tiers stay absent but are reported in the manifest. Originals are never
overwritten.

## Roadmap (design only — none of this is built yet)

1. Threat-model untrusted input: decompression bombs, parser exploits,
   macros, polyglots, path traversal, MIME spoofing, oversized inputs,
   prompt injection. Extractors run sandboxed with resource limits.
2. A vetted format-adapter matrix with fixtures and provenance — HTML,
   Markdown, PDF, DOCX, PPTX, XLSX, CSV/TSV, JSON/XML, email/MBOX/EML,
   images/OCR, audio/video/transcripts, archives, source code. "All file
   types" means an explicit safe fallback, never blind parsing of
   whatever shows up.
3. Acquisition adapters for allowlisted sources only, preserving response
   headers, retrieval time, canonical URL, redirects, raw bytes, and
   licensing decisions.
4. Content-defined chunking, document structure/citation extraction,
   deduplication, language detection, embedding/version metadata,
   retention and deletion workflows.
5. Custody-chain integration: append-only durable storage, trusted UTC,
   actor/workload identity, consent snapshots, signed manifests, key
   rotation, verification reports, recovery tests.
6. Independent security/privacy review, performance limits, golden-corpus
   tests, fuzzing, corrupted-chain tests, documented operator runbooks.

## Acceptance criteria for a real implementation

- Every accepted source has a raw-content hash, acquisition provenance,
  declared scope, parser/version record, derived-output hashes, and
  custody linkage.
- Unsupported or suspicious formats quarantine safely — no content
  execution, ever.
- Repeated ingestion is idempotent; deletion follows retention/consent
  rules.
- A verifier can reproduce every chain hash from exported canonical
  records.
- Never described as immutable, forensic, comprehensive, or legally
  compliant without independent evidence — the chain is tamper-evident,
  full stop, not a certification.

## Future extension ideas (further sketches, further from being built)

### A git-object-model-based distributed content store

Git's SHA-keyed blob/tree/commit model is already a content-addressed
Merkle DAG — an interesting substrate to explore against a DHT-style
distribution layer (content-hash keys for immutable objects, signed keys
for mutable branch-tip pointers). The hard part: BitTorrent-style
swarming works because chunks are fungible, while git merge/conflict
detection is content-*aware*, not availability-based — cryptographic
hashes can't be "fuzzed toward similarity," so a similarity/fuzzy-hash
prefilter (ssdeep/TLSH, MinHash+LSH) would need to run *before* anything
reaches a real diff.

Sketch of connector responsibilities:

- object-graph translation (git's own hashing is already free
  content-addressing, so this layer stays thin)
- mutable ref-tip tracking that stores every competing claim rather than
  picking a winner
- a relationship taxonomy: deterministic graph relationships
  (ancestor-of / diverges-from / conflicts-with / supersedes) versus one
  genuinely fuzzy relationship (semantic-equivalence) that needs an
  embedding pass
- dependency-manifest scanning feeding a generic SBOM/vulnerability
  pipeline
- any merge-conflict resolution is always *proposed*, never
  auto-committed — a human decides every time

Further sketched, held to the same "never auto-trust" tier as any
external evidence:

- hash-based OSINT (what else references this hash) landing in a clearly
  low-confidence table, never the verified graph
- a full local dev-environment inventory + portable migration-bundle
  export, with a hard exclusion flag for credentials/private weights
  before anything reaches an exportable path
- a dependency-driven discovery feed (using manifest data as search
  keywords for related repos/packages)
- a reputation-weighted (not simple-vote) bad-actor/threat-intel
  registry, always routed to human review, never auto-purging anything

### Licensed-source acquisition + leak-detection lane (concept only)

A fourth acquisition lane, scoped narrowly to openly-licensed model
weights/datasets distributed via magnet link for bandwidth reasons, from
named/allowlisted sources only — piece-hash-verified transport, re-chunked
to match the store's own content-defined chunking, with a required
machine-checked license field before anything leaves quarantine.

An adjacent but *explicitly separate* concept: a metadata-only
leak-detection tool for rights holders — match title/hash/fingerprint
against a vendor-supplied known-title list, log swarm metadata, report
matches — that never downloads or redistributes the matched content,
since detection doesn't require possession. Same commercial category as
existing anti-piracy detection vendors.

**Explicit boundaries for this lane, non-negotiable if ever built:**

- No acquisition of anything whose IP status is unclear or unlicensed.
- No general scraping/crawling for content discovery — allowlisted or
  vendor-supplied lists only, nothing crawls the open landscape looking
  for material.
- The leak-detection half stays metadata-only, permanently.
- Explicitly excludes ever pursuing disputed/litigated proprietary
  training corpora — that would be the opposite of the stated mission,
  not an extension of it.

## License

MIT — see `LICENSE`.

## Non-affiliation

This is an independent architecture sketch. It is not affiliated with,
endorsed by, or sponsored by any company, product, or service referenced
generically above.
