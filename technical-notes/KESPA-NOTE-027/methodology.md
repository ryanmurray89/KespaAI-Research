# Methodology

## Objective

Document the implemented KESPA Conversation Engine and SQL-driven personality-contract architecture as a distinct technical result while preserving the boundary between verified implementation behavior and future conversation-quality benchmarking.

## Source material

Primary source:

- user-supplied Jira update, `KESPA Conversation Engine + SQL-Driven Personality Contract`, dated 2026-09-23 and marked `Implemented baseline, tuning phase`.

Archive context:

- the current generated KESPA public research manifest through `KESPA-NOTE-026`, used to allocate `KESPA-NOTE-027` and avoid duplicating prior notes covering memory, Research Ops, artifact storage, or adaptive runtime benchmarking.

## Deduplication decision

This work is materially distinct from existing archive entries because it addresses **conversation-style policy and enforcement** rather than:

- durable memory storage,
- private artifact retrieval,
- research automation,
- Trusted Knowledge lifecycle,
- adaptive compute benchmarking.

The live dialogue examples are implementation evidence, not a formal benchmark. They are therefore documented inside this technical note rather than inflated into a separate experiment or benchmark entry.

## Evidence classification

### Directly recorded

The source directly records:

- the Conversation Engine as implemented,
- the current local model identifier,
- the current Brain build identifier,
- 17 named response-profile dimensions,
- 11 named posture classes,
- eight primary `conversation_*` configuration rows,
- current-context precedence over persistent style preferences,
- durable communication-preference influence,
- machine-readable contract generation,
- generic Brain-side validation/style repair,
- live examples showing differentiated conversational behavior,
- ongoing tuning needs,
- future plans for deterministic regression and personality-quality metrics.

### Derived

Only simple counts were derived from explicit lists:

- 17 response dimensions,
- 11 posture classes,
- eight primary configuration rows,
- four displayed live interaction examples.

No quality percentage, posture accuracy, repair success rate, or user-satisfaction metric was derived.

## Validation boundary

The note uses status `verified` because the supplied Jira update explicitly marks the baseline as implemented, describes current Brain/application components, and provides live interaction examples under a section titled current verified behavior.

The status applies to the implementation/architecture checkpoint, not to general conversational quality or scientific validation.

The source explicitly states that at least one supportive response still needs wording polish and that formal conversation regression / personality metrics remain future work.

## Publication/privacy boundary

The public package intentionally omits:

- raw private conversation history,
- user identities or account data,
- private memory contents,
- absolute production filesystem paths,
- raw SQL dumps,
- private application source code,
- internal prompt bodies beyond safe high-level behavioral examples,
- secrets, credentials, API tokens, cookies, or session data.

Safe component names, configuration-key names, model/build identifiers, architecture, and derived list counts are preserved because they are necessary to understand and reproduce the design boundary.

## Interpretation rule

This note documents a maintainability and policy-separation architecture:

```text
application/configuration decides conversational posture
→ Brain enforces the supplied contract generically
```

It does not claim that the personality system is optimal, that the local model is generally superior, or that planned regression metrics have already been measured.
