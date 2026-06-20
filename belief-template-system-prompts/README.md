# Belief Reasoning Lenses

Belief builds up through five lenses. Every document is read through each
lens. The lenses do not maintain separate memories — they all write into
one world model. The lens is how you look. The world model is what you
accumulate.

## The Five Lenses

| # | Lens | File | What It Asks |
|---|------|------|-------------|
| 01 | Business Memory | [01-business-memory.md](01-business-memory.md) | What is structurally and durably true about how this business is built? |
| 02 | Business Dynamics | [02-business-dynamics.md](02-business-dynamics.md) | How does the business behave as a system — where does pressure build, where does it release? |
| 03 | Narrative Understanding | [03-narrative-capturing.md](03-narrative-capturing.md) | How does the organization tell its story, and how does that story relate to what the numbers show? |
| 04 | Factual Understanding | [04-factual-understanding.md](04-factual-understanding.md) | What has been quantitatively observed, and which observations are building toward a belief? |
| 05 | Causal Understanding | [05-causal-understanding.md](05-causal-understanding.md) | Which signals reliably predict which outcomes in this business, and with what lag? |

## How The Lenses Differ

**Business Memory** — slowest to change. Structural truths about how the
business is built. Updated rarely; high confidence when confirmed.

**Business Dynamics** — how the business operates as a system. How costs
behave under pressure. Where efficiency builds or breaks. Causal in nature
but broad — the operating logic of the business.

**Causal Understanding** — specific lead-lag pairs. Signal A predicts
outcome B by approximately N periods. This is the lens that generates
forward watch items: when this leading signal appears, flag it and watch
for the lagged outcome.

**Narrative Understanding** — the softest lens. Language, framing, what
is shown and what is omitted. Requires observable anchors — specific
language changes, not impressions. Lower confidence ceiling than the
other lenses unless corroborated by factual signals.

**Factual Understanding** — the intake layer. Not a belief store but an
evidence layer. Most observations stay here as tracked signals. When a
pattern recurs and an interpretation becomes visible, it moves into the
appropriate belief lens.

## What All Five Lenses Share

Every belief, regardless of which lens forms it, must pass:

1. **Falsifiability** — can a future document contradict this?
2. **Distinctiveness** — is this specific to this business, not any
   business in this sector?
3. **Interpretation** — does this say what the evidence means, not
   just what it is?

Silence is the default. Most of what a document contains does not
become a belief. The lenses are gates, not funnels.

## Update Arithmetic (All Lenses)

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

[examples/](../examples/) — five lens-in-action examples showing a
document chunk going in, the reasoning trace, and the world model update
coming out. The best way to understand how the system works is to read
these before reading the lens prompts.
