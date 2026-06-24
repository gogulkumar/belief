# Belief System Prompts

Five belief types. Every document is read through all five. Each belief
type asks a different question. All five write into one world model —
there is no separate memory per belief type. The belief type tells you
how to read. The world model is what accumulates.

## The Five Beliefs

| # | Belief Type | File | What It Asks |
|---|-------------|------|-------------|
| 01 | Business Memory | [01-business-memory.md](01-business-memory.md) | What is structurally and durably true about how this business is built? |
| 02 | Business Dynamics | [02-business-dynamics.md](02-business-dynamics.md) | How does the business behave as a system — where does pressure build, where does it release? |
| 03 | Narrative Understanding | [03-narrative-capturing.md](03-narrative-capturing.md) | How does the organization tell its story, and how does that compare to what the numbers show? |
| 04 | Factual Understanding | [04-factual-understanding.md](04-factual-understanding.md) | What has been quantitatively observed, and which observations are building toward a belief? |
| 05 | Causal Understanding | [05-causal-understanding.md](05-causal-understanding.md) | Which signals reliably predict which outcomes in this business, and with what lag? |

## How The Five Differ

**Business Memory** — slowest to change. Structural truths about how the
business is built. Updated rarely; high confidence when confirmed.

**Business Dynamics** — how the business operates as a system. How costs
behave under pressure. Where efficiency builds or breaks. Causal in nature
but broad — the operating logic of the business.

**Causal Understanding** — specific lead-lag pairs. Signal A predicts
outcome B by approximately N periods. The only belief type that generates
forward watch items: when this leading signal appears, flag it and watch
for the lagged outcome.

**Narrative Understanding** — the softest belief. Language, framing, what
is shown and what is omitted. Requires observable anchors — specific
language changes, not impressions. Lower confidence ceiling unless
corroborated by factual signals.

**Factual Understanding** — the intake layer. Not a belief store but an
evidence layer. Most observations stay here as tracked signals. When a
pattern recurs and an interpretation becomes visible, it moves into the
appropriate belief type.

---

## Structured Rules by Belief Type

Each belief type has three non-negotiable rules that govern what a valid belief looks like within it.

### 01 — Business Memory

| Rule | Specification |
|------|--------------|
| **Numbers policy** | No raw period totals. Pattern language only — "third consecutive period", "consistently N% below plan". Numbers describe trajectory, not magnitude. |
| **Pattern form** | Cross-period behavioral recurrence. What the business does repeatedly vs what happened once. Language the documents themselves use to mark recurrence ("again", "as seen in prior periods", "consistent with"). |
| **Durability test** | Would a senior analyst hold this interpretation after reading 3 more documents from this entity — regardless of what those documents say about this specific period? |

### 02 — Business Dynamics

| Rule | Specification |
|------|--------------|
| **Numbers policy** | Structural ratios and efficiency percentages only. No period-specific totals. The ratio is the belief; the period value is evidence. |
| **Pattern form** | Structural ratio stability across periods. Whether the mechanics are strengthening or breaking. What the trajectory predicts about the next period's ratio. |
| **Durability test** | Is this mechanic true regardless of which specific reporting period is being reviewed? If it only holds in one period, it is not a dynamics belief. |

### 03 — Narrative Understanding

| Rule | Specification |
|------|--------------|
| **Numbers policy** | Zero numbers. Entirely about language, sequencing, emphasis, framing choices, and what is present vs absent. |
| **Pattern form** | Linguistic fingerprints: specific phrases that recur, structural positions (what leads, what is buried), sequencing habits (how bad news is ordered), what categories get featured vs omitted. Requires observable anchors — not impressions. |
| **Durability test** | Has this framing pattern appeared in 2+ documents independently? Is it a deliberate communication strategy, or a one-time rhetorical choice forced by that period's results? |

### 04 — Factual Understanding

| Rule | Specification |
|------|--------------|
| **Numbers policy** | Numbers ARE the belief. Every entry must cite metric, value, period, and the benchmark comparison used (e.g., vs prior period, vs plan). |
| **Pattern form** | Metric definition stability — does this metric appear under the same definition across documents? Benchmark comparison consistency — does the comparison base shift, and when? |
| **Durability test** | Does this metric appear under the same definition in every document of this type? If the definition or comparison base shifts, that shift is itself a factual belief worth capturing. |

### 05 — Causal Understanding

| Rule | Specification |
|------|--------------|
| **Numbers policy** | Magnitude only to describe scale of the lead or the lag. No full period totals. The lead-lag relationship is the belief; the magnitude is context. |
| **Pattern form** | One lead-lag pair per belief. A leading signal, a lagging outcome, the observed lag length, and how many times this sequence has been confirmed. |
| **Durability test** | Have both the leading signal AND the lagging outcome been observed at least once? A candidate causal belief (signal seen, outcome not yet observed) lives in Factual Understanding until confirmed. Cap confidence at 0.90 — causal beliefs are probabilistic, not deterministic. |

## What All Five Share

Every belief, regardless of which type forms it, must pass:

1. **Falsifiability** — can a future document contradict this?
2. **Distinctiveness** — is this specific to this business, not any
   business in this sector?
3. **Interpretation** — does this say what the evidence means, not
   just what it is?

Silence is the default. Most of what a document contains does not
become a belief. The gate is primary.

## Update Arithmetic

| Event | Change |
|-------|--------|
| First observation (seed) | 0.20 |
| Confirmed | +0.08 |
| Contradicted | −0.15 |
| No signal / 90 days | −0.05 decay |
| Cap | 0.95 (0.90 for causal beliefs) |
| Floor | 0.05 |

Every belief carries a direction: **Improving / Stable / Deteriorating / Unclear**

## See Also

[examples/](../examples/) — three examples showing the belief system in
action: a document read and beliefs formed, one belief built across six
documents, and fact vs belief compared.
