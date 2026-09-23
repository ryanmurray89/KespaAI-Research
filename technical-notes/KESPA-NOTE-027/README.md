# KESPA-NOTE-027 — Conversation Engine and SQL-Driven Personality Contract

**Type:** Technical Note  
**Date:** 2026-09-23  
**Status:** verified

## Summary

KESPA implemented an application-side Conversation Engine and SQL-driven personality contract so conversational style can be selected and tuned as runtime configuration rather than through repeated phrase-specific changes to the Brain runtime.

The engine evaluates the current turn using current situational context, recent conversational tone, durable user communication preferences, and `brain_config` policy. It produces a response profile containing a posture plus numeric/behavioral dimensions, then compiles those decisions into a machine-readable contract for Brain. Brain generates the response, validates style compliance, and can perform a style-only repair when needed.

The main architectural outcome is a separation of responsibility:

```text
Conversation Engine / SQL configuration
→ decides how KESPA should communicate

Brain
→ enforces the supplied behavior contract generically
```

Normal personality tuning therefore belongs in configuration/Admin rather than requiring a new phrase-specific Brain patch.

## Problem

The current local reasoning model was repeatedly falling back to generic chatbot/customer-service language even when the application had already identified the desired conversational posture.

Prompt placement improved behavior but was not reliable enough. A temporary implementation that removed known canned phrases in Brain demonstrated the problem but was not a maintainable solution because every new wording failure would require another runtime code change.

The replacement architecture makes personality policy data-driven and moves phrase-by-phrase tuning out of the Brain.

## Conversation Engine

The Conversation Engine derives a response profile with 17 recorded dimensions:

- social temperature,
- warmth,
- energy,
- formality,
- directness,
- seriousness,
- humor,
- sarcasm,
- playfulness,
- celebration,
- sympathy,
- curiosity,
- challenge,
- proactivity,
- question bias,
- closure bias,
- verbosity.

It selects from 11 recorded posture classes:

- neutral,
- focused,
- collaborative,
- playful,
- celebratory,
- supportive,
- serious,
- urgent,
- reflective,
- challenging,
- listening.

These postures intentionally produce different delivery characteristics rather than one global tone.

## Context Precedence

Current situational context has higher authority than ordinary user style preferences.

For example, a user who normally prefers jokes or profanity should not receive humor merely because those preferences exist when the current message clearly describes grief or loss. The recorded implementation applies serious-context clamps so humor and sarcasm can be suppressed, energy reduced, sympathy increased, and unnecessary questions avoided.

This prevents persistent style preferences from overriding an obviously incompatible current situation.

## Durable Communication Preferences

KESPA can use durable communication-preference memories to influence characteristics such as:

- concise versus detailed responses,
- directness,
- humor or sarcasm tolerance,
- professional/formal style,
- tolerance for unnecessary follow-up questions.

The design explicitly distinguishes stable communication preferences from transient emotional states. Persistent preferences may influence future conversations, but temporary states such as sadness are not intended to become durable personality labels.

## SQL-Backed Configuration

The runtime behavior is backed by `brain_config` rather than hard-coded phrase rules.

Eight primary Conversation Engine configuration rows are recorded:

- `conversation_engine_enabled`,
- `conversation_engine_policy`,
- `conversation_posture_profiles`,
- `conversation_signal_policy`,
- `conversation_humor_policy`,
- `conversation_high_stakes_policy`,
- `conversation_prompt_policy`,
- `conversation_telemetry_policy`.

These rows separate global engine behavior, posture targets, signal detection, humor permission, high-stakes clamps, prompt/contract behavior, and telemetry policy.

Configuration can therefore evolve without redefining personality in the model runtime.

## Machine-Readable Personality Contract

Instead of relying only on vague prose instructions, the engine compiles explicit behavioral constraints for the current response.

A contract can express expectations such as:

- avoid or permit a follow-up question,
- disallow generic offers of assistance,
- limit response length,
- mirror the user's conversational register,
- avoid humor,
- avoid unsolicited advice,
- acknowledge the situation before continuing.

The exact wording remains model-generated. The contract defines behavioral boundaries rather than a canned response.

## Brain Enforcement Boundary

The recorded Brain build at this checkpoint is:

```text
dev-019-personality-contract-v2
```

The current local model is:

```text
qwen2.5-coder:7b-instruct-q4_K_M
```

Brain's personality-related path is now:

```text
receive contract
→ generate response
→ validate style compliance
→ perform style-only repair when required
→ return response
```

Brain does **not** own the policy decision about whether KESPA should be playful, supportive, formal, brief, sarcastic, or celebratory. Those decisions belong to the Conversation Engine and its configuration.

## System Integration

The personality layer remains separate from factual/reasoning systems.

It does not replace or override:

- routing,
- memory retrieval,
- Trusted Knowledge,
- RAG,
- web evidence,
- planning,
- verification,
- safety or evidence policy.

This keeps style adaptation from becoming a substitute for correctness or provenance.

## Recorded Live Behavior

The supplied implementation record includes live interactions covering casual greeting, celebration, continued celebration, and grief/support scenarios. Those tests showed materially different behavior based on posture rather than one generic support-assistant response pattern.

One supportive response was explicitly described as still needing wording polish. That limitation is preserved here: the architecture is implemented and functioning, but conversational quality remains in a tuning phase.

No formal posture-accuracy percentage, contract-violation rate, user-satisfaction score, or conversation-regression benchmark was supplied, so none is claimed.

## Tuning Model

The intended workflow is now:

```text
behavior sounds wrong
→ inspect posture and telemetry
→ adjust SQL/Admin policy or posture profile
→ retest
```

rather than:

```text
behavior sounds wrong
→ add another phrase-specific Brain patch
```

This is the primary maintainability improvement of the work.

## Telemetry Direction

The implementation identifies telemetry fields useful for separating classification failures from configuration or model-obedience failures, including:

- posture,
- reason codes,
- conversation dimensions,
- supplied/applied directive state,
- contract version,
- style-validation result,
- repair attempted/succeeded,
- repair attempts and latency,
- model used,
- user feedback.

The design explicitly avoids retaining raw emotional profiling as personality telemetry.

## Limitations

This checkpoint does **not** establish:

- formal conversation-posture accuracy,
- a measured contract-violation rate,
- a measured repair success rate,
- user-rated naturalness improvements,
- superiority over other conversational models,
- that every response is free of generic or robotic wording,
- that the planned deterministic conversation regression suite has been completed.

The local model remains coder-tuned, and the source explicitly records that some supportive wording still needs refinement.

## Future Work

Recorded future work includes:

- a readable Admin tuning interface over the SQL configuration,
- a structured communication-preference profile with confidence and reinforcement history,
- feedback-driven preference adaptation,
- a deterministic conversation regression suite based on expected posture/permitted behavior rather than exact wording,
- posture/contract/repair quality metrics,
- a dedicated conversational-model role when hardware allows.

These items remain future work and are not represented as completed results in this release.
