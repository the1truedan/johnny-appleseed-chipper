# Handoff checklist (public + implementers)

## Before pushing a public mirror of related stacks

- [ ] No `.env`, key files, or credential dumps  
- [x] LAN IPs / hostnames → roles (`desk-host`, `gpu-host`, `nas-host`) — recheck 2026-08-06  
- [x] README states **public alpha intent** (docs-first; not finished product)  
- [x] Late July modular sketch narrative cleaned (status waves + socratic ask)  
- [x] Package manager catalog + hero art; no private LAN download URLs in tree  
- [ ] Dashboards: Prometheus UIDs may be placeholders; document import steps in ops stacks  
- [ ] Incident write-ups keep **process truth**, drop private log absolute paths if sensitive  
- [ ] Human: `gh repo edit … --visibility public` then protect `main` (free plan after public)

## Before implementing Johnny / CHIPPER for real

- [ ] Threat model for untrusted files (bombs, macros, polyglots, path traversal)  
- [ ] Format adapter matrix with fixtures  
- [ ] Sandboxed extractors with resource limits  
- [ ] Catalog layout (see STAGED_CATALOG_LAYOUT) wired to a single SoR  
- [ ] C.H.A.I.N.S. receipts are tamper-evident only — no overclaim in marketing  

## After a dedup or bees ops wave

- [ ] Catalog indexes filed under `johnny-chipper/…`  
- [ ] Verification manifest attached if any execute path ran  
- [ ] Grafana / metrics still healthy (bees up, occupancy not stuck at 1.0)  
- [ ] Public process templates updated if a new pattern was learned  
