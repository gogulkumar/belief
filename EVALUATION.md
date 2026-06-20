# Prompt Evaluation — Cortex Belief Reasoning System

*What was assessed, what was found, what was changed, and why.*

---

## The Aim of the Application — Restated Clearly

Cortex exists to answer one question that no current AI system answers:

**How does an AI agent accumulate business understanding over time the way a trained analyst does — so that every interaction it has is informed by what it has learned across months of documents, not just what is in the current session?**

The answer is a belief layer. Not facts. Not summaries. Beliefs — durable, falsifiable interpretations of how a business behaves, held with calibrated confidence, updated as evidence accumulates, and loaded into agent context at every session start.

The 18 reasoning prompts are the engine of this. Each one watches a different dimension of the business. Each one enforces a silence default — most inputs produce no update. Each one makes surgical, not wholesale, changes to the world model. The quality of the beliefs the system produces is entirely a function of how well these prompts are written.

---

## Evaluation Framework

Each prompt was assessed against five criteria:

| Criterion | Question |
|-----------|----------|
| **Belief definition** | Is the belief type genuinely distinct from a fact, a summary, or a metric? |
| **Silence calibration** | Is the gate strict enough? Most inputs should produce no update. |
| **Interpretation anchor** | Does the prompt require the AI to state what a signal *implies*, not just that it exists? |
| **Boundary clarity** | Is the boundary with adjacent lenses stated where overlap is likely? |
| **Audience fit** | Are examples and language appropriate for a general corporate professional, not just a financial analyst? |

---

## Findings and Changes

### 01 — Business Model & Structure
**Before:** Examples referenced Hotels, Air, B2B (Expedia-specific).
**After:** Generalized to enterprise/consumer, segment A/B framing.
**Verdict:** Structurally sound. The survival test ("would this still be true after a bad quarter?") is the right frame. Examples now industry-neutral.

### 02 — Business Dynamics
**Before:** Examples referenced G&A, Hotels. Reasoning section was only two sentences — thinnest in the set.
**After:** Generalized examples. Reasoning section expanded with explicit watch-list: spend that precedes results, operational changes that appear in financials later, customer behavior that precedes commercial outcomes.
**Verdict:** This is the hardest lens to apply correctly because causal relationships are easy to fabricate from coincidence. The two-document requirement and the explicit "hypothesis vs belief" distinction were added.

### 03 — Growth Engine
**Before:** Hotels/Air examples.
**After:** Generalized to product/customer framing. "Price vs volume, acquisition vs retention" is the right decomposition for any business.
**Verdict:** Strong. The restatement that a revenue growth number is never itself a belief — only its mechanism is — is the core filter this lens needs.

### 04 — Cost & Efficiency Behavior
**Before:** "COGS/opex" in example — accounting jargon.
**After:** "Cost per unit served" as the primary signal. "Platform costs" as a generic proxy.
**Verdict:** The relationship-not-level framing is correct. Fixed cost expanding faster than the business can absorb it is the right belief type.

### 05 — Strategic Priorities & Investment *(replaced Capital Allocation)*
**Before:** Capital Allocation — finance/CFO jargon, narrow.
**After:** Watches the gap between what is stated as a priority and where real resources (headcount, budget, executive attention) actually go. Works for any corporate team.
**Verdict:** This is one of the most important lenses in the system. The insight that every organization states priorities but not all organizations fund them is universally true and underobserved.

### 06 — Scale & Organizational Efficiency *(replaced Operating Scale & Leverage)*
**Before:** "Leverage" — accounting-adjacent language.
**After:** "Output per person" as the primary signal. Three-period requirement for conclusions.
**Verdict:** Good. Growth that requires proportional headcount is linear. Growth that requires more is negative leverage. The framing is now clear without accounting language.

