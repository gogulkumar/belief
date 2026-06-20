# Belief Reasoning Lenses

Belief is not built by running 18 separate agents. It is built by reading
every document through four broad lenses, each asking a different kind of
question about what the business reveals. The lenses do not maintain
separate memories — they all write into one world model.

## The Four Lenses

| Lens | File | What It Asks |
|------|------|-------------|
| Business Memory | [01-business-memory.md](01-business-memory.md) | What is structurally and durably true about how this business is built? |
| Business Dynamics | [02-business-dynamics.md](02-business-dynamics.md) | How does one part of this business move another — what causes what, and with what lag? |
| Narrative Capturing | [03-narrative-capturing.md](03-narrative-capturing.md) | How does the organization tell its story, and how does that story relate to what the numbers show? |
| Factual Understanding | [04-factual-understanding.md](04-factual-understanding.md) | What has been quantitatively observed, and which observations are building toward a belief? |

## How The Lenses Relate

**Business Memory** holds the slowest-moving beliefs — structural truths
about how the business is built that survive bad quarters and leadership
changes. Updated rarely. High stakes.

**Business Dynamics** holds causal beliefs — how one signal predicts
another in this specific business. Requires observing the relationship
across multiple periods before it earns belief status.

**Narrative Capturing** holds the softest beliefs — interpretations of
how the organization communicates, what it emphasizes and omits, and
whether stated commitments match outcomes. Lower natural confidence
ceiling than the other lenses; needs corroboration from factual signals.

**Factual Understanding** is different from the other three — it is the
intake layer. Most observations stay here as tracked signals. Only when
a pattern recurs and an interpretation becomes visible does something
move from this lens into a belief in one of the other three.

## What The Lenses Share

Every belief formed through any lens must pass the same three tests:

1. **Falsifiability** — can a future document contradict this?
2. **Distinctiveness** — is this specific to this business or true of
   any business in this sector?
3. **Interpretation** — is this what the evidence means, not just what
   it says?

Silence is the default. Most of what a document contains does not become
a belief. The lenses are gates, not funnels.

## Update Arithmetic (All Lenses)

| Event | Confidence Change |
|-------|-----------------|
| Confirmed | +0.08 |
| Contradicted | −0.15 |
| New (first seed) | 0.20 |
| No signal / 90 days | −0.05 decay |
| Cap | 0.95 |
| Floor | 0.05 |

Every belief also carries a direction: **Improving / Stable / Deteriorating / Unclear**

## See Also

- [examples/](../examples/) — what the world model looks like in practice,
  how a belief evolves across six documents, and how facts differ from beliefs
- [world-model/schema.md](../world-model/schema.md) — the structure of
  the belief store that all four lenses write into
