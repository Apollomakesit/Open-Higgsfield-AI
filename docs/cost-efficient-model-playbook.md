# Personal Cost-Efficient Model Playbook

This guide is for a personal self-hosted Open Higgsfield-style UI that starts with the cheapest useful draft lanes, then promotes only stable prompts into premium final runs.

It separates three things that provider sites often mix together:

- direct per-image or per-second usage pricing
- monthly platform plans or bonus-credit bundles
- homepage marketing claims that are weaker than a real pricing table

Public pages were checked on 2026-04-10. Treat this as an architecture input, not a billing guarantee.

## Rules For Router Design

1. Base automatic routing on explicit per-image or per-second public prices.
2. Do not use monthly-plan pages as the cheapest-path baseline.
3. Do not compare undefined aggregator rows directly against official rows when resolution or audio is omitted.
4. Keep provider keys server-side if you replace Muapi. Do not carry the current browser BYOK pattern into a multi-provider router.
5. Re-check prices immediately before production launch.

## Strong Versus Weak Pricing Signals

### Strong enough to drive router defaults

- Google Vertex AI pricing
- fal model pricing pages
- Replicate pricing page
- WaveSpeed pricing page
- SiliconFlow pricing page

### Useful context, but not strong enough to set router defaults alone

- PiAPI pricing and homepage
- Segmind pricing
- MindStudio pricing
- BytePlus ModelArk pricing page
- APIYI homepage

## Verified Public Pricing Snapshot

### Image lanes

| Route | Posted public price | Architecture use |
| --- | --- | --- |
| Replicate `Flux Schnell` | `$0.003/image` | Cheapest repo-aligned image draft lane found in this pass |
| WaveSpeed `Flux Dev Ultra Fast` | `$0.005/image` | Very cheap draft lane when you want a Flux-family look instead of the absolute cheapest path |
| SiliconFlow `Z-Image-Turbo` | `$0.005/image` | Ultra-cheap image lane, but this is off-catalog relative to the current repo model list |
| Replicate `Flux Dev` | `$0.025/image` | Stable general image draft baseline |
| fal `Flux Dev` | `$0.025/megapixel` | Useful when you want resolution-linked cost math instead of a flat per-image number |
| SiliconFlow `FLUX.2 [Pro]` | `$0.03/image` | Cheapest explicit premium FLUX.2 row found publicly in this pass |
| Replicate `Flux 1.1 Pro` | `$0.04/image` | Alternative premium Flux-family final lane |
| WaveSpeed `Seedream V4.5` | `$0.04/image` | More polished social-style or realism-oriented image lane |
| Google Vertex AI `Gemini 3.1 Flash Image Preview` | `512: $0.045/image`, `1K: $0.067/image`, `2K: $0.101/image`, `4K: $0.15/image` | Useful as a direct Google image baseline, but do not assume it is the same SKU as the repo's `Nano Banana` labels |
| WaveSpeed `FLUX.2 [Pro]` | `$0.055/image` | Second public FLUX.2 premium reference point |
| WaveSpeed `Midjourney` | `$0.08/image` | Style-first final lane, not a good draft default |
| Replicate `Ideogram v3 Quality` | `$0.09/image` | Best public text-heavy image lane found in this pass |
| WaveSpeed `Nano Banana Pro` | `$0.14/image` | Premium photoreal or polished final lane |

SiliconFlow currently posts `FLUX.2 [pro]` cheaper than `FLUX.2 [flex]` on its public pricing page. Use the posted number as a signal, but verify billing behavior during implementation because the relative ordering is counterintuitive.

### Video lanes

