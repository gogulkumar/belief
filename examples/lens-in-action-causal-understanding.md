# Lens In Action — Causal Understanding

## Context

This example spans three documents — Cycles 3, 4, and 5. The causal
lens works differently from the other lenses: a single document rarely
produces a causal belief. The lens watches for the pairing of a signal
with a lagged outcome, which requires reading across time.

---

## Document 1 (Cycle 3 — Mid-Year Review)

> "Customer onboarding completion rate declined to 61% in Q2, down
> from 74% in Q1. The decline is attributed to a product update that
> introduced additional setup steps. Engineering is addressing this
> in Q3."

**Causal lens reading:**

A signal has appeared: onboarding completion rate dropped 13 points.
This is a leading candidate — if customers are not completing setup,
they are likely to churn before they get value. But this is hypothesis,
not a causal belief. No lagging outcome is visible yet.

The document says engineering is addressing it. The signal may reverse.

**→ SILENT.** File onboarding completion decline in Factual Understanding
as a tracked signal (FU-C01). Note the potential causal hypothesis:
"onboarding decline may precede churn increase by 1–2 periods." Watch
for churn data in Cycles 4 and 5.

---

## Document 2 (Cycle 4 — Quarterly Review)

> "Onboarding completion rate recovered to 69% in Q3, partially
> recovering from Q2's decline. Churn in Q3 increased to 4.2%,
> up from 3.1% in Q2. Management noted that Q3 churn reflected
> 'customers who experienced friction earlier in the year.'"

**Causal lens reading:**

Two things visible here:
1. Onboarding completion recovering (signal reversing — partially)
2. Churn increasing — management explicitly linking it to "earlier
   friction," which matches the Q2 onboarding decline

The management attribution is not enough to form a causal belief on
its own. But it confirms the hypothesis is at least plausible to the
organization itself.

More importantly: the pattern appeared. Onboarding declined in Q2.
Churn rose in Q3. The lag is approximately one period.

Is this one observation of the relationship? Yes. But it is the first
time both the signal and the lagged outcome are visible. This is enough
to seed at 0.20.

— Falsifiable? Yes. If onboarding declines again and churn does not
  rise in the following period, the relationship fails.
— Specific lag? Approximately one period (Q2 signal → Q3 outcome).
— Count: 1.

**→ SEED CU-01**: "A decline in onboarding completion rate precedes
a churn increase by approximately one period. Observed once (Q2 decline
→ Q3 churn rise). Management explicitly linked Q3 churn to earlier
onboarding friction."
Confidence 0.20. Evidence count 1. Direction: Deteriorating.
Status: One observation — needs recurrence.

**→ FLAG**: Onboarding completion at 69% in Q3 — still below the 74%
baseline. If onboarding remains below baseline in Q4, watch for churn
impact in Cycle 5.

---

## Document 3 (Cycle 5 — Annual Results)

> "Full-year churn came in at 4.8%, above the 3.5% plan, with the
> majority of the increase concentrated in customers acquired in Q1
> and Q2. Onboarding completion for Q4 returned to 76%, the highest
> rate in six quarters. Management expects churn to normalize in H1
> of next year as the onboarding improvements take effect across the
> cohort."

**Causal lens reading:**

Two things to evaluate:

1. **Full-year churn elevated (4.8% vs 3.5% plan)** with concentration
   in Q1–Q2 cohorts. The Q2 onboarding decline affected those cohorts.
   The lagged outcome from the Q2 signal has now fully materialized.
   This is a second confirmation of the relationship — the Q2 onboarding
   decline (signal) → elevated churn in the back half of the year
   (outcome), lag approximately one to two periods.

2. **Q4 onboarding returned to 76%** — management predicts churn
   normalization in H1 next year. This is a testable forward claim
   created by the causal belief. If onboarding improvement leads to
   churn improvement one to two periods later, H1 next year should
   show churn declining. File this as a pending confirmation.

— The relationship has now held twice in observable form (Q2 decline
  → churn; Q4 recovery → predicted churn improvement pending).
— Count: 2 confirmed, 1 pending.

**→ UPDATE CU-01**: Confidence 0.20 → 0.28. Evidence count 1 → 2.
Direction: Unclear (direction of the underlying metric is improving,
but the causal relationship itself is neither good nor bad — it is
a map, not a result).

Updated statement: "A meaningful change in onboarding completion rate
precedes a corresponding change in churn by approximately one to two
periods. Observed twice: Q2 decline preceded elevated churn in Q3–Q4;
Q4 recovery predicted by management to normalize churn in H1 next year
(pending)."

**→ WATCH ITEM**: Cycle 6 churn data will confirm or fail the Q4
recovery → churn normalization direction of the relationship. If churn
declines in H1, confidence rises to 0.36. If it does not, confidence
drops to 0.13 and the relationship is weakened.

---

## What This Example Shows

1. **A causal belief cannot be seeded from a single document.** Cycle 3
   produced only a tracked signal. The belief required seeing the outcome
   follow the signal in Cycle 4.

2. **Management attribution is corroborating evidence, not the belief.**
   The fact that management linked Q3 churn to Q2 friction made the seed
   more credible, but the belief is in the observable data, not the
   management explanation.

3. **Causal beliefs generate forward watch items.** Once CU-01 exists,
   it changes how you read future documents. Cycle 6 is now a test of
   the relationship in the improving direction, not just a routine update.

4. **Confidence is appropriately low at two observations.** 0.28 is not
   enough to act on predictively in isolation. It needs three to four
   more confirming cycles before it is reliable enough to use as a
   forward signal in decision-making.
