# KESPA-NOTE-029 — Zero-User Curriculum Learning and Evidence-Gated Autonomous Research

## Summary

KESPA established an operational zero-user learning driver that can create useful research work without waiting for an active chat. The system combines an explicit curriculum, scheduled daily learning, background lifecycle workers, knowledge-freshness policy, benchmark-gap inputs, and hard publication/promotion safety boundaries so autonomous activity can generate research opportunities without turning research quotas into unverified Trusted Knowledge.

This note records the production checkpoint where the scheduling, persistence, curriculum selection, queue initiation, and key fail-closed controls were observed working. It does **not** claim that every queued research request has been proven to complete every downstream evidence, verification, conflict, promotion, indexing, and reverification stage under all operating conditions.

## Why this is distinct from earlier KESPA autonomous-research work

Earlier KESPA research records documented user-triggered knowledge-gap acquisition, the natural research-gap lifecycle, evidence qualification, signed cross-host handoff, Trusted Knowledge promotion, and the foundation for freshness/reverification.

This checkpoint adds a different control loop:

```text
no active user required
→ curriculum / benchmark gaps / stale knowledge
→ scheduled research opportunity selection
→ queued research work
→ asynchronous lifecycle processing
→ evidence and verification gates
→ eligible knowledge lifecycle handling
→ freshness / reverification
→ repeat
```

The material difference is that **research initiation itself can now be proactive and scheduled**, while promotion and publication remain independently gated.

## Curriculum-driven learning

The recorded production curriculum contains **24 enabled research domains**. Curriculum entries support operational fields such as topic identity, research directive, priority, minimum interval, enabled state, previous queue time, next due time, and research count.

Selection is intended to favor due and under-researched areas rather than repeatedly consuming the same topic. The source describes bulk curriculum ingestion as future work; large-scale importer functionality is therefore not claimed as operational in this release.

## Evidence-gated learning lifecycle

The intended learning path is:

```text
curriculum / knowledge gaps / stale knowledge
→ research request
→ research processing
→ source and evidence collection
→ claim extraction
→ verification
→ conflict checking
→ knowledge lifecycle handling
→ eligible Trusted Knowledge
→ freshness / reverification
```

A research request is **not** equivalent to a Trusted Knowledge card.

The source explicitly establishes the desired fail-closed behavior:

```text
research attempted: yes
evidence insufficient: yes
trusted card created: no
```

That outcome is considered successful when the evidence does not justify promotion. KESPA is intended to prefer zero new cards over unsupported cards.

## Scheduling and background execution

The autonomous-learning driver is connected to operating-system scheduling.

The recorded daily-learning timer is installed and enabled, with the default cycle occurring at approximately **02:00**. The corresponding service is intentionally one-shot: after a successful execution it exits normally while the timer remains active.

Separate lifecycle workers are scheduled on an approximately **5-minute** cadence. These workers support activities including research-inbox processing, knowledge refresh, knowledge lifecycle processing, and artifact enrichment. Filesystem locking is used to avoid overlapping worker execution when a previous cycle is still running.

## First production zero-user run

The first recorded autonomous daily-learning run completed successfully.

Recorded result:

- daily-learning status: **complete**
- queued public research requests: **4**
- automatic public publication: **off / human-gated**

A later readiness audit also reported the most recent daily-learning run as complete.

This demonstrates that the scheduler, autonomous-learning persistence layer, curriculum selection path, and research-queue initiation were operational at the recorded checkpoint. It does not establish universal downstream completion for every queued request.

## Knowledge freshness safeguard

The production checkpoint also records an active maximum verification-age ceiling of **30 days**.

This ceiling is an additional safeguard rather than a claim that all knowledge changes on the same schedule. KESPA retains differentiated freshness classes such as static, slow-changing, volatile, highly volatile, event-driven, and manual.

## Benchmark-gap feeder

Benchmark failures and knowledge gaps may feed new research opportunities without copying benchmark prompt/gold content into the learning pipeline.

The intended separation is:

```text
benchmark exposes weakness
→ identify topic/category gap
→ create research opportunity
→ improve corpus independently
→ future benchmark measures whether performance changed
```

This protects the benchmark from answer contamination while still allowing evaluation failures to guide future knowledge acquisition.

## Publication and promotion safety boundaries

Two safety boundaries were recorded as hard-disabled:

### Automatic public research publication

**HARD OFF.** Public research remains human-reviewed.

### Blind Trusted Knowledge promotion

**HARD OFF.** Autonomous learning does not impose a minimum card-production quota and may complete successfully with no promoted knowledge when evidence is insufficient.

These controls intentionally separate:

```text
research generation
!= evidence sufficiency
!= Trusted Knowledge promotion
!= public research publication
```

## Readiness audit checkpoint

The production lock-in readiness audit recorded:

- failures: **0**
- warnings: **1**
- result: **PASS WITH WARNINGS**

The remaining warning was that an external S3-compatible artifact-storage provider was not configured. The source identifies this as non-blocking because local encrypted artifact storage remained active.

The audit covered major areas around autonomous-learning schema/policy, curriculum, verification-age policy, benchmark-gap feeding, last learning run, lifecycle scripts, publication safety, promotion safety, scheduling, worker cron, and the cron daemon, alongside broader application readiness checks.

## Pause and audit model

The source defines an important operational principle: autonomous systems must remain stoppable and auditable.

Some pause/audit controls are stated as **requirements or next-stage expectations**, not all as completed production features. These include:

- a global autonomous-learning pause,
- independent provider disablement,
- per-topic curriculum enable/disable,
- promotion freeze independent of research generation,
- emergency scheduler shutdown without deleting queued work,
- run-level accounting for selected topics, requests, evidence, claims, verification, conflicts, promotions/rejections, provider usage, tokens, cost, latency, retries, and final state.

These requirements are preserved here because they define the safety boundary for expanding autonomy, but they are not upgraded to verified implementation claims where the source only specifies future/expected behavior.

## Current verified state

Recorded operational at this checkpoint:

- autonomous-learning schema/persistence foundation,
- 24 enabled curriculum domains,
- zero-user research opportunity generation,
- daily systemd scheduling installed/enabled,
- approximately five-minute lifecycle worker scheduling,
- first autonomous daily run completed,
- four research requests queued by that run,
- benchmark-gap research feeder present,
- 30-day maximum verification-age safeguard,
- automatic public research publication hard-disabled,
- blind Trusted Knowledge promotion hard-disabled,
- lock-in readiness audit completed with zero failures and one non-blocking warning.

## Limitations and non-claims

This release does **not** claim:

- that every autonomous research request completes the full downstream lifecycle,
- that all queued research produces new knowledge,
- that card-count growth is a quality metric,
- that the bulk curriculum importer is implemented,
- that the full Learning Operations dashboard is implemented,
- that all proposed pause/kill-switch UI controls are already deployed,
- that autonomous promotion volume has been validated at large scale,
- that general learning from ordinary user conversations is safe for a shared corpus,
- that large-corpus retrieval or concurrency limits have been established,
- that external-provider use is automatically beneficial.

The source explicitly recommends controlled expansion: freeze a baseline, add workload, remeasure quality/cost/compute, and continue only where measurable benefit justifies the added complexity.

## Engineering principle

The recorded direction is not to maximize the number of knowledge cards. The system should maximize useful, retrievable, current, provenance-backed intelligence per unit of compute, cost, and complexity.

The meaningful checkpoint established here is that KESPA can now initiate useful research work without a user actively teaching it, while retaining evidence, promotion, publication, and future audit boundaries around what that autonomous work is allowed to become.
