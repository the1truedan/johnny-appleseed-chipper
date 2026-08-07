# Example ask-packet BOM (Johnny face)

Illustrative only — no weight blobs. Pattern for
`johnny-chipper/ask-bom/<date>/` when indexing what an ask_packet.v1
`manifest.requires[]` may reference.

## Roles → inventory refs (mok-tua stage / lora inventory)

| kind | ref | role |
|------|-----|------|
| unet | `qwen_edit_2509_fp8` | storyboard_base_edit |
| lora | `multi_angles` | storyboard_camera |
| lora | `next_scene` | storyboard_continuity |
| workflow | `wan22_animate` | i2v |
| workflow | `qwen_next_scene_angles` | still multi-angle |

## Join key

Nodes advertise residency of `tier_lock` slices, e.g.:

```text
T1_vid_gen@<blake2b-of-tier-slice>
```

See mok-tua `docs/ASK_PACKET_FEDERATION.md` and `schemas/ask_packet.v1.json`.

## Process templates

- Template E (staged catalog) — index digests, not blobs
- Template F (public sanitize) — no secrets, no PHI paths
