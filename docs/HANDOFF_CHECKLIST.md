# Handoff checklist (public + implementers)

## Before pushing a public mirror of related stacks

- [ ] No `.env`, key files, or credential dumps  
- [ ] LAN IPs / hostnames → roles (`desk-host`, `gpu-host`, `nas-host`)  
- [ ] Dashboards: Prometheus UIDs may be placeholders; document import steps  
- [ ] Incident write-ups keep **process truth**, drop private log absolute paths if sensitive  
- [ ] README states design-only vs shipped code  

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
