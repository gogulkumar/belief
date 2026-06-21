# The Four-Prompt Architecture

Belief runs on four prompts that form a closed loop. None are static templates. They are generated from the user's setup conversation and adapt to the document type and business domain.

---

## Prompt 1 — Goal Alignment

**Runs:** Once, at system setup
**Pattern:** ReAct interview loop — one question at a time

### What It Discovers

| Question Domain | Why It Matters |
|----------------|----------------|
| What business is this? | Belief reasoning is domain-specific |
| What documents will arrive? | Routes ingestion method and calibrates extraction |
| Which belief types are active? | Determines which of the five types run on each document |
| What is the noise? | Teaches the gate to silence known irrelevant signals |
| What is the wrong-belief cost? | Calibrates how conservative the gate should be |

### What It Produces

The **master memory goal** — a compact, structured statement of what this instance of Belief is trying to understand. This file is loaded into context for every subsequent Prompt 3 pass.

**Example master memory goal:**
```
Business: Hotels.com (HCOM) within Expedia Group
Documents: Monthly business review decks, quarterly transcripts, planning documents
Active belief types: All five
Observation angle: Understand how the business actually behaves vs how it is narrated.
Watch for structural cost behavior, forecast reliability patterns, and tone shifts.
Known noise: Currency effects already disclosed separately. Seasonal hotel patterns well understood.
Wrong-belief cost: High. Beliefs feed into ODIN SQL calibration and exec-level reporting.
```

---

## Prompt 2 — Document Intake

**Runs:** On every document upload
**Pattern:** Inspect → route → extract → index

### What It Does

1. Inspects the file type
2. Routes to vision (pptx/image), OCR (scanned), or direct extract (text/md)
3. Executes the extraction
4. Writes raw extract to **L3** (immutable, never reprocessed)
5. Writes the source index entry to **L2**: period covered, key themes, notable metrics, narrative signals, anomalies

### The L2 Index Entry

The index is what allows future retrieval without scanning raw documents. When an agent needs evidence for a belief, it queries L2 to find candidate documents, then retrieves the relevant section from L3.

```
Source Index Entry:
  document: HCOM_Business_Review_Q2_2026.pptx
  period: Q2 2026
  document_type: business_review_deck
  key_themes: [Hotels recovery, Air pricing pressure, cost discipline]
  metrics_present: [revenue, EBITDA, margin, headcount, ADR, occupancy]
  narrative_signals: [CFO cautious on H2, competitive language softer]
  anomalies: [efficiency ratio absent for second consecutive deck]
  raw_archive_path: l3/HCOM_Business_Review_Q2_2026/
```

---

## Prompt 3 — Belief Reasoning

**Runs:** For every chunk × every active belief type
**This is the five belief type system prompts in this repository**

### What It Receives

1. The document chunk (≤50k tokens)
2. The current world model file for this belief type (all active beliefs)
3. The master memory goal from Prompt 1

### What It Does

1. Applies the **fact-to-belief gate** to the chunk
2. For each observation that passes the gate:
   - Find the matching belief in the world model
   - Determine update state: Confirm / Contradict / New Prior / Silent
   - Make the surgical update
3. Writes the ledger contribution for this chunk

### The Silence Default

The most important instruction in every Prompt 3 is: **stay silent unless a durable interpretation is visible.** Most chunks pass through with zero updates. This is not a failure. This is the gate working correctly.

### Why Five Separate Prompts

A single monolithic prompt watching all five dimensions simultaneously produces diluted, generic beliefs. Separate prompts allow each belief type to:
- Maintain its own silence threshold
- Apply its own guardrails (narrative is the softest signal and carries lower confidence; causal is capped at 0.90)
- Use domain-specific examples and worked illustrations
- Reason deeply about one dimension rather than shallowly about all

---

## Prompt 4 — Decay & Maintenance

**Runs:** After every ingestion cycle
**Pattern:** Audit → decay → archive → report

### What It Checks

1. Scans the world model for beliefs not seen in 90+ days (configurable)
2. Applies confidence decay: −0.05 per cycle for each unseen belief
3. Archives beliefs that fall below 0.10 (moves to `world_model/archive/`)
4. Reports which beliefs are in volatility (confidence dropped significantly in recent cycles)
5. Ensures world model stays bounded

### Why Decay Matters

Without decay, stale beliefs accumulate indefinitely. A business changes — a pattern that was anomalous becomes normal, a structural truth resolves. Decay forces the world model to reflect current reality. A belief that was 0.75 and has not been seen in a year should not still be 0.75.

Decay does not erase. It reduces confidence until the belief is archived. If the pattern returns, a new prior is created — and the archive provides the historical record.

---

## The Closed Loop

```
Prompt 1 ──► Master Memory Goal
                     │
                     ▼
Prompt 2 ──► L3 Raw Archive + L2 Source Index
                     │
                     ▼
Prompt 3 ──► Surgical world model updates (×5 belief types ×N chunks)
                     │
                     ▼
Prompt 4 ──► Decay pass + bounded world model
                     │
                     ▼
             Updated World Model ──► loaded at next agent session start
```

The loop is closed: every new document feeds the world model; the world model makes every subsequent agent query smarter.
