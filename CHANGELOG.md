# Changelog

All notable changes to **johnny-appleseed-chipper** are documented here.
Format inspired by [Keep a Changelog](https://keepachangelog.com/).

## [0.2.0] — 2026-08-16

### Added

- **GitHub Pages** one-pager (`docs/index.html`): orchard/crate theme sibling to
  [mok-tua](https://the1truedan.github.io/mok-tua/),
  [fast-models](https://the1truedan.github.io/fast-models/),
  [ai-gateway](https://the1truedan.github.io/ai-gateway/),
  [grok-tua-tok-tua](https://the1truedan.github.io/grok-tua-tok-tua/), and
  [cmip-terpene-db](https://the1truedan.github.io/cmip-terpene-db/).
  Live: [the1truedan.github.io/johnny-appleseed-chipper](https://the1truedan.github.io/johnny-appleseed-chipper/)
- `VERSION` file and this changelog.
- [docs/CHAINS_AUDIT_CONTRACT.md](docs/CHAINS_AUDIT_CONTRACT.md) — receipt
  shape and audit invariants. Still **tamper-evident bookkeeping**, not a
  court seal.

### Changed

Process templates and layer notes pick up checks from the parallel lab tool
(the private classify / hash / apply path). Public wording only:

- Reclaim layers stay **L1 structure / L2 content-hash / L3 bees**. The
  Johnny catalog is a different plane. Do not renumber one into the other.
- L2 skips callable venv trees by default. Darwin and Linux envs are not
  interchangeable.
- Apply / purge stays **sha256 + verification-manifest**
  (`verified` and `all_hosts_readable`). A staging filter is not an apply.
- Audit JSON field names must match the apply tool, or get reshaped first.
- Vendor shared libraries match `.so`, `.so.N`, and `.dylib` — not a
  literal `endswith(".so")`.
- Compression on the pool is a **mount property**, not a cron job.
- Tools live next to the job, not on the data pool.

Still **docs-first**. No extractors, no auto-delete, no crawl.

## [0.1.0] — 2026-08-12

First versioned public release: alpha-intent narrative, process templates,
AI-attribution `CONTRIBUTING.md`.
