# AgentOracle Benchmark — Cross-Operator Verification Suite

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)
[![Methodology v0.1](https://img.shields.io/badge/methodology-v0.1-1f6feb)](./methodology/v0.1.md)
[![Open Submissions](https://img.shields.io/badge/submissions-open-success)](#submitting-results)

**An open, reproducible benchmark for AI claim-verification systems.**

Pre-action verification is becoming a category. Multiple operators are building primitives that ask the same question — *given a factual claim, with what verdict and confidence should an agent be allowed to act?* — using different pipelines, source mixes, and calibration anchors. Today there's no shared way to compare them on the same input.

This repository is the public methodology, the reference harness, and the submissions registry.

## Status

| Component | Status | Notes |
|---|---|---|
| Methodology v0.1 | ✅ Live | AVeriTeC-based reference test set, fixed seed, reproducible |
| Reference harness | ✅ MIT-licensed | [TKCollective/agentoracle-eval-harness](https://github.com/TKCollective/agentoracle-eval-harness) |
| Submission format | ✅ Live | See [methodology/submission-format.md](./methodology/submission-format.md) |
| Submissions registry | 🟡 Open | First baseline filed by TKCollective (57.6% / 57.7%) |
| v0.2 methodology | 🔄 Drafting | Multi-modal (image, audio, code) extension |

## Why this exists

A verification primitive that can't be benchmarked across operators isn't infrastructure — it's a single-vendor claim. The category needs a public methodology so:

- Operators can show calibration discipline (anchor dataset, seed, scoring rubric all in the open)
- Buyers can compare on the same axis
- Drift can be detected over time
- New entrants don't waste cycles inventing methodology — they reuse it

This isn't a leaderboard. It's a reproducibility floor.

## Current registered submissions

| Operator | Methodology | Full Set | Held-Out | Notes |
|---|---|---|---|---|
| [TKCollective / AgentOracle](https://agentoracle.co) | v0.1 | 57.6% | 57.7% | Reference baseline, harness MIT licensed |
| (your submission here) | v0.1 | — | — | See submission process below |

## Submitting results

We welcome submissions from any operator building verification primitives. The methodology is designed to be implementation-agnostic — your pipeline, your sources, your calibration, our test set and scoring.

### Submission process

1. Fork this repo.
2. Run the reference harness (or your own harness conforming to the methodology) against the v0.1 test set.
3. Add your submission as `submissions/<operator-id>/v0.1.json`. See [methodology/submission-format.md](./methodology/submission-format.md) for the schema.
4. Include your raw `results.jsonl` so the score is independently verifiable.
5. Open a PR with title `submission: <operator-id> v0.1`.
6. Submission is reviewed for methodology conformance (NOT for accuracy — your accuracy is your accuracy). Merged when format is valid.

### Submission format (summary)

```json
{
  "operator_id": "your-operator-id",
  "submission_version": "v0.1",
  "methodology_version": "v0.1",
  "scores": {
    "full_set": 0.576,
    "held_out": 0.577
  },
  "calibration": {
    "anchor_dataset": "averitec-dev-2024-q3",
    "anchor_seed": "...",
    "valid_until": "2026-11-30"
  },
  "verifier_metadata": {
    "pipeline_version": "...",
    "source_mix": [...],
    "submission_date": "2026-06-01"
  }
}
```

## Methodology v0.1

Defined in [methodology/v0.1.md](./methodology/v0.1.md). Headlines:

- **Test set:** AVeriTeC dev split (500 claims), held-out subset of 100
- **Verdict mapping:** `verdict_raw` ∈ {supported, refuted, unverifiable} → AVeriTeC labels; `vulnerable` adversarial → `Conflicting Evidence/Cherrypicking`
- **Scoring:** Per-label accuracy plus overall held-out accuracy
- **Calibration discipline:** anchor dataset + seed + valid-until period must be declared in every submission
- **Drift:** submissions older than calibration valid_until are tagged "stale" but remain in registry for historical comparison

The full normative methodology is in the linked document. The reference harness is open-source and MIT-licensed. There is no proprietary scoring code.

## Related work

This benchmark is positioned alongside, not competitively against, related work in the agent verification space:

- **[AgentOracle receipt spec v0.3](https://github.com/TKCollective/agentoracle-receipt-spec/tree/v0.3-binary-halt)** — the signed receipt format that every benchmark submission's verifier ideally emits
- **[@agentoracle/receipt-verify](https://github.com/TKCollective/agentoracle-receipt-verify)** — open-source offline verifier client for the receipt format
- **AVeriTeC** (Schlichtkrull et al., 2024) — source dataset
- **Anthropic Zero Trust for AI Agents** (May 2026) — names benchmark-based verification accuracy under the explainability tier
- **EU AI Act Article 12** — record-keeping for high-risk AI systems, applicable August 2, 2026

## Governance

This benchmark is operated by TKCollective with the explicit intent of becoming a community-owned reference. Proposals to evolve the methodology (v0.2, v0.3 …) go through public RFC in this repository. Substantial revisions ship as versioned upgrades; submissions remain valid against the version they were made under.

If you're an operator building in this space and want to co-steward the v0.2 methodology development — open an issue. The bar to participate is shipping a submission.

## License

MIT. Use, fork, copy, vendor in. The methodology is meant to be a floor for the field, not a wall around it.

## References

- AgentOracle: [agentoracle.co](https://agentoracle.co)
- Receipt spec v0.3: [github.com/TKCollective/agentoracle-receipt-spec](https://github.com/TKCollective/agentoracle-receipt-spec/tree/v0.3-binary-halt)
- Reference harness: [github.com/TKCollective/agentoracle-eval-harness](https://github.com/TKCollective/agentoracle-eval-harness)
- Verifier client: [github.com/TKCollective/agentoracle-receipt-verify](https://github.com/TKCollective/agentoracle-receipt-verify)
