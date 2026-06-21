# Evaluation — Belief Reasoning System

*What was assessed, what was found, what was changed, and why.*

---

## The Aim — Restated Clearly

Belief exists to answer one question no current AI system answers:

**How does an AI agent accumulate business understanding over time the way a trained analyst does — so that every interaction it has is informed by what it has learned across months of documents, not just what is in the current session?**

The answer is a belief layer. Not facts. Not summaries. Beliefs — durable, falsifiable interpretations of how a business behaves, held with calibrated confidence, updated as evidence accumulates, and loaded into agent context at every session start.

The five belief type prompts are the engine of this. Each one watches a different dimension of the business. Each one enforces a silence default — most inputs produce no update. Each one makes surgical, not wholesale, changes to the world model. The quality of the beliefs the system produces is entirely a function of how well these prompts are written.

---

## Evaluation Framework

Each prompt was assessed against five criteria:

| Criterion | Question |
|-----------|----------|
| **Belief definition** | Is the belief type genuinely distinct from a fact, a summary, or a metric? |
| **Silence calibration** | Is the gate strict enough? Most inputs should produce no update. |
| **Interpretation anchor** | Does the prompt require the AI to state what a signal *implies*, not just that it exists? |
| **Boundary clarity** | Is the boundary with adjacent belief types stated where overlap is likely? |
| **Falsifiability** | Can every belief this prompt produces be contradicted by a future document? |

---

## Findings by Belief Type

### 01 — Business Memory

Watches six perspectives simultaneously: Business Model & Structure, Business Dynamics, Growth Engine, Margin & Cost Behavior, Capital Allocation, Operating Scale & Leverage. The core risk for this belief type is conflating a result with a structural truth. A revenue miss is a result. "Revenue growth is increasingly price-driven rather than volume-driven" is a business memory belief.

**Gate in place:** The survival test — would this still be true after a bad quarter? If yes, it belongs in Business Memory. If it might not, it belongs in Factual Understanding.

**Boundary with Business Dynamics:** Business Memory holds *what is true* about the structure. Business Dynamics holds *how the parts interact*. A cost structure fact belongs in Business Memory. The relationship between cost growth and revenue growth belongs in Business Dynamics.

### 02 — Business Dynamics

The hardest belief type to apply correctly because causal relationships are easy to fabricate from coincidence. The prompt requires observing a relationship across at least two periods before it becomes a belief.

**Gate in place:** Hypothesis vs belief distinction. If the relationship has appeared once, it is filed in Factual Understanding as a tracked signal. It becomes a Business Dynamics belief only when the pattern recurs.

**Boundary with Causal Understanding:** Business Dynamics holds broad operating behavior — how the business responds to pressure as a system. Causal Understanding holds specific lead-lag pairs with a measurable lag and a confirmed count. "Cost rises slower than revenue when the business scales" is Business Dynamics. "Onboarding completion declining below 65% precedes churn increase by one period" is Causal Understanding.

### 03 — Narrative Understanding

The softest belief type. Language signals are real but require observable anchors — specific words, specific changes, specific omissions. The core failure mode is impression: "management sounded cautious." That is not a belief.

**Gate in place:** Every belief requires a specific, quotable language anchor. A tone shift requires naming what specifically changed — numbers replaced by ranges, timelines extended, commitments becoming directional.

**Boundary with Factual Understanding:** Factual Understanding tracks what was committed to and whether it was met. Narrative Understanding interprets the *pattern of how* commitments are made — the precision, the hedging, the attribution, what is shown and what is omitted.

### 04 — Factual Understanding

Not a belief store — the evidence layer. The intake layer that feeds the other four. Most of what a document contains stays here. When a pattern recurs and an interpretation becomes visible, it moves out of Factual Understanding and into the appropriate belief type.

**Role:** Revenue figures, growth rates, segment data, forward commitments, and signals being tracked all live here. The Forward Commitments Tracker is the most important table — it holds what was said, for what period, and what the current status is.

**What does not stay here:** Interpretations. When something in Factual Understanding has produced a belief in another type, the belief holds the interpretation and the factual record holds the underlying data. They are not duplicated.

### 05 — Causal Understanding

The most specific belief type. Each belief is one lead-lag pair. Both the signal and the outcome must have been observed before a belief is written — a signal with no observed outcome stays in Factual Understanding as a pending candidate.

**Gate in place:** One observation is not a causal belief. A single pairing seeds a candidate. The belief requires at least one more confirming instance. Two consecutive failures call the relationship into question.

**Cap at 0.90:** Causal relationships are probabilistic, not deterministic. The cap enforces this.

**Boundary with Business Dynamics:** See above. The distinction is specificity — a specific measurable signal, a specific measurable outcome, a counted and named lag.

---

## Cross-Cutting Observations

### Silence Is the Most Important Feature

Every belief prompt instructs the agent to stay silent unless a durable interpretation is visible. This is not a weakness — it is the mechanism that keeps confidence meaningful. A world model where every document produces five belief updates across all types is a world model where confidence numbers are noise. The value of a 0.74 belief depends on 90% of inputs producing no update.

### Interpretation Is Required, Not Optional

The core failure mode across multiple belief types is accepting a recurring signal as a belief. A metric that keeps appearing is a recurring fact. The belief is the interpretation: *what does its recurrence imply about the business?* Every belief must state an implication, not just a pattern.

### Factual Understanding Is the Intake Layer, Not the Output

The second most common failure mode is leaving interpretations in Factual Understanding. When an observation has recurred enough to support an interpretation, it should move into the appropriate belief type. Factual Understanding holds evidence. The other four hold understanding.

---

## The Five Belief Types — Final State

| # | Belief Type | Key Gate | Key Risk |
|---|------------|----------|----------|
| 01 | Business Memory | Survival test — would this survive a bad quarter? | Conflating results with structural truths |
| 02 | Business Dynamics | Two-period requirement before a relationship becomes a belief | Fabricating causal relationships from coincidence |
| 03 | Narrative Understanding | Observable language anchor required — no impressions | Vague characterizations of tone without specific evidence |
| 04 | Factual Understanding | Interpretations move out; evidence stays | Leaving interpretations here instead of promoting them |
| 05 | Causal Understanding | Both signal and outcome must be observed; one instance is not enough | Seeding causal beliefs from single observations |