| Route | Posted public price | Architecture use |
| --- | --- | --- |
| WaveSpeed `Hailuo Minimax` | `$0.01/second` | Cheapest public cloud video draft lane found in this pass |
| Google Vertex AI `Veo 3.1 Lite` video-only | `720p: $0.03/second`, `1080p: $0.05/second` | Cheapest official Google Veo lane found publicly |
| Google Vertex AI `Veo 3.1 Lite` video plus audio | `720p: $0.05/second`, `1080p: $0.08/second` | Cheapest official Google Veo lane with synchronized audio |
| WaveSpeed `Wan 2.6` | `$0.08/second` | Low-cost general draft lane if you want an open-model-adjacent workflow |
| Google Vertex AI `Veo 3.1 Fast` video-only | `720p: $0.08/second`, `1080p: $0.10/second`, `4K: $0.25/second` | Best explicit direct value lane for high-quality final video |
| Replicate `Wan 2.1 I2V` | `480p: $0.09/second`, `720p: $0.25/second` | Transparent open-model image-to-video baseline |
| WaveSpeed `Seedance 2.0` | `$0.10/second` | Strong camera-aware or motion-directed lane |
| Google Vertex AI `Veo 3.1 Fast` video plus audio | `720p: $0.10/second`, `1080p: $0.12/second`, `4K: $0.30/second` | Explicit direct price for higher-fidelity final clips with audio |
| WaveSpeed `Veo 3.1` | `$0.12/second` | Useful benchmark, but the summary table does not spell out resolution or audio |
| WaveSpeed `Kling O3` | `$0.15/second` | Human-motion benchmark lane |
| Google Vertex AI `Veo 3.1` video-only | `720p or 1080p: $0.20/second`, `4K: $0.40/second` | Premium direct final lane |
| fal `Kling 2.1 Master` image-to-video | `5 seconds: $1.40`, `each additional second: $0.28` | Transparent Kling row, but not a cheap path |
| Google Vertex AI `Veo 3.1` video plus audio | `720p or 1080p: $0.40/second`, `4K: $0.60/second` | Premium direct final lane with audio |
| fal `Veo 3 Fast` | `$0.25/second` audio off, `$0.40/second` audio on | Transparent row, but more expensive than the direct Google public rates found in this pass |
| fal `Veo 3` standard | `$0.50/second` audio off, `$0.75/second` audio on | High-cost premium lane, not a default budget route |

WaveSpeed's summary page gives excellent routing signals, but some rows omit resolution and audio detail. Use those rows as benchmark prices, not as perfect apples-to-apples replacements for Vertex AI's explicit Veo tiers.

## Pages Reviewed But Excluded From Core Cost Routing

| Provider page | Public signal found | Architecture takeaway |
| --- | --- | --- |
| PiAPI pricing | Monthly plans such as `Free $0/month`, `Creator $15/month`, `Pro $60/month`, `Enterprise $100/month`, with bonus-credit framing | This is a platform-plan layer, not a clean cheapest-per-model signal |
| PiAPI homepage | Large model catalog plus a visible `Midjourney API Service Unavailable` notice | Useful as a coverage signal, not a reliable per-model pricing source |
| Segmind pricing | Subscription-style plans and GPU-hour style pricing | Better treated as managed infrastructure or credits, not as a simple personal router default |
| MindStudio pricing | `Free $0/month + usage` and `Individual $20/month + usage` platform framing | Adds platform overhead on top of usage, which is usually the wrong fit if you already own the UI |
| BytePlus ModelArk pricing | Strong free-quota messaging such as `up to 2M free tokens per model`, `Seedance 1.5 Pro free 2M tokens`, and `Seedream 5.0 lite 200 free 4K images` | Good to revisit for promos or free quotas, but I did not get a clean public per-image or per-second pay-as-you-go row from the reviewed page |
| APIYI homepage | Homepage claims such as `Nano Banana Pro API $0.05/image` and `Sora 2 API $0.12/video` | Too weak to set router defaults without a deeper pricing doc or billing confirmation |

## Routing Recommendations

### Image

- Cheapest repo-aligned draft: Replicate `Flux Schnell` at `$0.003/image`.
- Better-looking default draft: WaveSpeed `Flux Dev Ultra Fast` at `$0.005/image` or Replicate `Flux Dev` at `$0.025/image`.
- Text-heavy route: Replicate `Ideogram v3 Quality` at `$0.09/image`.
- Best value premium final: `FLUX.2 [Pro]`. SiliconFlow posts `$0.03/image`; WaveSpeed posts `$0.055/image`.
- Premium photoreal final: WaveSpeed `Nano Banana Pro` at `$0.14/image`.
- Google image note: Vertex AI publishes `Gemini 3.1 Flash Image Preview` output pricing, but I am not mapping that directly to `Nano Banana` without an explicit provider SKU mapping.

### Video

