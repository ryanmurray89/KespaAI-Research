# Methodology

## Classification

This material is published as a **technical note**, not as an image-quality benchmark.

The source establishes a working end-to-end local image-generation system and multiple deployment validations. It also explicitly states that quality/model optimization and the image-generation benchmark suite are future work. Accordingly, this package records the verified architecture and operational checkpoint without converting example latencies or subjective image quality into benchmark claims.

## Source handling

The publication was derived only from the supplied KESPA Image Generation V1 implementation/update record and the existing public manifest used to allocate the next technical-note ID.

The source was separated into four evidence classes:

1. **Operational architecture** — request routing, bridge, ComfyUI worker, encrypted artifact return path.
2. **Recorded validation** — direct ComfyUI, local bridge, public bridge, and real-chat generation checks.
3. **Retention/storage controls** — cleanup cadence, plan retention, encrypted storage and storage-provider routing.
4. **Future work / limitations** — quality benchmarking, richer workflows, external fallback, async queues, multi-GPU scaling and production object storage.

Future items were not converted into completed results.

## Status rationale

`verified` is used because the supplied material records successful production checks across the local worker, dedicated bridge, public bridge and real KESPA chat path, plus successful deployment QA for the retention/storage additions.

This status is scoped to the implementation checkpoint. It does not verify final image quality, production-scale throughput, external-storage operation, external-provider fallback, or multi-worker behavior.

## Latency treatment

The source records approximately 13.7 seconds for one local-bridge generation example and approximately 16.6 seconds for one public-bridge example. These values are preserved as `recorded_example` metrics.

They are **not** presented as averages, percentiles, benchmark distributions, or service-level objectives because the source does not establish repeated-run statistics.

## Security/publication treatment

The public artifact preserves the architectural security decisions while omitting unnecessary sensitive implementation details. In particular, this publication does not reproduce credential values, private configuration contents, tunnel identifiers, absolute private filesystem paths, internal tokens, or private user data.

The note does preserve the externally meaningful boundaries that raw ComfyUI remains localhost-only, the Image Bridge is authenticated, and KESPA stores generated-image artifacts encrypted at rest.

## Deduplication boundary

KESPA-NOTE-025 already documents the broader private encrypted artifact storage and scoped retrieval foundation. This note does not duplicate that artifact as a general file-storage release. It records only the image-generation-specific lifecycle additions: operational generation, generated-image retention, cleanup, and provider/storage routing needed to make generated images sustainable in production.
