# Submission Format — v0.1

Every submission lives at `submissions/<operator-id>/v0.1.json` and is paired with the raw results file.

## File layout

```
submissions/
└── your-operator-id/
    ├── v0.1.json              ← submission metadata (required)
    ├── v0.1-results.jsonl     ← raw per-claim results (required)
    └── v0.1-receipts.jsonl    ← signed receipts (optional but encouraged)
```

## v0.1.json schema

```json
{
  "operator_id": "string (lowercase, hyphenated, your namespace)",
  "operator_name": "string (human-readable)",
  "operator_url": "string (URL)",
  "submission_version": "v0.1",
  "methodology_version": "v0.1",
  "submission_date": "YYYY-MM-DD",
  "scores": {
    "full_set": 0.0,
    "held_out": 0.0,
    "per_label": {
      "Supported": 0.0,
      "Refuted": 0.0,
      "Not Enough Evidence": 0.0,
      "Conflicting Evidence/Cherrypicking": 0.0
    }
  },
  "calibration": {
    "anchor_dataset": "string",
    "anchor_seed": "string",
    "valid_until": "YYYY-MM-DD"
  },
  "verifier_metadata": {
    "pipeline_version": "string",
    "source_mix": ["string", "..."],
    "adversarial_coverage": "full | partial | none"
  },
  "receipt_format": "v0.3 | other | none",
  "notes": "string (optional, ≤500 chars)"
}
```

## results.jsonl schema

One JSON object per line, per claim:

```json
{
  "claim_id": 0,
  "claim": "...",
  "gold_label": "Supported",
  "predicted_label": "Supported",
  "agentoracle": {
    "verdict_raw": "supported",
    "verdict_mapped": "Supported",
    "recommendation": "confident_supported",
    "confidence_overall": 0.87,
    "confidence_claim": 0.85,
    "adversarial_result": "resilient",
    "adversarial_flags": []
  },
  "sources": ["..."],
  "latency_s": 1.2,
  "error": null,
  "ts": 1748730000
}
```

If your verifier emits a different field structure, you may use any equivalent — but the scoring script (`scripts/score.py` in the reference harness) must run against your file without modification. The simplest path is to emit the canonical shape above.

## receipts.jsonl (optional)

One JWS-encoded receipt per line, one line per claim_id, in the same order as `results.jsonl`. If included, anyone can independently verify the receipts using [`@agentoracle/receipt-verify`](https://github.com/TKCollective/agentoracle-receipt-verify).

## Example complete submission

See [`submissions/tkcollective/v0.1.json`](../submissions/tkcollective/v0.1.json) for the reference baseline submission.
