# KESPA-NOTE-025 — Private Encrypted Artifact Storage and Scoped Retrieval Foundation

**Type:** Technical Note  
**Date:** 2026-09-22  
**Status:** verified

## Summary

KESPA implemented a private artifact foundation for user-uploaded files and images. The architecture validates uploads, encrypts original artifacts at rest, stores them outside the public web root, records owner-scoped metadata/index data, and associates artifacts with chat, project, or account scope so later retrieval can respect where the user intended the artifact to be available.

A live production upload was successfully stored as an encrypted `.kespa` blob after two independent failures were diagnosed and corrected. The stored artifact was recorded as owned by the PHP service account and permissioned `600`. Runtime storage was also removed from Git tracking so uploaded artifacts, logs, caches, backups, and other private state remain outside source control.

This note verifies the **private storage and scoping foundation**. It does not claim that semantic cross-chat artifact recall, multimodal enrichment, or every supported file extractor has been fully validated end to end.

## Technical Goal

The artifact system is intended to preserve private user material as evidence that can be retrieved later without collapsing files into ordinary global memory.

The recorded workflow is:

```text
Upload
→ validate
→ encrypt
→ store outside public web root
→ extract / OCR where available
→ index
→ apply scope
→ retrieve later under owner/scope controls
```

This creates a distinct boundary between:

- conversational context,
- durable user memory,
- public Trusted Knowledge,
- private user artifacts.

## Scope Model

KESPA defines three artifact scopes:

| Scope | Intended availability |
|---|---|
| Chat | Available to the conversation that owns the artifact. |
| Project | Available within the associated project workspace. |
| Account | Available to later conversations for the same user when retrieval policy selects it. |

The scope model is designed to prevent a chat-only file from becoming globally available simply because it was uploaded once.

## Private Storage Boundary

The configured private artifact root is outside the public web root:

```text
/var/www/kespa/storage/private_artifacts
```

Artifact binaries are stored as encrypted `.kespa` blobs using per-user UUID-based paths. The public artifact intentionally omits the specific test user's identifier and stored artifact UUID.

The recorded live storage checkpoint established that the successfully stored artifact was:

- encrypted at rest,
- outside the public web root,
- owned by the PHP service account,
- permissioned `600`.

## Metadata and Retrieval Index Boundary

The supplied implementation records separate responsibilities for artifact metadata, chunk/index information, search terms, and message associations. Explicitly named structures include:

- `user_artifacts`
- `user_artifact_chunks`
- `user_artifact_terms`
- `message_artifacts`

Additional multimodal/enrichment structures exist in the later integration work, but they are outside the verified scope of this note.

The design allows extracted/indexed representations to support later retrieval while the original binary remains encrypted.

## Supported Input Coverage

The current upload path is intended to handle common text, data, code/configuration, document, Office, and image formats, including:

- TXT / Markdown
- CSV / JSON
- code/configuration files
- PDF
- DOCX
- XLSX
- PPTX
- JPG / PNG / WebP / GIF

Extraction quality depends on the host tools and libraries available for a particular format. The source documentation specifically identifies `pdftotext`, `tesseract`, and PHP ZIP support as useful host capabilities.

## Production Failure and Correction Record

Two independent upload failures were recorded during live bring-up.

### 1. Private storage was not writable

PHP-FPM could not initially create the private artifact storage path.

Correction:

- create the required storage directory,
- grant the PHP service account the required write access,
- verify the service account could write to the private path.

### 2. Numeric lexical terms triggered a strict type error

Numeric-string array keys were converted by PHP into integers before a term-hashing function expecting a string was called.

Correction:

- explicitly cast search terms to string before hashing.

After these corrections, the final test artifact was successfully stored.

## Git / Runtime Separation

Runtime storage is excluded from Git using the repository storage ignore boundary:

```text
/storage/*
```

Two historical backup JSON files that had been tracked before the ignore rule existed were removed from the Git index. The recorded follow-up `git ls-files storage` returned no tracked storage content.

This keeps private/runtime material such as uploads, logs, backups, caches, synchronized research runtime data, and enrichment logs out of source control.

## Security Boundary

The current design records the following non-negotiable artifact rules:

- originals remain encrypted at rest,
- binaries stay outside the public web root,
- every read/search/download/delete path must enforce ownership and scope,
- MIME/type/size validation is server-side,
- upload/plan limits are enforced from server-side database policy rather than browser state,
- private runtime storage is excluded from Git.

No keys, encryption material, private file contents, user identifiers, or private artifact UUIDs are published in this package.

## Validation Boundary

Directly demonstrated or explicitly recorded as complete at this checkpoint:

- private artifact architecture implemented,
- chat/project/account scoping defined,
- encrypted artifact storage outside the public web root,
- a live artifact physically stored after production fixes,
- stored artifact permission mode recorded as `600`,
- four core metadata/index tables explicitly identified,
- two upload failures diagnosed and corrected,
- runtime `storage/` content removed from Git tracking.

Not yet claimed verified end to end:

- semantic cross-chat account-scoped retrieval quality,
- embedding-based retrieval,
- multimodal/vision enrichment,
- background enrichment worker under production cron,
- every Office/PDF/image extractor path,
- artifact deduplication/versioning,
- cold/object-storage migration,
- backup/restore and retention lifecycle.

## Next Technical Direction

The recorded roadmap moves from secure storage toward durable artifact intelligence:

```text
encrypted private artifact
→ extraction / OCR
→ structured metadata
→ chunks + embeddings
→ lexical + semantic + metadata retrieval
→ reranking
→ provenance back to the original artifact
```

The target user experience is to locate a previously uploaded artifact from the correct private scope and answer from that evidence rather than relying on vague conversational memory.
