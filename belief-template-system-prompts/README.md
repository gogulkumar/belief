# Belief Reasoning System Prompts — Full Library

Eighteen detailed belief reasoning agents, organized under three broad parent buckets. The three buckets are what a user picks first. The sub-lenses are the specific belief memories that live inside each bucket. Every prompt shares the same action structure — gate, silence default, surgical update, direction field, guardrails — and differs only in observation focus and examples.

Every belief carries a direction — **Improving / Stable / Deteriorating / Unclear** — and the standard update arithmetic: confirm +0.08, contradict −0.15, new prior 0.20, cap 0.95, floor 0.05.

---

## BUSINESS MEMORY — what is durably true about how the business operates

| # | Lens | File |
|---|------|------|
| 01 | Business Model & Structure | [01-business-model-and-structure.md](01-business-memory/01-business-model-and-structure.md) |
| 02 | Business Dynamics | [02-business-dynamics.md](01-business-memory/02-business-dynamics.md) |
| 03 | Growth Engine | [03-growth-engine.md](01-business-memory/03-growth-engine.md) |
| 04 | Cost & Efficiency Behavior | [04-cost-and-efficiency-behavior.md](01-business-memory/04-cost-and-efficiency-behavior.md) |
| 05 | Strategic Priorities & Investment | [05-strategic-priorities-and-investment.md](01-business-memory/05-strategic-priorities-and-investment.md) |
| 06 | Scale & Organizational Efficiency | [06-scale-and-organizational-efficiency.md](01-business-memory/06-scale-and-organizational-efficiency.md) |

## BI SIGNALS — what is moving, anomalous, or recurring right now

| # | Lens | File |
|---|------|------|
| 07 | Metric Movement & Anomaly | [07-metric-movement-and-anomaly.md](02-bi-signals/07-metric-movement-and-anomaly.md) |
| 08 | Operating Risk | [08-operating-risk.md](02-bi-signals/08-operating-risk.md) |
| 09 | Forecast Reliability | [09-forecast-reliability.md](02-bi-signals/09-forecast-reliability.md) |
| 10 | People, Talent & Organizational Health | [10-people-talent-and-organizational-health.md](02-bi-signals/10-people-talent-and-organizational-health.md) |
| 11 | Execution Consistency | [11-execution-consistency.md](02-bi-signals/11-execution-consistency.md) |
| 12 | Early Warning Signals | [12-early-warning-signals.md](02-bi-signals/12-early-warning-signals.md) |

## NARRATIVE / STORYLINE — how the story is told and where it diverges from numbers

| # | Lens | File |
|---|------|------|
| 13 | Narrative vs Reality | [13-narrative-vs-reality.md](03-narrative-storyline/13-narrative-vs-reality.md) |
| 14 | Management Credibility | [14-management-credibility.md](03-narrative-storyline/14-management-credibility.md) |
| 15 | External Attribution | [15-external-attribution.md](03-narrative-storyline/15-external-attribution.md) |
| 16 | Emphasis & Omission | [16-emphasis-and-omission.md](03-narrative-storyline/16-emphasis-and-omission.md) |
| 17 | Tone & Confidence Shift | [17-tone-and-confidence-shift.md](03-narrative-storyline/17-tone-and-confidence-shift.md) |
| 18 | Competitive Positioning Narrative | [18-competitive-positioning-narrative.md](03-narrative-storyline/18-competitive-positioning-narrative.md) |

---

## How The Library Is Used

A user picks a parent bucket first — Business Memory, BI Signals, or Narrative. Inside that bucket they pick or are guided to a sub-lens, or the questioning agent maps their described focus to the closest one. The chosen prompt's identity paragraph and examples are then adapted to the user's specific business, document type, and cadence captured in setup.

The action structure — gate, silence default, surgical update, direction field, guardrails — is identical across all eighteen. Only the observation focus and examples differ. That is what keeps the system consistent while letting each belief memory reason about exactly the right thing.
