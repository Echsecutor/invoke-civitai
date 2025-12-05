# InvokeAI Image Metadata

## PNG Metadata Location
InvokeAI stores metadata in PNG files under the key `invokeai_metadata` (accessible via `PIL.Image.info['invokeai_metadata']`).

## Metadata Structure (JSON)
Key fields:
- `positive_prompt` - Main prompt
- `negative_prompt` - Negative prompt (optional)
- `steps` - Number of inference steps
- `scheduler` - Sampler/scheduler name (see enum below)
- `cfg_scale` - Classifier-free guidance scale (optional)
- `seed` - Random seed
- `width`, `height` - Image dimensions
- `model` - Object with `name` and `hash` (format: `sha256:...`)
- `vae` - Optional, same structure as model
- `loras` - Optional array of `{model: {name, hash}, weight}`
- `_canvas_objects` - Present for inpainted images, contains `imageName` of original

## Scheduler/Sampler Enum

**Source:** https://github.com/invoke-ai/InvokeAI/blob/main/invokeai/backend/stable_diffusion/schedulers/schedulers.py

### How to Find Updated Schedulers
1. Fetch raw file: `curl -s "https://raw.githubusercontent.com/invoke-ai/InvokeAI/main/invokeai/backend/stable_diffusion/schedulers/schedulers.py"`
2. Look for `SCHEDULER_NAME_VALUES` Literal type definition
3. Or check `SCHEDULER_MAP` dictionary keys

### Complete Scheduler List (as of Dec 2024)
Non-Karras: `ddim`, `ddpm`, `deis`, `lms`, `pndm`, `heun`, `euler`, `euler_a`, `kdpm_2`, `kdpm_2_a`, `dpmpp_2s`, `dpmpp_2m`, `dpmpp_2m_sde`, `dpmpp_3m`, `dpmpp_sde`, `unipc`, `lcm`, `tcd`

Karras variants (append `_k`): `deis_k`, `lms_k`, `heun_k`, `euler_k`, `kdpm_2_k`, `kdpm_2_a_k`, `dpmpp_2s_k`, `dpmpp_2m_k`, `dpmpp_2m_sde_k`, `dpmpp_3m_k`, `dpmpp_sde_k`, `unipc_k`

### Mapping to A1111 Names
The `sampler_info` dict in `converter.py` maps InvokeAI scheduler names to A1111 format:
- `name`: A1111 sampler name (e.g., "DPM++ 2M SDE")
- `type`: Schedule type ("Automatic" or "Karras")

## InvokeAI API
- Swagger UI: `{invoke_url}/docs`
- OpenAPI spec: `{invoke_url}/openapi.json`