- Absolute cheapest cloud draft: WaveSpeed `Hailuo Minimax` at `$0.01/second`.
- Cheapest official Google draft: `Veo 3.1 Lite` direct at `$0.03/second` for 720p video-only or `$0.05/second` for 1080p video-only.
- Best open-model-aligned draft: WaveSpeed `Wan 2.6` at `$0.08/second` or Replicate `Wan 2.1 I2V` 480p at `$0.09/second`.
- Best final value direct: `Veo 3.1 Fast` direct at `$0.08/second` for 720p video-only or `$0.10/second` for 1080p video-only.
- Best premium direct: `Veo 3.1` direct at `$0.20/second` for 720p or 1080p video-only.
- Best camera-aware final: WaveSpeed `Seedance 2.0` at `$0.10/second`.
- Human-motion benchmark: WaveSpeed `Kling O3` at `$0.15/second`. fal `Kling 2.1 Master` is transparent, but much more expensive at `$1.40` for the first 5 seconds and `$0.28` for each additional second.

## Default Personal Presets For A Self-Hosted UI

### Image presets

| UI mode | Default route | Why |
| --- | --- | --- |
| `Image Draft Cheapest` | Replicate `Flux Schnell` | Cheapest repo-aligned image draft lane found in this pass |
| `Image Draft Default` | WaveSpeed `Flux Dev Ultra Fast` | Very low cost with a better default look than the absolute cheapest lane |
| `Image Draft With Text` | Replicate `Ideogram v3 Quality` | Saves retries when text fidelity matters |
| `Image Final Default` | `FLUX.2 [Pro]` via SiliconFlow first | Cheapest posted premium FLUX.2 row found publicly |
| `Image Final Photoreal` | WaveSpeed `Nano Banana Pro` | Better final polish when realism matters |
| `Image Final Style` | WaveSpeed `Midjourney` | Use only when you specifically want that look |

### Video presets

| UI mode | Default route | Why |
| --- | --- | --- |
| `Video Draft Cheapest` | WaveSpeed `Hailuo Minimax` | Lowest public cloud draft cost found in this pass |
| `Video Draft Official` | Google Vertex AI `Veo 3.1 Lite` 720p | Cheapest official Veo draft lane found publicly |
| `Video Draft Open Model` | WaveSpeed `Wan 2.6` | Low-cost general draft lane |
| `Video Final Value` | Google Vertex AI `Veo 3.1 Fast` 720p or 1080p | Best explicit direct cost-quality sweet spot |
| `Video Final Premium` | Google Vertex AI `Veo 3.1` | Best premium direct lane with clean public pricing |
| `Video Final Camera` | WaveSpeed `Seedance 2.0` | Strong camera-directed motion option |

## Repo Model ID Mapping And Direct-Provider Status

This section maps the current registry IDs that matter to the draft and final lanes in this playbook. It does not try to classify tool-only IDs, the single V2V utility, or the lip-sync models.

Every ID below still routes through Muapi today. Status means what your replacement router still needs.

### Lanes With Matching Repo IDs

