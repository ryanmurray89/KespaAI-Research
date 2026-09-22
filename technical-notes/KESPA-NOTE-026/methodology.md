# Methodology

## Objective

Document the operational KESPA V2.01 Research Operations lifecycle without duplicating the newly published TK-54 benchmark result or converting routine daily regression output into unnecessary public entries.

## Source material

Primary source:

- user-supplied `KESPA AI — RESEARCH OPS / AUTOMATED BENCHMARKING / SECURE REVIEW DELIVERY UPDATE`, dated 2026-09-22.

Archive context used for deduplication:

- current KESPA public research manifest after `KESPA-BENCH-005`, containing 39 entries and 232 declared public artifact files.

The already supplied V2.01 benchmark review bundle was treated as corroborating context for the existence of the frozen baseline, frozen suite, completed benchmark run, and NexLedger commitments, but the benchmark performance result itself remains published separately as `KESPA-BENCH-005`.

## Deduplication decision

The update contains several kinds of material:

1. the already published TK-54 benchmark result,
2. a new operational Research Ops lifecycle,
3. a secure review-delivery subsystem,
4. ordinary Admin/UI readability work,
5. future AR-30/MM-24 plans.

The benchmark result was excluded from this technical note as a duplicate of `KESPA-BENCH-005`.

Routine Admin styling/readability work was not promoted into public research.

Future AR-30/MM-24 plans are mentioned only as future direction, not completed results.

This package preserves the distinct operational accomplishment: turning baseline/suite freezing, benchmark execution, SQL persistence, sanitized exports, NexLedger commitments, recurring jobs, review packaging, and human-gated publication into one coherent research lifecycle.

## Evidence classification

### Directly recorded operational state

The source explicitly records operational/working/active state for Research Ops, baseline freeze, QA, suite generation, benchmark execution, SQL storage, exports, NexLedger commitments, recurring regression/telemetry, review-bundle creation, webhook delivery, one-time download, cleanup, and the Research Ops Admin surface.

### Derived values

Only simple enumerations were derived:

- eight explicitly identified research / delivery tables,
- five explicitly named NexLedger research namespaces,
- two recurring research jobs recorded active.

No benchmark-performance value was recalculated or reinterpreted in this note.

### Future work

AR-30 and MM-24 remain future milestones. They are not counted as completed experiments here.

## Publication and privacy boundary

The public note intentionally omits:

- private user/customer content,
- raw benchmark prompts and raw answers,
- credentials and API keys,
- authorization/session data,
- private artifact content,
- secret webhook values,
- token values,
- internal filesystem ACL commands and deployment-specific download endpoint details,
- private application source code.

Only the architecture-level lifecycle, safe identifiers, high-level operational state, and public research-governance rules are preserved.

## Status rationale

Status is `verified` because the supplied update explicitly identifies the subsystem as implemented/operational and records the major lifecycle components as WORKING/ACTIVE/PASS, with a completed frozen baseline, frozen suite, benchmark run, and NexLedger commitments already demonstrated in the accompanying research material.

The verification status applies to the operational lifecycle recorded at this checkpoint. It does not imply an independent third-party audit or scientific generalization of any benchmark outcome.
