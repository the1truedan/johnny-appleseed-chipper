# Example catalog wave — models-storyboard 2026-08-02

Illustrative index only (no weight blobs). Pattern for
`johnny-chipper/models-storyboard/<date>/`.

## Present (lab pool snapshot)

- `loras/Qwen-Edit-2509-Multiple-angles.safetensors` — multi-angle camera
- `loras/next-scene_lora-v2-3000.safetensors` — next-scene continuity
- `loras/Qwen-Image-Edit-2509-Lightning-4steps-V1.0-bf16.safetensors` — lightning
- `animatediff/mm_sd_v15_v2.fp16.safetensors` — AD fallback
- Wan / LTX diffusion models under `diffusion_models/`

## Gaps

- Comfy **API-format** workflow exports for Qwen next-scene+angles and
  AnimateDiff basic (UI graphs alone are not enough for headless queue)

## Process templates used

- Template A (structure) — bounded probe of models tops
- Template E (staged catalog) — this folder shape
- Template F (public sanitize) — only indexes, no paths with secrets

See repo `docs/PROCESS_TEMPLATES.md`.
