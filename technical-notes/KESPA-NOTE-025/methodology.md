# Methodology

## Objective

Document the first production-capable private artifact-storage boundary in KESPA without overstating later semantic or multimodal capabilities.

## Source review

This note was built from three user-supplied September 22, 2026 project documents:

1. the detailed KESPA Jira/project update,
2. the current-state README addendum,
3. the future-plan document.

The existing public research manifest was also used to allocate the next technical-note ID and to check for overlap with prior archive entries.

## Deduplication decision

This artifact was kept separate because the existing archive documents public Trusted Knowledge indexing, user/community telemetry, research automation, and freshness/reverification, but does not document the private user-artifact storage boundary.

Routine chat UI work, account-menu cleanup, and other ordinary product changes from the same development batch were not promoted into separate public artifacts.

## Evidence classification

### Recorded completed state

The source directly records that:

- the private artifact architecture is implemented,
- artifacts support chat/project/account scoping,
- original artifacts are encrypted and stored outside the public web root,
- a live artifact was successfully stored after two production failures were fixed,
- the successful artifact had permission mode `600`,
- core metadata/index structures are present,
- runtime storage was removed from Git tracking.

### Derived values

Only simple counts were derived:

- 3 scope classes from chat/project/account,
- 4 explicitly named core artifact tables,
- 2 enumerated upload failures.

No performance, capacity, encryption-strength, retrieval-quality, OCR-quality, or latency values were inferred.

### Design / future work

The supplied sources describe OCR, embeddings, semantic retrieval, multimodal enrichment, deduplication, versioning, and storage abstraction as implemented direction or future work. Where production end-to-end validation was explicitly still open, this note keeps those capabilities outside the verified boundary.

## Privacy and publication method

The public package intentionally excludes:

- the test user's identifier,
- the artifact UUID and exact user-specific artifact path,
- uploaded file contents,
- encryption keys or secret material,
- private application source code,
- credentials and provider secrets.

Only architecture-level storage paths, table names, high-level failure causes, and validation outcomes needed to describe the technical boundary are retained.

## Status rationale

Status is `verified` because the primary claim of this note is the private storage/scoping foundation, and the source records a successful live encrypted artifact store plus the corresponding production corrections.

The status does **not** extend verification to semantic retrieval, background enrichment, or future storage features; those limitations are called out explicitly in the package.