### 07 — Metric Movement & Anomaly ⚠️ STRUCTURAL FIX
**Before:** Confused signal with belief. Example 3 produced: "Supplier lead time is emerging as a tracked operational signal" — this is a fact about what appears in a document, not a belief about the business.
**After:** Added explicit distinction: a recurring metric movement is still a fact; the *interpretation of what that movement implies* is the belief. Reasoning section expanded significantly. New example 3 interprets declining onboarding completion as implying retention pressure.
**Verdict:** The most important fix in this evaluation. An AI running this prompt incorrectly would populate the world model with facts dressed as beliefs — high confidence in things that carry no interpretive value.

### 08 — Operating Risk
**Before:** "Cybersecurity +20%" silent example was oddly specific and company-type-dependent.
**After:** Replaced with a clean managed-risk silent case. Core structure sound.
**Verdict:** The "is there a mitigation plan visible?" guardrail is the right filter.

### 09 — Forecast Reliability
**No changes.** The strongest prompt in the BI Signals bucket. The systematic bias framing ("not whether one forecast missed, but what the pattern reveals") is exactly right. The H1/H2 directional bias example is concrete and general.

### 10 — People, Talent & Organizational Health *(replaced Balance Sheet Stress)*
**Before:** Balance Sheet Stress — narrow accounting/treasury signal, not relevant to most corporate professionals.
**After:** Watches organizational capacity, capability gaps, leadership stability, talent patterns. The "at capacity" while new mandates arrive pattern is universally observable and predictive.
**Verdict:** This is the right replacement. People signals are early — they appear before operational impact. They are visible in any business document. They are systematically underweighted by AI systems that focus on financial metrics.

### 11 — Execution Consistency
**Before:** No explicit boundary with lens 05 (Strategic Priorities) or lens 14 (Management Credibility).
**After:** Added explicit boundary: this lens watches whether things that are *already resourced* get done. Lens 05 watches whether things get resourced. Lens 14 watches leadership commitments specifically. The three-lens distinction is now stated in the prompt.
**Verdict:** The "note it, observe it, then believe it" pattern for new initiatives is correct.

### 12 — Early Warning Signals *(replaced Leading & Lagging Indicators)*
**Before:** BI jargon ("leading/lagging"). 
**After:** "Early Warning Signals" — plain language. "What to watch so that future documents can be read with calibrated attention" is the right purpose statement.
**Verdict:** Strong. The requirement to state the lead time ("by approximately two periods") is correct — a vague "before" is not useful.

### 13 — Narrative vs Reality
**No changes.** Best prompt in the Narrative bucket. The "efficiency ratio metric disappeared from decks as growth missed plan" example is the clearest signal-to-belief demonstration in the entire library.

### 14 — Management Credibility
**Before:** Overlapped with lens 11 and 13 without boundary. "Margin roadmap" jargon in example.
**After:** Explicit boundary stated: lens 11 is organizational execution patterns; lens 14 is specific named commitments made by leadership. Example rewritten to avoid finance jargon.
**Verdict:** The distinction between "guidance that resets instead of recovers" and a single miss is the core filter. Three-observation requirement is correct given the stakes.

### 15 — External Attribution
**No changes.** The "miss period vs beat period" framing is the sharpest analytical frame in the Narrative bucket. The guardrail against characterizing intent while describing the pattern is correct.

### 16 — Emphasis & Omission
**Before:** "Free-cash-flow" in example — finance-specific. No boundary with lens 13.
**After:** Example generalized to "customer satisfaction metric." Explicit boundary with lens 13 added: lens 13 is whether the *language about results is accurate*; lens 16 is whether the *right results are even being shown*.
**Verdict:** The "introduced when favorable, dropped when it turns" pattern is observable in any organization's reporting. Good lens.

### 17 — Tone & Confidence Shift ⚠️ STRUCTURAL FIX
**Before:** Produced vague, impressionistic beliefs. "The CEO sounded cautious" is not a belief. General tone assessment will always be at low confidence and direction Unclear. At that state, it adds noise not signal.
**After:** Required the belief to be anchored to *specific, observable language changes* — not impressions. Added concrete watch-list: numbers replaced by ranges replaced by phrases, time horizon extending, specific commitments becoming directional. "What specifically changed" is now required for a belief to form.
**Verdict:** Tone shifts are genuinely predictive — they appear before numbers change. But they must be grounded in observable language evidence, not felt impressions. The prompt now enforces this.

