# KESPA-NOTE-030 — Private-Safe Image Editing and Deterministic Transformation Routing

## Summary

KESPA added a first-class image-editing path that preserves private-data routing boundaries while allowing authorized local processing and deterministic image transformations. The implementation grew out of a correctly fail-closed condition: KESPA could recognize a private image-edit request, but no provider was eligible to receive the private source image, so the request stopped with `image_edit_private_route_unavailable` rather than silently sending the image to an unauthorized processor.

The v7.0 work introduced an explicit `image_edit` capability, designated the KESPA Local Image Bridge as an authorized local processor for that capability, added deterministic transparent-background handling, introduced provider fallback strategies, improved edit lineage/provenance, and prevented raw internal routing failures from being exposed directly to end users.

This release records the architecture and implementation checkpoint. The supplied source marks the broader v7.0→v7.3 session as **Implemented / Pending Production Validation**, so this note is published rather than upgraded to verified.

## Why this is distinct from KESPA-NOTE-028

KESPA-NOTE-028 documented local-first **image generation**: generating a new image, routing it through the local image bridge/ComfyUI stack, storing the result as an encrypted artifact, and applying plan-based retention/storage policy.

This note covers a different problem:

```text
existing/private image
→ edit intent
→ private-data eligibility check
→ authorized image-edit route
→ deterministic or provider-backed transformation
→ lineage/provenance preservation
→ safe user-facing result/error
```

The key research/engineering boundary is that image editing must not weaken the privacy guarantees already applied to private artifacts.

## Fail-closed private routing

The observed failure mode was:

```text
image_edit_private_route_unavailable
```

That condition represented correct privacy behavior rather than a reason to bypass the routing policy. KESPA had identified an image-edit request involving private image data but did not have a route authorized to receive that data.

The implementation response was to create an explicit capability and eligibility path rather than treating image editing as generic image generation or allowing private content to flow to any configured external provider.

## First-class `image_edit` capability

Image editing is now represented as its own provider capability.

Conceptually:

```text
user requests edit
→ orchestration classifies image_edit
→ source image privacy/scope is evaluated
→ provider routes are filtered for image_edit eligibility
→ authorized route is selected
→ transformation executes
→ result is retained with lineage/provenance
```

This preserves the larger KESPA provider model: processors are replaceable and route eligibility is controlled by the application/control plane rather than by weakening the durable privacy boundary.

## Authorized local image processing

The KESPA Local Image Bridge was explicitly trusted/authorized for private image-edit processing.

That matters because the local bridge is under KESPA/NexLabs control and can process source-image data without requiring that private evidence be sent to an unrelated public provider.

The source does not establish that every future external image provider is private-data eligible. Eligibility remains route/provider policy rather than an assumption created merely because a provider is configured.

## Deterministic transparent-background transformation

The v7.0 implementation also added deterministic transparent-background processing.

This is important because not every image transformation needs a generative model. For transformations that can be performed deterministically, KESPA can avoid unnecessary model/provider dependence while producing a predictable operation over the source image.

The source specifically identifies transparent-background processing as one such deterministic path. It does not provide benchmark measurements for accuracy, edge quality, latency, or general segmentation performance, so no such metrics are claimed here.

## Provider fallback

Provider fallback strategies were added for image editing so the capability is not tied to one hardcoded implementation.

The release preserves the following boundary:

- capability routing is controlled by Admin / `brain_config` / provider-routing data,
- private-data eligibility is checked before a route may receive private image content,
- an unavailable eligible route should fail closed rather than silently degrade privacy,
- additional providers may be used only when they satisfy the configured capability and privacy rules.

The supplied source does not enumerate a production-tested fallback matrix, so this package does not claim that every fallback path has been validated in production.

## Lineage and provenance

Image edits now preserve stronger lineage/provenance semantics.

An edited result should remain attributable to its source image and transformation path rather than appearing as an unrelated new artifact. This supports later auditability, evidence handling, regeneration/re-edit workflows, and user-facing provenance.

The source establishes that lineage/provenance handling was improved, but it does not provide a complete public schema for every lineage field. This release therefore records the capability without inventing undocumented fields.

## Error handling

The implementation also prevents raw internal image-routing errors from leaking directly to the user.

Internal failures such as provider eligibility/routing problems remain operational diagnostics, while user-visible behavior is handled through the orchestration/error layer. This keeps internal architecture details from becoming accidental public error messages and separates diagnosable system state from end-user messaging.

## Control-plane ownership

The source explicitly states:

- control plane: Admin / `brain_config` / provider routing database,
- Brain runtime modification: none required for this work.

That matches the broader KESPA architecture rule that policy, provider eligibility, routing, and tunable behavior should be controlled outside the locked Brain runtime whenever practical.

## Current publication status

Recorded implementation state:

```text
Release range:                 v7.0 → v7.3
Image-edit implementation:     v7.0
Overall session status:        Implemented / Pending Production Validation
Brain runtime modification:    none required
```

For that reason, this technical note is classified as `published`, not `verified`.

## Limitations and non-claims

This release does **not** claim:

- production validation of every image-edit path,
- benchmarked edit quality,
- benchmarked transparent-background quality,
- universal support for arbitrary image transformations,
- production validation of every configured fallback provider,
- authorization for every external provider to receive private image data,
- any specific image-edit latency or throughput,
- that deterministic transparent-background processing replaces generative editing generally,
- that the Brain runtime was modified to implement the feature.

## Engineering result

The central result is architectural: KESPA now treats image editing as a distinct, privacy-aware capability with provider eligibility, local trusted processing, deterministic transformations where appropriate, provenance/lineage, and safer error handling. The implementation closes the earlier gap without weakening the private-artifact boundary or making `brain_api.py` the policy control point.