- `image_draft_cheapest`: `flux-schnell`. Target: Replicate `Flux Schnell`. Status: needs a Replicate adapter.
- `image_draft_default`: `flux-dev`, `flux-dev-lora`. Target: Replicate `Flux Dev`, WaveSpeed `Flux Dev Ultra Fast`, or fal `Flux Dev`. Status: needs a direct adapter; `flux-dev-lora` also needs a LoRA-capable request path, so it is not a drop-in replacement for the base model.
- `image_draft_polished`: `bytedance-seedream-v4.5`, `bytedance-seedream-v4.5-edit`, `seedream-5.0`, `seedream-5.0-edit`. Target: WaveSpeed `Seedream V4.5`. Status: needs a WaveSpeed adapter; `seedream-5.0` and `seedream-5.0-edit` still need SKU verification because the verified public price row was for V4.5, not 5.0.
- `image_draft_text`: `ideogram-v3-t2i`, `ideogram-v3-reframe`, `ideogram-character`. Target: Replicate `Ideogram v3 Quality`. Status: needs a Replicate adapter.
- `image_final_default`: `flux-2-pro`, `flux-2-pro-edit`. Target: SiliconFlow `FLUX.2 [Pro]` first, WaveSpeed fallback. Status: needs a SiliconFlow adapter; `flux-2-pro-edit` also needs edit-mode request mapping.
- `image_final_photoreal`: `nano-banana-pro`, `nano-banana-pro-edit`. Target: WaveSpeed `Nano Banana Pro`. Status: needs provider-SKU confirmation plus an adapter; `nano-banana-pro-edit` also needs an edit-path mapping.
- `image_final_style`: `midjourney-v7-text-to-image`, `midjourney-v7-image-to-image`, `midjourney-v7-style-reference`, `midjourney-v7-omni-reference`. Target: WaveSpeed `Midjourney`. Status: needs a WaveSpeed adapter and per-mode parameter mapping.
- `video_draft_cheapest`: `minimax-hailuo-02-standard-t2v`, `minimax-hailuo-02-pro-t2v`, `minimax-hailuo-2.3-standard-t2v`, `minimax-hailuo-2.3-pro-t2v`, `minimax-hailuo-02-standard-i2v`, `minimax-hailuo-02-pro-i2v`, `minimax-hailuo-2.3-standard-i2v`, `minimax-hailuo-2.3-pro-i2v`, `minimax-hailuo-2.3-fast`. Target: WaveSpeed `Hailuo Minimax`. Status: needs a WaveSpeed adapter; the current Hailuo variants still need one-to-one provider SKU mapping.
- `video_draft_open`: `wan2.1-text-to-video`, `wan2.2-text-to-video`, `wan2.2-5b-fast-t2v`, `wan2.5-text-to-video`, `wan2.5-text-to-video-fast`, `wan2.6-text-to-video`, `wan2.1-image-to-video`, `wan2.2-image-to-video`, `wan2.2-spicy-image-to-video`, `wan2.5-image-to-video`, `wan2.5-image-to-video-fast`, `wan2.6-image-to-video`, `wan2.1-reference-video`. Target: WaveSpeed `Wan 2.6` for the cheap general lane and Replicate `Wan 2.1 I2V` as the transparent baseline. Status: needs a Wan family adapter and per-variant routing rules.
- `video_final_value`: `veo3.1-fast-text-to-video`, `veo3.1-fast-image-to-video`. Target: Google Vertex AI `Veo 3.1 Fast`. Status: needs a Vertex adapter.
- `video_final_pro`: `veo3.1-text-to-video`, `veo3.1-image-to-video`, `veo3.1-reference-to-video`. Target: Google Vertex AI `Veo 3.1`. Status: needs a Vertex adapter and reference-video request mapping.
- `video_final_camera`: `seedance-lite-t2v`, `seedance-pro-t2v`, `seedance-pro-t2v-fast`, `seedance-v1.5-pro-t2v`, `seedance-v1.5-pro-t2v-fast`, `seedance-v2.0-t2v`, `seedance-v2.0-extend`, `seedance-lite-i2v`, `seedance-pro-i2v`, `seedance-pro-i2v-fast`, `seedance-lite-reference-video`, `seedance-v1.5-pro-i2v`, `seedance-v1.5-pro-i2v-fast`, `seedance-v2.0-i2v`. Target: WaveSpeed `Seedance 2.0`. Status: needs a WaveSpeed adapter; older Seedance IDs need explicit SKU mapping or they should remain separate premium lanes.
- `video_human_motion_benchmark`: `kling-v2.1-master-t2v`, `kling-v2.5-turbo-pro-t2v`, `kling-v2.6-pro-t2v`, `kling-o1-text-to-video`, `kling-v3.0-pro-text-to-video`, `kling-v3.0-standard-text-to-video`, `kling-v2.1-master-i2v`, `kling-v2.1-standard-i2v`, `kling-v2.1-pro-i2v`, `kling-v2.5-turbo-pro-i2v`, `kling-v2.5-turbo-std-i2v`, `kling-o1-image-to-video`, `kling-o1-reference-to-video`, `kling-o1-standard-image-to-video`, `kling-o1-standard-reference-to-video`, `kling-v2.6-pro-i2v`, `kling-v3.0-pro-image-to-video`, `kling-v3.0-standard-image-to-video`. Target: WaveSpeed `Kling O3` or fal `Kling 2.1 Master` only as a benchmark family. Status: needs explicit provider-SKU mapping before direct routing.

### Recommended Lanes Missing A Current Repo ID

- `video_draft_official`: there is no `veo3.1-lite-text-to-video` or `veo3.1-lite-image-to-video` ID in the current registry. Status: needs new registry IDs in both model files plus a Vertex Lite adapter if you want the cheapest official Google lane.

### Current Repo IDs That Still Need Provider Research Before They Belong In A Lane

