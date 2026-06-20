# Belief Examples

This folder shows the five belief lenses in action — a document chunk
goes in, the lens reasons through it, and the world model update comes
out. The examples show not just what beliefs look like but how the
reasoning from evidence to belief actually works.

## Files

| File | What It Shows |
|------|--------------|
| [lens-in-action-business-memory.md](lens-in-action-business-memory.md) | Business Memory lens reading a quarterly review — what it extracts, what it ignores, what it adds to the world model |
| [lens-in-action-causal-understanding.md](lens-in-action-causal-understanding.md) | Causal Understanding lens identifying a lead-lag relationship across documents — how a hypothesis becomes a belief |
| [lens-in-action-narrative-understanding.md](lens-in-action-narrative-understanding.md) | Narrative Understanding lens reading the same document as Business Memory — shows how the same text produces different belief types |
| [world-model-snapshot.md](world-model-snapshot.md) | The accumulated world model after six documents — what all five lenses have built together into one belief store |
| [fact-vs-belief.md](fact-vs-belief.md) | The same raw observation filed as a fact (correct) vs held as a belief (wrong) — why the difference matters |

## How To Read These

Each "lens in action" example has three sections:
1. **The document** — what came in (a real excerpt-style chunk)
2. **The reasoning** — how the lens evaluated it against the gate tests
3. **The world model update** — exactly what changed and what stayed silent

The most important thing to notice: silence is the most common output.
Most of what a document contains does not pass the gate. That is correct
behavior, not a failure.
