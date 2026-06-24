# Strategic Blueprint — Layer 1 of the Belief System

The Strategic Blueprint is the master configuration document for one belief stream. It is produced once, from the user's setup session, and is read by every downstream step — both compilers and all execution passes.

---

## What the Blueprint Is

The blueprint is not a template with placeholders. It is an answered document. A setup agent reads the user's inputs, reasons through specific questions about this entity and angle, and produces explicit answers — concrete answers about this specific business, this specific scope, this specific document cadence.

**The blueprint determines everything below it.** Quality injected here propagates through both compilers (Layer 2) into every belief produced at runtime (Layer 3). A generic blueprint produces generic beliefs. A precise, entity-specific blueprint produces entity-specific beliefs from the first document.

---

## The Four Sections

### Section 1 — Belief Stream Identity

The identity record. Downstream steps read this to know who they are working for.

- Entity name and organizational context
- Belief stream name and purpose statement
- Observation angle (the type of understanding this stream accumulates)
- Business scope (what part of the entity this stream covers)
- Document types that will be ingested
- Document cadence (weekly / monthly / quarterly / ad hoc)

---

### Section 2 — Document Types and Signal Matrix

For each document type this stream will receive:

| Field | What It Contains |
|-------|-----------------|
| **What it is** | The document's purpose and when it arrives |
| **What it CAN carry** | Which signals are reliably present for this angle |
| **What it CANNOT give you** | Signals this document cannot support, no matter how carefully read — prevents invented facts |
| **Trigger question** | The single question this document type helps answer for this angle |

The "cannot give you" column is as important as the "can carry" column. It is what prevents the belief engine from filling gaps with plausible-sounding fabrications.

---

### Section 3 — Belief Definition for This Stream

This is the entity-specific, angle-specific belief definition. Not the generic angle rules — those are generic. This section answers: what does a belief in this angle look like for THIS entity, with THESE documents, at THIS cadence?

The blueprint generator must answer these questions explicitly:

1. **Worked example belief** — write one belief entry using this entity's actual vocabulary, document structure, and metric definitions. If the entity doesn't have any documents yet, base it on the stated scope.
2. **Worked non-belief** — write one entry that feels like a belief but fails the test. Explain specifically why it fails.
3. **Durability standard** — for this entity's cadence, how many documents constitute "Confirmed"? Monthly documents: 2 months. Daily documents: perhaps 10 days. Weekly: 3 weeks. Make it explicit.
4. **Pattern form** — what form do patterns take for this entity in this angle? Language recurrence? Structural choices? Attribution habits? Sequencing behavior? Metric definition choices? Be specific.
5. **What these documents cannot give you** — for this angle, what is unknowable from the available document types, no matter what they say?
6. **Forward signal standard** — what must the next document show for the current beliefs to be considered holding?

**Why this section is critical:** Without it, both compilers must re-derive the belief definition from generic angle rules and invent entity-specific examples that may not match the entity's actual language. With Section 3, the compilers read and encode the entity-specific definition directly. Beliefs become entity-grounded from document one.

---

### Section 4 — Named Watch Areas

Each watch area is a category the belief stream tracks. Every belief entry in the world model corresponds to one watch area.

For each watch area, the blueprint must specify:

- **What it tracks** — in entity-specific terms, not generic category labels
- **Pattern form** — what form patterns take within this watch area (language, structure, sequencing, or behavioral markers)
- **Forward signal** — what the next document needs to show for this area to hold
- **Pattern break signal** — what absence or change would constitute a signal worth flagging

Watch areas map directly to `##` sections in the world model file. The watch area list defines the belief scope. The belief engine will not accumulate beliefs outside the named areas.

---

## The Angle Is a Hypothesis, Not a Constraint

The angle stated in the blueprint is the user's starting hypothesis based on what they know about their documents and purpose. The execution prompt compiler (Layer 2A) tests whether that hypothesis matches what the documents can actually carry.

If the documents cannot support the stated angle, the compiler names the mismatch and either refines the angle or notes that this document type produces thin signal for the stated purpose. This is by design — the blueprint is the hypothesis; the compiler is the reality check.

Possible mismatches:
- User stated Narrative Understanding, but the documents are raw data exports with no commentary — the compiler flags this as low-signal for that angle and may suggest Factual Understanding instead
- User described a scope that maps to a new angle not in the five standard types — the compiler defines it from first principles using Section 3's framework
- The scope supports two distinct angles — the compiler notes this and may produce two separate execution prompts

---

## How the Blueprint Flows Into the Compilers

```
Blueprint
   │
   ├──► Execution Prompt Compiler (Layer 2A)
   │    Reads: Section 1 (identity), Section 3 (belief definition), Section 4 (watch areas)
   │    Produces: belief_reasoning_prompt.md
   │    The execution prompt is self-contained — it carries everything the
   │    belief engine needs at runtime, derived from the blueprint.
   │
   └──► Fact Extraction Prompt Compiler (Layer 2B)
        Reads: Section 2 (signal matrix), Section 3 (pattern form), Section 4 (watch areas)
        Produces: fact_extractor_prompt.md (per document type)
        The manifest is angle-scoped and watch-area-aligned — it captures
        only signals the belief engine can use, in the form it needs them.
```

The compilers are generic. The blueprint is what makes their output entity-specific. No code changes are required to support a new entity, a new angle, or a new document type — only a new blueprint.