- `nano-banana`, `nano-banana-edit`, `nano-banana-effects`, `nano-banana-2`, `nano-banana-2-edit`: clearly related to the premium photoreal family, but I do not yet have a clean verified public provider SKU mapping for these non-Pro variants.
- `flux-kontext-dev-t2i`, `flux-kontext-pro-t2i`, `flux-kontext-max-t2i`, `flux-kontext-dev-i2i`, `flux-kontext-pro-i2i`, `flux-kontext-max-i2i`, `flux-kontext-effects`: provider pricing and request-shape mapping still need verification.
- `flux-2-dev`, `flux-2-flex`, `flux-2-dev-edit`, `flux-2-flex-edit`, `flux-2-klein-4b`, `flux-2-klein-4b-edit`, `flux-2-klein-9b`, `flux-2-klein-9b-edit`, `z-image-turbo`, `z-image-base`: these can become extra cheap or mid-tier image lanes, but the current playbook only selected `flux-schnell`, `flux-dev`, and `flux-2-pro` as the v1 defaults.
- `runway-text-to-video`, `runway-image-to-video`, `runway-act-two-i2v`: the playbook does not use Runway as a cost-first default, and I do not yet have a clean public router price for these specific SKUs.
- `openai-sora`, `openai-sora-2-text-to-video`, `openai-sora-2-pro-text-to-video`, `openai-sora-2-image-to-video`, `openai-sora-2-pro-image-to-video`: present in the registry, but still outside the verified cost-first default lanes.
- `veo3-text-to-video`, `veo3-fast-text-to-video`, `veo3-image-to-video`, `veo3-fast-image-to-video`: direct-provider targets exist, but the playbook currently prefers the `veo3.1*` lanes. Integrate these only if you want explicit legacy Google tiers as separate options.
- `kling-o1-text-to-image`, `kling-o1-edit-image`, `midjourney-v7-image-to-video`: these IDs exist in the registry, but the verified pricing pass did not produce a clean direct-provider route for these exact modes.
- `google-imagen4`, `google-imagen4-fast`, `google-imagen4-ultra`, `gpt-image-1.5`, `gpt-image-1.5-edit`, `gpt4o-text-to-image`, `gpt4o-image-to-image`, `gpt4o-edit`: these image families are outside the current cost-first routing set and still need a separate direct-provider decision.

## Models To Avoid While The Prompt Is Still Unclear

- `Nano Banana Pro`
- `Midjourney`
- `Veo 3.1`
- `Sora 2 Pro`
- `Runway Gen-3`
- premium edit models such as `Flux Kontext Max` and `Nano Banana Pro Edit`

## Minimal Routing Policy For Open Higgsfield

If you replace Muapi with your own backend router, this is the smallest high-value policy to implement first.

```text
image_draft_cheapest -> replicate_flux_schnell
image_draft_default -> wavespeed_flux_dev_ultra_fast
image_draft_text -> replicate_ideogram_v3
image_final_default -> flux_2_pro_provider
image_final_photoreal -> wavespeed_nano_banana_pro
video_draft_cheapest -> wavespeed_hailuo
video_draft_official -> vertex_veo_3_1_lite
video_draft_open -> wavespeed_wan_2_6
video_final_value -> vertex_veo_3_1_fast
video_final_pro -> vertex_veo_3_1
video_final_camera -> wavespeed_seedance_2_0
```

## Best Practical Recommendation

If you want the cleanest v1 architecture with strong cost control:

1. Keep Open Higgsfield as the UI base.
2. Replace Muapi with a server-side router.
3. Use Replicate, WaveSpeed, and SiliconFlow for image lanes.
4. Use Google Vertex AI direct for Veo lanes.
5. Ignore plan-based wrappers in v1 unless you explicitly want managed aggregation more than lowest direct cost.

## Sources

- [Google Vertex AI pricing](https://cloud.google.com/vertex-ai/generative-ai/pricing)
- [fal pricing](https://fal.ai/pricing)
- [fal Flux Dev](https://fal.ai/models/fal-ai/flux/dev)
- [fal Kling image-to-video](https://fal.ai/models/fal-ai/kling-video/v2.1/master/image-to-video)
- [fal Veo 3](https://fal.ai/models/fal-ai/veo3)
- [Replicate pricing](https://replicate.com/pricing)
- [PiAPI pricing](https://piapi.ai/pricing)
- [PiAPI homepage](https://piapi.ai/)
- [Segmind pricing](https://www.segmind.com/pricing)
- [WaveSpeed pricing](https://wavespeed.ai/pricing)
- [SiliconFlow pricing](https://www.siliconflow.com/pricing)
- [MindStudio pricing](https://www.mindstudio.ai/pricing)
- [BytePlus ModelArk pricing](https://www.byteplus.com/en/product/modelark/pricing)
- [APIYI homepage](https://apiyi.com/)
