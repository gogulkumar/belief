# World Model Schema

The world model (L1) is the living belief model. It is the file an agent loads at session start. It is surgically updated — never rewritten from scratch.

## File Structure

One world model file per active belief type. Example: `world_model/business_memory.md`

```
world_model/
├── business_memory.md
├── business_dynamics.md
├── narrative_understanding.md
├── factual_understanding.md
└── causal_understanding.md
```

## Belief Entry Format

Each belief is one block in its category file. Fields are always present; values update over time.

```markdown
## [BELIEF_ID]

**Statement:** [Durable, falsifiable interpretation of how the business behaves]

**Confidence:** 0.XX
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
## BM-004

**Statement:** Revenue growth is price-driven with volume in mild decline,
monetizing an existing base rather than expanding it.

**Confidence:** 0.52
**Direction:** Deteriorating
**Last Seen:** HCOM_Business_Review_Q2_2026
**First Seen:** HCOM_Business_Review_Q4_2025

**Evidence Trail:**
- Q4 2025 — Revenue +9% on +11% price, volume −1%. First signal.
- Q1 2026 — Pattern repeats. Volume −2%, price +8%. Confirmed.
- Q2 2026 — Volume now −3%, price holding +7%. Direction tightening.

**Contradictions:**
(none)
```

## Causal Belief Entry Format

Causal beliefs carry additional fields for the lead-lag relationship.

```markdown
## CU-001

**Leading Signal:** [what moves first, with threshold if visible]
**Lagging Outcome:** [what follows]
**Observed Lag:** ~N periods
**Confirmed:** N times
**Status:** Active | Pending | Candidate (Factual Understanding)

**Confidence:** 0.XX  (cap: 0.90)
**Direction:** Improving | Stable | Deteriorating | Unclear

**Evidence Trail:**
- [Doc 1] — leading signal appeared
- [Doc 2] — lagging outcome arrived N periods later

**Contradictions:** (if any)
```

## Update Arithmetic

| Action | Confidence Change |
|--------|------------------|
| Confirm | +0.08 |
| Contradict | −0.15 |
| New Prior | initialized at 0.20 |
| Decay (90+ days unseen) | −0.05 per cycle |

**Bounds:** Floor 0.05 · Cap 0.95 (0.90 for causal beliefs)
**Archive threshold:** Below 0.10 → moved to `world_model/archive/`

## World Model Bounds

- **Max active beliefs per type:** ~20
- **Decay trigger:** 90 days since last confirming document (configurable)
- **Archive, not delete:** Contradicted and decayed beliefs are archived, not erased. History is preserved.

## What the Belief Reasoning Agent Reads

Every Prompt 3 (Belief Reasoning) call receives:

1. **The document chunk** — the text being analyzed right now
2. **The current world model file** for the relevant belief type — all active beliefs
3. **The master memory goal** — the user-defined observation angle from Prompt 1

It writes back **only the lines that change**. The rest of the world model is untouched.

## The Ledger

Every ingestion cycle appends one entry to the ledger. The ledger is immutable and append-only.

```markdown
## Ledger Entry — [Timestamp]

**Document:** [filename]
**Document Type:** [pptx / pdf / transcript / etc]
**Period Covered:** [e.g., Q2 2026]
**Belief Types Run:** [list of active belief types]

**Belief Updates:**
- BM-004: Confirmed (+0.08 → 0.52)
- NU-002: Confirmed (+0.08 → 0.74)
- CU-001: New Prior added (0.20) — "Onboarding decline → churn increase, lag ~1 period"

**Decay Pass:**
- No beliefs archived this cycle

**Source Index Entry Written:** Yes (L2)
**Raw Archive Written:** Yes (L3)
```