### 18 — Competitive Positioning Narrative ⚠️ STRUCTURAL FIX
**Before:** Entirely language-dependent. "Market leader" → "a leading player" is the kind of signal that requires enormous document volume to be meaningful. Language-only competitive beliefs will always be at low confidence.
**After:** Required a behavioral anchor alongside any language signal. Added concrete behavioral watch-list: sales motion shifting from acquisition to retention, expansion pausing or reversing, pricing becoming defensive. A belief requires at least one language signal AND one behavioral/metric signal. Language alone can be noted but not believed.
**Verdict:** The new example 3 (international "foundation-building" language with headcount reductions in those markets) shows what a strong competitive belief looks like — language and behavior diverging. That divergence is the signal.

---

## Cross-Cutting Observations

### The Silence Default Is the Most Important Feature
Every prompt instructs the agent to stay silent unless a durable interpretation is visible. This is not a weakness — it is the mechanism that keeps confidence meaningful. A world model where every document produces 18 belief updates is a world model where confidence numbers are noise. The value of a 0.74 confidence belief depends on 90% of inputs producing no update.

### Interpretation Is Required, Not Optional
The core failure mode across multiple prompts was accepting a "recurring signal" as a belief. A metric that keeps appearing is a recurring fact. The belief is the interpretation: *what does its recurrence imply about the business?* Every belief in the system must state an implication, not just a pattern.

### Boundary Clarity Across Adjacent Lenses
Three pairs of lenses risk producing duplicate beliefs without explicit boundary guidance:
- Lens 05 (Strategic Priorities) and Lens 11 (Execution Consistency): resources going to the wrong places vs. funded things not completing
- Lens 13 (Narrative vs Reality) and Lens 16 (Emphasis & Omission): inaccurate framing vs. selective disclosure
- Lens 11 (Execution Consistency) and Lens 14 (Management Credibility): organizational patterns vs. specific leadership commitments

All three boundary pairs are now stated within the relevant prompts.

### Audience Fit
The full set now avoids: EBITDA, COGS, opex, margin, leverage (in the financial sense), free cash flow, capital allocation, balance sheet, covenant — all of which are meaningful only to financial professionals. The replacement language works for anyone who reads business documents in any corporate function.

---

## The 18 Lenses — Final State

| # | Lens | Status |
|---|------|--------|
| 01 | Business Model & Structure | Revised — generalized examples |
| 02 | Business Dynamics | Revised — expanded reasoning, generalized examples |
| 03 | Growth Engine | Revised — generalized examples |
| 04 | Cost & Efficiency Behavior | Revised — removed accounting jargon |
| 05 | Strategic Priorities & Investment | New — replaced Capital Allocation |
| 06 | Scale & Organizational Efficiency | Revised — renamed, cleaner language |
| 07 | Metric Movement & Anomaly | Structural fix — interpretation now required |
| 08 | Operating Risk | Revised — cleaner examples |
| 09 | Forecast Reliability | Unchanged — strongest in BI Signals |
| 10 | People, Talent & Org Health | New — replaced Balance Sheet Stress |
| 11 | Execution Consistency | Revised — boundary with 05 and 14 added |
| 12 | Early Warning Signals | New — replaced Leading & Lagging Indicators |
| 13 | Narrative vs Reality | Unchanged — strongest in Narrative |
| 14 | Management Credibility | Revised — boundary with 11 and 13 added |
| 15 | External Attribution | Unchanged — sharp and specific |
| 16 | Emphasis & Omission | Revised — boundary with 13 added, example generalized |
| 17 | Tone & Confidence Shift | Structural fix — observable language anchor required |
| 18 | Competitive Positioning Narrative | Structural fix — behavioral anchor now required |
