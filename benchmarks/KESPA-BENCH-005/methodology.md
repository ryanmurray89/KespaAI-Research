# Methodology

## Benchmark role

`KESPA-BENCH-005` publishes the completed TK-54 engineering regression checkpoint represented by run `1229d2a0-5473-426d-899c-bda596dc5e7a`.

The source protocol defines TK-54 as a deterministic 54-case suite generated from active, public, indexed Trusted Knowledge QA records. Its purpose is production regression detection for the adaptive KESPA Brain, not automatic creation of a scientific or generalization claim.

## Frozen inputs

The review material records:

- Suite UUID: `d8d945ae-6879-4c1a-bf67-13895c585859`
- Suite version: `2026-09-22`
- Cases: `54`
- Frozen suite commitment: `897908b5da8f32a39ee6e2b8b54f3f2021c303ad12d9427eba8b695f28441309`
- Baseline UUID: `51740386-7f9c-47d3-958b-5cab95eaece5`
- Baseline label: `KESPA V2.01 post-multimodal baseline`
- Baseline manifest SHA-256: `c94cdc0226582d4b4c3dfc55f625a5877b94bd83812f3e2594b274fef0e82efe`
- Runtime config SHA-256: `f570257df712ecbf4740c82ab6bf96c7f3621d2d52f5058a371c7e5e394821fe`
- Knowledge cards: `500`

The protocol explicitly warns that current production with 500 cards is a new V2.01 condition and must not be silently substituted for the older 423-claim V1 condition when claiming direct A/B/C comparability.

## Execution

The benchmark was run with:

- purpose: `regression`
- condition: `adaptive`
- model route: `local-primary`
- Brain version/build: `1.1.1-dev` / `dev-013-trusted-knowledge-index`
- external benchmark judge: off / not recorded

All 54 cases completed. Per-case rows are preserved in `results.csv` without raw prompts or raw model answers.

## Deterministic scoring

The protocol's judge-free regression path records deterministic measures including lexical F1, required-term recall, deterministic quality, deterministic task success, latency, TTFT, throughput, token usage, RAG use, escalation, GPU energy, quality/sec, and quality/Wh where available.

For this run:

- `mean_judge_score` is null.
- `retrieval_hit_rate` is null.
- `routing_match_rate` is null.

Those nulls are preserved. No substitute values were inferred.

## Result interpretation

`failed_cases = 0` describes execution completion. Separately, deterministic task success was 50/54; 4 cases failed the deterministic success criterion.

The benchmark should therefore be read as:

1. a complete execution of the frozen TK-54 regression suite;
2. a measured deterministic quality/efficiency checkpoint;
3. a regression baseline for future KESPA engineering comparisons;
4. not an external-judge evaluation and not a claim of broad generalization.

## Privacy / publication handling

The supplied review bundle states that raw prompts, raw answers, private artifacts, credentials, customer data, and raw runtime exceptions were omitted. This public package retains only safe metrics, hashes/commitments, run identifiers, and benchmark methodology.

The full supplied `baseline.json` is intentionally not republished because it includes internal configuration and prompt material beyond what is required to support the benchmark result.
