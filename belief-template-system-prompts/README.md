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
