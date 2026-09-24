# KESPA-NOTE-028 — Local-First Image Generation Bridge and Encrypted Retention Architecture

## Summary

KESPA established an operational local-first image-generation path using the existing RTX 3070 image worker. A normal chat request can be identified as image generation, routed by the WWW application through the configured provider layer, sent to an authenticated OpenAI-compatible image bridge, executed by localhost-only ComfyUI, returned to KESPA, retained as an encrypted private artifact, and displayed in the originating conversation.

This note records the working integration checkpoint rather than an image-quality benchmark. The initial SDXL Base 1.0 configuration was selected as a known-good functional baseline; higher-quality checkpoint comparison remains future work.

## Architecture

The verified request path is:

```text
KESPA chat
→ image-generation intent / capability route
→ KESPA WWW provider layer
→ authenticated OpenAI-compatible image bridge
→ Cloudflare-protected public bridge endpoint
→ localhost-only ComfyUI worker
→ RTX 3070 generation
→ generated image returned to KESPA
→ encrypted private artifact
→ rendered in chat
```

The public application does not communicate directly with raw ComfyUI. ComfyUI remains bound to localhost, while the dedicated bridge provides the externally routable API boundary and bearer-token authentication.

No `brain_api.py` change was required for this implementation.

## Operational generation checkpoint

The recorded initial generation baseline used:

- GPU: NVIDIA GeForce RTX 3070
- VRAM: 8 GB
- checkpoint: `sd_xl_base_1.0.safetensors`
- output resolution: 1024 × 1024
- steps: 25
- CFG: 7
- sampler: `dpmpp_2m`
- scheduler: `karras`
- batch size: 1

The image worker shares the GPU with KESPA's other local AI workloads, so the implementation uses conservative VRAM settings and the source explicitly identifies ongoing VRAM/resource contention as a scaling concern.

## Recorded validation

Three operational layers were exercised successfully:

1. direct ComfyUI API generation,
2. generation through the local KESPA Image Bridge,
3. generation through the public bridge and a real KESPA chat request.

The source records an example local Image Bridge generation latency of approximately **13.7 seconds** and an example public/Cloudflare bridge latency of approximately **16.6 seconds**. These are individual observed examples, **not benchmark means or latency guarantees**.

The real-chat validation completed the full path:

```text
image request detected
→ image_generation capability selected
→ local provider selected
→ image generated on RTX 3070
→ artifact stored
→ image displayed in chat
```

The recorded status for this end-to-end test is **PASS**.

## Generated-image retention

Generated images have two storage stages with different responsibilities.

### Worker copy

Raw ComfyUI output is temporary worker state. The recorded policy removes raw outputs older than approximately 60 minutes, with cleanup running every 15 minutes.

### KESPA artifact copy

After KESPA receives the generated image, the application stores it through the private artifact lifecycle. The recorded metadata identifies generated-image artifacts as AI-generated/generated authority and local encrypted storage at this checkpoint. The binary payload is encrypted at rest and is not intentionally stored as a publicly browsable image under the web root.

Plan-aware generated-image retention was added with the following initial policy:

| Plan | Recorded generated-image retention |
|---|---:|
| Free | 1 hour |
| Plus | 12 hours |
| Pro | 24 hours |
| Enterprise | 168 hours / 7 days |

The policy is configurable. The source also records special values where `-1` means no automatic expiration and `0` means immediately eligible for expiration.

The WWW cleanup worker runs every 10 minutes and uses the normal artifact-deletion lifecycle so associated indexes, message bindings, analysis/enrichment state, and encrypted storage objects are cleaned consistently rather than deleting files blindly.

Generation-event history can outlive the image binary so KESPA can preserve usage/accounting/provider telemetry without retaining generated content indefinitely.

## Storage-provider architecture

The update also extends artifact storage routing so generated images and uploaded artifacts can be assigned independently.

Resolution order is:

```text
per-user storage override
→ plan storage provider
→ built-in local encrypted storage
```

The implementation includes S3-compatible provider support and encrypts artifact content before an external storage backend receives it.

At this checkpoint:

- local encrypted storage is operational,
- the S3-compatible storage framework is implemented,
- no production external object-storage provider is configured,
- changing provider assignments does not silently move historical artifacts,
- future migration is expected to be explicit/queued rather than implicit.

## Security boundary

The recorded security choices include:

- raw ComfyUI is localhost-only,
- public generation traffic terminates at the dedicated Image Bridge,
- generation/model endpoints require bearer-token authorization,
- generated WWW artifacts are encrypted at rest,
- generated images are subject to automatic retention rather than indefinite hosting,
- user uploads and generated images can be routed independently for future cost/storage policy.

Credential values, tunnel identifiers, private filesystem paths, and private configuration details are intentionally not reproduced in this public note.

## Deployment validation

The supplied update records successful schema/application QA for the new retention/storage foundation:

- QA failures: **0**
- QA warnings: **1**
- warning: no external storage providers configured, expected for the current local-storage deployment

The initial retention-worker dry run recorded:

- expired: **0**
- deleted: **0**
- failed: **0**

This establishes that the worker executed without recorded deletion failures at installation time; it does not demonstrate long-term retention behavior across expired production artifacts.

## Current verified state

Verified/recorded operational at this checkpoint:

- local image generation on RTX 3070,
- ComfyUI startup automation,
- dedicated authenticated Image Bridge,
- public bridge endpoint,
- provider routing for `image_generation`,
- automatic chat image-intent path,
- image rendering in chat,
- encrypted artifact retention,
- plan-specific generated-image retention controls,
- worker-side raw-output cleanup,
- WWW generated-image cleanup,
- per-plan storage routing,
- per-user storage overrides,
- S3-compatible storage framework implementation,
- no `brain_api.py` modification required.

## Limitations and non-claims

This release does **not** claim:

- production-final image quality,
- a completed image-generation quality benchmark,
- Fast/Balanced/Quality preset validation,
- production S3/R2 integration,
- external image-generation fallback,
- asynchronous generation queues,
- multi-GPU image scheduling,
- production-scale concurrency,
- a latency service-level objective,
- superiority over external image models or providers.

The source explicitly identifies SDXL Base 1.0 as a functional baseline rather than the final quality model. Planned follow-on work includes controlled checkpoint comparison, richer image UX/editing, async queueing, multi-worker routing, object-storage validation, and longer-term GPU/resource coordination.
