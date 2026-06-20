# World Model Schema

The world model (L1) is the living belief model. It is the file an agent loads at session start. It is surgically updated — never rewritten from scratch.

## File Structure

One world model file per active belief category. Example: `world_model/growth_engine.md`

```
world_model/
├── business_model_structure.md
├── business_dynamics.md
├── growth_engine.md
├── margin_cost_behavior.md
├── capital_allocation.md
├── operating_scale_leverage.md
├── metric_movement_anomaly.md
├── operating_risk.md
├── forecast_reliability.md
├── balance_sheet_stress.md
├── execution_consistency.md
├── leading_lagging_indicators.md
├── narrative_vs_reality.md
├── management_credibility.md
├── external_attribution.md
├── emphasis_omission.md
├── tone_confidence_shift.md
└── competitive_positioning_narrative.md
```

## Belief Entry Format

Each belief is one block in its category file. Fields are always present; values update over time.

```markdown
## [BELIEF_ID]

**Statement:** [Durable, falsifiable interpretation of how the business behaves]

**Confidence:** 0.XX
**Evidence:** N documents
**Direction:** Improving | Stable | Deteriorating | Unclear
**Last Seen:** [Document name or date of most recent confirming document]
**First Seen:** [Document name or date of originating document]

**Evidence Trail:**
- [Doc 1] — [brief note on what it showed]
- [Doc 2] — [brief note on what it showed]

**Contradictions:** (if any)
- [Doc N] — [brief note on what challenged this belief]
```

### Example

```markdown
## GE-004

**Statement:** Air growth is now entirely price-driven with volume in mild decline,
monetizing an existing demand base rather than expanding it.

**Confidence:** 0.52
**Evidence:** 3 documents
**Direction:** Deteriorating
**Last Seen:** HCOM_Business_Review_Q2_2026
**First Seen:** HCOM_Business_Review_Q4_2025

**Evidence Trail:**
- Q4 2025 — Air revenue +9% on +11% ticket price, volume −1%. First signal.
- Q1 2026 — Pattern repeats. Volume −2%, price +8%. Confirmed.
- Q2 2026 — Volume now −3%, price holding +7%. Direction tightening.

**Contradictions:**
(none)
```

## Update Arithmetic

| Action | Confidence Change | Evidence Change |
|--------|------------------|-----------------|
| Confirm | +0.08 | +1 |
| Contradict | −0.15 | no change (contradiction note appended) |
| New Prior | initialized at 0.20 | starts at 1 |
| Decay (90+ days unseen) | −0.05 per cycle | no change |

**Bounds:** Floor 0.05 · Cap 0.95
**Archive threshold:** Below 0.10 → moved to `world_model/archive/`

## World Model Bounds

- **Max active beliefs per category:** ~50–100
- **Total active beliefs across all 18 lenses:** configurable via `credo_config.yaml`
- **Decay trigger:** 90 days since last confirming document (configurable)
- **Archive, not delete:** Contradicted and decayed beliefs are archived, not erased. History is preserved.

## What the Belief Reasoning Agent Reads

Every Prompt 3 (Belief Reasoning) call receives:

1. **The document chunk** — the text being analyzed right now
2. **The current world model file** for the relevant lens — all active beliefs
3. **The master memory goal** — the user-defined observation angle from Prompt 1

It writes back **only the lines that change**. The rest of the world model is untouched.

## The Ledger

Every ingestion cycle appends one entry to the ledger. The ledger is immutable and append-only.

```markdown
## Ledger Entry — [Timestamp]

**Document:** [filename]
**Document Type:** [pptx / pdf / transcript / etc]
**Period Covered:** [e.g., Q2 2026]
**Categories Run:** [list of active lenses]

**Belief Updates:**
- GE-004: Confirmed (+0.08 → 0.52, ev 3)
- FR-002: Confirmed (+0.08 → 0.80, ev 6)
- NR-007: New Prior added (0.20, ev 1) — "Efficiency ratio metric disappeared from decks as growth missed plan"

**Decay Pass:**
- No beliefs archived this cycle

**Source Index Entry Written:** Yes (L2)
**Raw Archive Written:** Yes (L3)
```
