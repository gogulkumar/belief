# Ingestion Pipeline

How a document enters Belief and becomes belief updates in the world model.

## The Seven-Task Sequence

### Task 1 — Identify Document Type

**Input:** Raw file
**Output:** Transcription + document type label

The file is inspected and routed to the correct extraction method:

| Document Type | Extraction Method |
|--------------|-------------------|
| PPTX / image-heavy PDF | Vision (slide-by-slide extraction) |
| Scanned PDF | OCR |
| Text PDF / MD / TXT | Direct extract |
| Audio / video transcript | Transcription → text |

The raw extract is written to **L3 (Raw Archive)**. This is the only time L3 is written. It is never reprocessed.

---

### Task 2 — Select Belief Types

**Input:** `belief_config.yaml` → `belief_types_active` list
**Output:** Ordered list of belief type prompts to run

The system loads the system prompts for all active belief types. Typically all five run, but the config allows selective activation for performance or focus.

---

### Task 3 — Chunk the Transcription

**Input:** Raw transcription from Task 1
**Output:** Ordered list of chunks with sequence numbers

- Default chunk size: 50,000 tokens (configurable)
- Chunks respect sentence boundaries — no mid-sentence splits
- Documents under the token limit: single-pass (no chunking)
- Documents over the token limit: chunk loop

---

### Task 4 — Belief Extraction Loop

**Input:** Chunks × active belief types × current world model
**Output:** Temp belief per active belief type

For each belief type, for each chunk:

```
Chunk 1:
  temp_belief[type] = empty
  → run belief reasoning prompt
  → dump all outputs directly into temp_belief[type]

Chunk 2+:
  → run belief reasoning prompt against chunk
  → compare outputs against temp_belief[type]
  → resolve: confirm / contradict / merge / new / redundant / drifting
  → update temp_belief[type]
```

The belief reasoning prompt (Prompt 3) receives:
- The document chunk
- The current world model for this belief type
- The master memory goal

It applies the fact-to-belief gate and makes surgical updates only.

---

### Task 5 — Temp Belief Maintenance

**Input:** Temp belief (post-chunk outputs)
**Output:** Clean temp belief ready for next chunk or finalization

Resolves within-document conflicts before they reach the world model:

| State | Definition | Resolution |
|-------|-----------|------------|
| Overlapping | Two chunks produced the same belief in different words | Merge into one |
| Redundant | A chunk produced a belief already held at high confidence | Discard |
| Drifting | Similar to but subtly different from an existing temp belief | Flag, suggest merge or split |
| Contradicting | A chunk challenges a belief produced by an earlier chunk of the same document | Hold both, flag tension |
| New | No match in the temp set | Add it |

---

### Task 6 — Document-Level Belief Finalization

**Input:** Clean temp belief per belief type
**Output:** Final document-level belief file per belief type

After all chunks are processed, the temp belief is reviewed for coherence. Redundancies are collapsed. Contradictions are marked. The result is the single document-level belief file that will be merged against the world model.

---

### Task 7 — Merge Into World Model

**Input:** Document-level belief + current world model (L1)
**Output:** Updated world model + ledger entry

For each belief in the document-level file:
- Find the matching belief in the world model (by semantic match, not exact string)
- Apply the appropriate update state: Confirm, Contradict, New Prior
- Write the ledger entry
- Run the decay pass (any belief not seen in 90+ days loses 0.05 confidence; archived if below 0.10)
- Write the updated L2 source index entry for this document

---

## Token Budget Considerations

The belief reasoning pass (Task 4) is the most expensive step. Each chunk × each belief type × each pass = one LLM call. For a 200k-token document running all five belief types:

- Chunks: 4 (at 50k each)
- Belief types: 5
- Calls: ~20 reasoning passes + maintenance passes

The config allows selective belief type activation to reduce cost when only specific dimensions are needed.

## Failure Recovery

- **L3 is immutable**: if processing fails mid-pipeline, the raw extract is preserved. Reprocessing restarts from Task 2.
- **Temp belief is ephemeral**: discarded after merge. No state to recover between runs.
- **World model is surgical**: a failed merge does not corrupt existing beliefs. The pre-merge state is unchanged until the write completes.
- **Ledger is append-only**: no entry is written until Task 7 completes successfully. An absent ledger entry = pipeline did not complete for that document.

---

## Belief Lifecycle — How A Belief Evolves Over Time

A belief is not static. Each new document that arrives is an opportunity to confirm, contradict, or leave unchanged every belief in the world model. The lifecycle rules govern what happens to a belief after it is written.

**Confirming a belief:** When a later document shows the same mechanism independently — not in the same words, not from the same source, but from a different period or a different angle — the belief earns more confidence. The update is written as the story continuing: name the new document, name what it showed, say what it adds to the existing understanding.

**Revising a belief:** When the direction reverses or the mechanism changes, the belief is revised — not retired. The prior understanding is preserved as history; the current understanding is updated to reflect what is now true. A belief whose direction has reversed is different from a belief that has been contradicted.

**Retiring a belief:** When multiple documents contradict a belief, it is retired to the archive. The archive preserves the record of what was believed and when — it is not deleted. A retired belief is available as historical context if the pattern re-emerges.

**Decaying a belief:** A belief not seen in 90 days loses 0.05 confidence per decay cycle. This is not contradiction — it is the absence of evidence. The world moves on. A belief the documents have stopped supporting should not be held at the same confidence as one the documents keep confirming.

These four states — confirm, revise, retire, decay — are managed by Prompt 4 (Decay & Maintenance) after every ingestion cycle, not by the belief reasoning prompts themselves. The belief reasoning prompts only answer one question: what does this document say to the existing belief right now?
