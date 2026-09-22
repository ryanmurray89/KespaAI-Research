# KESPA-NOTE-026 — Automated Research Operations and Human-Gated Publication Lifecycle

**Type:** Technical Note  
**Date:** 2026-09-22  
**Status:** verified

## Summary

KESPA V2.01 operationalized a formal Research Operations lifecycle that turns benchmark and telemetry work into repeatable, attributable, tamper-evident research evidence while keeping public publication under deliberate human review.

The implemented path can freeze a system baseline, freeze benchmark suites, run and persist benchmark results, generate sanitized exports, commit safe research events to NexLedger, execute recurring regression and telemetry jobs, prepare review bundles, and hand reviewed material into the existing public-research workflow. Automatic public Git publication remains disabled by design.

This note documents the **research operations lifecycle itself**. The first TK-54 performance result is published separately as `KESPA-BENCH-005` and is not duplicated here as a new benchmark claim.

## Research Operations Goal

The operational objective is to replace ad-hoc benchmark handling with a durable lifecycle:

```text
measure
→ preserve configuration and identifiers
→ hash
→ attest safe commitments
→ sanitize
→ prepare review material
→ human / AI-assisted review
→ deliberate public publication
→ verify the public release
→ attest the public release
```

The intentionally manual gate is the decision about what becomes a public research claim.

## Persisted Research State

The update records a dedicated research-operations persistence layer for:

- frozen system/research baselines,
- benchmark suites,
- benchmark cases,
- benchmark runs,
- benchmark results,
- provider-usage events,
- customer/design-partner validation,
- secure research-review delivery state.

Eight research-operations / delivery tables are explicitly identified in the supplied implementation record.

## Frozen Baseline and Suite Boundary

KESPA can now freeze a system/research baseline that preserves enough system state to interpret later results, including such items as:

- Brain version/build,
- model configuration,
- knowledge-card count,
- provider configuration state,
- relevant application/research state,
- database counts,
- artifact configuration,
- public research state,
- applicable Git/system metadata.

The first recorded V2.01 baseline and the first frozen Trusted Knowledge regression suite were both hashed and committed through NexLedger before the first recorded regression run.

The suite is explicitly a new V2.01 engineering regression and is **not** a relabeling or replacement of the historical A/B/C benchmark.

## Benchmark Execution and Storage

The research subsystem records benchmark execution as structured data rather than relying on screenshots or manually transcribed notes.

The supplied operational record identifies the following path as working:

- frozen suite generation,
- benchmark execution,
- SQL run/result storage,
- sanitized benchmark exports,
- benchmark-result hashing,
- NexLedger benchmark attestation.

The completed TK-54 run provides an operational proof point for this path, but its detailed performance metrics are intentionally left to `KESPA-BENCH-005`.

## NexLedger Event Separation

Research evidence uses explicit namespaces with different meanings:

| Namespace | Meaning |
|---|---|
| `BLD` | Frozen system/research baseline or build commitment |
| `DS` | Frozen benchmark suite / dataset commitment |
| `BM` | Completed benchmark result |
| `MET` | Aggregate operational telemetry evidence |
| `RS` | Reviewed and actually published research release / milestone |

A key publication-control rule is that a completed local benchmark can create a `BM` event, but does **not** create an `RS` event merely because a local result exists.

`RS` is reserved for the later public-release state after review, publication, and public-repository verification.

## Recurring Research Automation

The supplied deployment state records two recurring research jobs as active:

1. a daily Trusted Knowledge engineering regression,
2. a daily telemetry attestation.

The regression is intended to build a longitudinal engineering record for changes in:

- quality,
- task success,
- latency,
- TTFT / throughput when available,
- energy,
- RAG behavior,
- escalation behavior.

The telemetry job creates a continuing series of safe `MET` commitments rather than relying on retrospective manual reconstruction.

Daily benchmark evidence is **not automatically promoted into a public research release**. Routine daily variation can remain operational evidence without inflating the public archive.

## Sanitized Research Review Boundary

Completed research-worthy runs can be packaged into sanitized review material. The recorded review bundle includes benchmark/export metadata, protocol material, baseline commitments, NexLedger state, checksums, and publication instructions while intentionally excluding raw private user prompts, raw private answers, credentials, customer data, and private artifact contents.

This creates a separation between:

- measurement and immutable evidence capture,
- interpretation / narrative drafting,
- final publication approval.

## Human-Gated Publication

The canonical boundary is:

```text
AUTOMATIC
benchmark → metrics → sanitized export → hashes → BM attestation → review bundle

REVIEW
human / ChatGPT examines the supplied research evidence and limitations

HUMAN CONTROL
final package is reviewed and manually published to KespaAI-Research

AUTOMATIC AGAIN
KESPA sync verifies the committed public release → RS attestation may be created
```

Automatic public GitHub publishing remains disabled by design.

## Negative and Null Result Policy

The Research Ops rules explicitly preserve:

- failed experiments,
- null metrics,
- inconvenient benchmark outcomes,
- absent retrieval/routing/judge metrics.

The system is not intended to optimize the research record for a favorable narrative. Deterministic scores remain deterministic scores and are not silently relabeled as human or external-judge quality.

## Operational Validation Checkpoint

The September 22 checkpoint records the following state:

- Research Ops: operational,
- baseline freeze: working,
- V2.01 QA: PASS,
- suite generation: working,
- benchmark execution: working,
- benchmark SQL storage: working,
- sanitized benchmark exports: working,
- NexLedger benchmark attestations: working,
- baseline/suite commitments: working,
- daily regression: active,
- daily telemetry attestation: active,
- secure research-review bundle creation: working,
- webhook delivery: working,
- one-time review download: working,
- review-bundle cleanup: active,
- Research Ops Admin surface: implemented,
- automatic public publication: disabled by design,
- human-reviewed public release: required.

## Scope and Limitations

This technical note verifies the operational research lifecycle recorded in the supplied deployment update. It does not claim:

- that the TK-54 benchmark generalizes beyond its frozen configuration,
- that every daily regression should be publicly released,
- that external-judge quality exists when the judge is disabled,
- that retrieval-hit or routing-match metrics exist where the initial suite did not define them,
- that the automated research system independently decides scientific significance,
- that the public release process is fully autonomous.

## Next Research Direction

With TK-54 automated, the recorded next major research milestones are:

- `AR-30` — private artifact recall,
- `MM-24` — multimodal evidence.

Those future experiments are intended to test whether private evidence can be retrieved correctly across time/scope and whether image/document evidence can become reliable, provenance-preserving intelligence.
