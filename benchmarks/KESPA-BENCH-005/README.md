# KESPA-BENCH-005 — V2.01 Trusted Knowledge Regression Baseline

## Summary

KESPA completed a frozen 54-case Trusted Knowledge engineering regression against the V2.01 adaptive runtime. All 54 cases completed without an execution failure. Deterministic task success was **50/54 (92.5926%)**, with a mean deterministic quality score of **83.4 / 100**.

This is a **deterministic engineering regression result**, not an externally judged benchmark and not evidence of general performance beyond the frozen suite, configuration, runtime, knowledge snapshot, and measurement conditions represented by this run.

## Run identity

- Run UUID: `1229d2a0-5473-426d-899c-bda596dc5e7a`
- Suite: `trusted-knowledge-regression` v2026-09-22
- Suite UUID: `d8d945ae-6879-4c1a-bf67-13895c585859`
- Purpose: `regression`
- Condition: `adaptive`
- Model route: `local-primary`
- Brain: `1.1.1-dev` / `dev-013-trusted-knowledge-index`
- Knowledge cards: **500**
- Config SHA-256: `f570257df712ecbf4740c82ab6bf96c7f3621d2d52f5058a371c7e5e394821fe`
- Run summary SHA-256: `042a5ea58c03ab13cddef2287622d17f88b2f3d241eda2c9b8e6a30b8aae3940`
- Frozen suite commitment: `897908b5da8f32a39ee6e2b8b54f3f2021c303ad12d9427eba8b695f28441309`

## Recorded summary metrics

| Metric | Value |
|---|---:|
| Total cases | 54 |
| Completed cases | 54 |
| Execution failures | 0 |
| Deterministic task successes | 50 / 54 |
| Deterministic task failures | 4 / 54 |
| Mean deterministic quality | 83.4 / 100 |
| Mean lexical F1 | 0.684776 |
| Mean required-term rate | 0.914352 |
| Mean latency | 2653.463 ms |
| Mean TTFT | 234.574 ms |
| Mean tokens/sec | 66.001 |
| Mean GPU energy | 0.14564943 Wh/case |
| RAG rate | 1 |
| Escalation rate | 0.018519 |
| Provider cost attributed | $0 |
| Quality/sec | 31.430636 |
| Quality/Wh | 572.608009 |
| External judge score | n/a |
| Retrieval hit rate | n/a |
| Routing match rate | n/a |

`failed_cases = 0` means all benchmark executions completed. It does **not** mean every case passed deterministic task scoring; four cases were scored as deterministic task failures.

## Negative results retained

| Case | Deterministic quality | Lexical F1 | Required-term rate |
|---|---:|---:|---:|
| `tk-75` | 9.2917 | 0.033333 | 0.125 |
| `tk-74` | 62.2207 | 0.617021 | 0.625 |
| `tk-38` | 32.627 | 0.235772 | 0.375 |
| `tk-72` | 41.6971 | 0.262774 | 0.5 |


The per-case export records **54/54 RAG use**, **1/54 escalations**, and **1/54 web-use cases**. These counts are derived directly from the 54 exported result rows. Retrieval-hit and routing-match values were not produced for this run, so no retrieval-accuracy or routing-accuracy claim is made.

## Derived aggregate totals

The following totals were calculated by summing the supplied per-case rows:

- GPU energy: **7.865069 Wh**
- Prompt tokens: **46,266**
- Output tokens: **7,113**
- Total tokens: **53,379**
- LLM calls: **54**

## Integrity / provenance

The frozen V2.01 baseline, frozen suite, and completed benchmark were separately attested in NexLedger as BLD, DS, and BM events. The BM event for this run is `0ab84032-a5ab-4c4c-863d-95d185460103`.

The benchmark's frozen suite commitment (`897908b5da8f32a39ee6e2b8b54f3f2021c303ad12d9427eba8b695f28441309`) is distinct from the byte-level SHA-256 of the exported `suite_manifest.json` review-bundle file (`20e70ac010205b0167e42e73afef26963e198be9624154c6e58c77ecdb35f9ca`). Both are preserved in provenance rather than treated as interchangeable hashes.

## Publication boundary

The review bundle explicitly excludes raw prompts, raw model answers, private artifacts, credentials, customer data, and raw runtime exceptions. This public package also excludes the supplied full `baseline.json` because it contains internal configuration and prompt material unnecessary for reproducing the published metrics.

This result should be interpreted only as a documented engineering regression checkpoint for the frozen TK-54 suite and this recorded KESPA runtime state. It is not a direct replacement for the older 423-claim V1 A/B/C condition and does not establish broad model superiority or generalization.
