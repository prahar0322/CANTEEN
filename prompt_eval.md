# Prompt Evaluation Report

## Overall Score

| Parameter | Score |
|---|---:|
| Prompt Clarity | 89 / 100 |
| Output Quality & Schema Guidance | 63 / 100 |
| Efficiency & Token Economy | 28 / 50 |
| **Total** | **180 / 250** |

## Executive Summary

The prompt is a strong operational brief: it defines a five-day pilot, a hard ₹10,000 ceiling, attendance variation, dayparts, a specific menu, and concrete tables and arithmetic checks. Its central weakness is not lack of detail, but lack of a shared accounting model. It asks for planned procurement spending, sold-unit costs, leftovers, waste, reserve cash, and surplus without defining how those figures relate. That makes contradictory but superficially complete answers likely.

The requested output is also very large: 19 sections and 10 tables revisit overlapping quantities. There are no examples of a correctly reconciled day or an explicit rule for handling assumptions that make the budget or service target infeasible. Clarifying cash budget versus cost of goods sold, defining a canonical demand model, and allowing concise cross-referenced tables would improve reliability substantially.

## Evaluated Prompt Analysis

**Target:** the complete prompt in `prompt.md`, evaluated in place. It contains 2,521 words (about 3,500 tokens; tokenizer-dependent), organized into 19 numbered sections. The exact source remains in that file; this report quotes representative wording rather than duplicating the full source.

**Structure:** operating conditions and demand pattern; menu and price rules; cost and demand assumptions; time- and day-based inventory; snack and lunch operations; budget and five-day costing; revenue, profit and waste; demand adjustment and reserve; ten required tables; arithmetic audit; management summary.

**Representative instruction:** “The total budget must be strictly reconciled to ₹10,000.” The prompt also asks for both “planned expenditure” and “Gross surplus/profit,” while requiring that unsold and wasted quantities be estimated. It does not define whether surplus subtracts purchases, sold-unit cost, or waste, or how the cost of carried packaged inventory is treated.

## Detailed Parameter Breakdown

### 1. Prompt Clarity: 89 / 100

| Criterion | Score | Rationale |
|---|---:|---|
| Role & persona definition | 18 / 20 | A canteen manager is the practical audience (“a practical plan that a college canteen manager could actually implement”), but the model's role and decision authority are not stated directly. |
| Task specificity & negative constraints | 23 / 25 | Scope is exceptionally specific, including “Do not prepare the full day's quantity at opening” and “Do not hide unexplained money.” Some requirements compete when the budget cannot support all requested availability. |
| Structure & delimiters | 19 / 20 | Numbered sections, headings, bullets, and named tables make the brief easy to navigate. Dynamic inputs and generated assumptions are not separately delimited. |
| Tone & target audience | 13 / 15 | The practical college-project/canteen-manager audience and direct style are apparent; reading level and desired response length are not specified. |
| Unambiguous language | 16 / 20 | Many rules are measurable, but “approximately,” “where practical,” “adequate,” and “realistic” need operational thresholds. The distinction between a ₹150–₹200 meal price and a student's total lunch spend is not fully pinned down. |
| **Subtotal** | **89 / 100** | |

### 2. Output Quality & Schema Guidance: 63 / 100

| Criterion | Score | Rationale |
|---|---:|---|
| Format & schema enforcement | 28 / 30 | Ten tables have explicit columns and the answer has a required audit and summary. There is no single-source-of-truth or precedence rule for conflicting tables. |
| Few-shot examples | 2 / 25 | The samosa example illustrates batching but is not a complete input/output example demonstrating arithmetic, edge cases, or the required table format. |
| Edge cases & fallback | 19 / 25 | High/normal/low attendance, over/under-demand responses, and reserve triggers are covered. Missing supplier prices, infeasible quantities, stock carried to the next week, and zero/very small denominators in the adjustment formula are not resolved. |
| Factuality & hallucination prevention | 14 / 20 | The prompt correctly calls unknown prices “planning estimates” and requests supplier validation. It does not require source/date/region labeling, distinguish MRP from procurement price, or explicitly prohibit presenting estimates as verified quotations. |
| **Subtotal** | **63 / 100** | |

### 3. Efficiency & Token Economy: 28 / 50

| Criterion | Score | Rationale |
|---|---:|---|
| Conciseness & fluff elimination | 10 / 15 | Repetition reinforces important constraints, but budget, demand, stock availability, and consistency checks recur across sections and tables. |
| Token economy | 7 / 15 | At 2,521 words, the prompt is clear but large. Several time-based, day-based, category, and financial tables ask for the same quantities from different angles. |
| Dynamic parameterization | 4 / 10 | Budget and five-day duration are fixed, and no named placeholders allow the same prompt to be reused for a different college, region, schedule, or supplier price list. |
| Signal-to-noise ratio | 7 / 10 | The priority is clear, but critical accounting definitions are missing while detailed output requirements are repeated. |
| **Subtotal** | **28 / 50** | |

## Actionable Recommendations

1. Define one accounting convention: cash purchases count against the ₹10,000 budget; gross operating surplus uses revenue less the cost of units sold and explicitly written-off waste; usable ending inventory is reported separately and is not silently counted as profit.
2. Preserve the emergency reserve as unspent cash unless an explicitly modeled trigger uses it. Reconcile planned spending plus reserve to the total available budget, and do not describe an allocation as expenditure.
3. Define a single demand calculation: attendance by day type × item purchase rate, adjusted by daypart shares; have each table reference those same totals rather than independently inventing quantities.
4. Require a feasibility correction when the requested menu, stock, and budget cannot all be met. State the resulting service capacity and trade-off instead of forcing implausible prices, margins, or demand.
5. Separate planning estimates from verified local prices; label assumed region/date, supplier cost, taxes/packaging where included, and MRP for packaged goods.
6. Consolidate duplicated quantities and state that all tables must reconcile to one daily ledger. Add one small arithmetic example, including how leftover packaged stock and perishable waste are counted.
7. Add safe batch, storage, FIFO, and end-of-day handling limits, especially for cooked food; do not imply that unsafe leftovers may be resold.

## Optimized Prompt Rewrite (Production-Ready)

```text
<role>
Act as a practical Indian college-canteen operations and costing planner. Produce a numerically reconciled five-day pilot plan for a canteen serving only a controlled subset of the college population.
</role>

<inputs>
<operating_days>5</operating_days>
<weekly_cash_budget_inr>10000</weekly_cash_budget_inr>
<location_or_price_basis>State an assumed Indian city/price basis if none is supplied.</location_or_price_basis>
<supplier_quotes>None supplied; use clearly labeled planning estimates and state that local quotations must replace them.</supplier_quotes>
</inputs>

<rules>
1. State all assumptions before calculations: expected attendance by day, attendance class, operating hours/dayparts, item purchase rates, meal capacity, and how customers/items are counted. Use at least one low-, two normal-, and one high-attendance day; identify the fifth day's class. Do not assume the entire college is served.
2. Model demand as attendance × item purchase rate, then divide item demand among morning, lunch rush, and post-lunch using stated shares. Use the same resulting quantities everywhere in the answer. Forecast must differ by day; capacity may be increased or capped, but explain the cap and expected unmet demand.
3. Keep roughly ₹1,000 (10%) as emergency cash unless a justified alternative is shown. Base planned routine procurement on no more than ₹9,000. Show the full ₹10,000 reconciliation, with reserve as cash held, not ordinary planned spending. Show any reserve use separately and never treat it as guaranteed revenue or spending.
4. Distinguish procurement/cash spending from cost of goods sold (COGS). Cash purchases, including basic disposables and opening stock, must fit the budget. Revenue = units sold × selling price. Report gross operating surplus as revenue minus COGS for sold units and the cost of explicitly written-off waste; show ending usable packaged inventory separately. Do not call revenue minus all opening purchases “profit” when stock remains. State exclusions such as rent, wages, utilities, or tax.
5. For every offered item, provide estimated supplier/production cost per unit, selling price, and contribution per unit (price minus unit cost). For packaged products, respect labeled MRP and distinguish MRP from assumed purchase cost. Mark all unquoted prices as estimates; do not claim they are verified market quotations or invent unsupported precision.
6. Keep main lunch meal prices around ₹150–₹200. Price tea, coffee, snacks, water, and packaged products affordably. Explain whether ₹150–₹200 refers to the meal price, not every customer's total basket.
7. Offer a limited rotating lunch menu, and include tea, coffee, poha, idli-sambar, veg sandwich, samosa, veg roll, veg momos, chips, biscuits, namkeen, 500 ml water, practical 1 L water, packaged juice/juice box, and soft-drink cans. These need not all be offered or prepared fresh every day; label availability and justify conservative/low-demand stock.
8. Prepare fresh food in small batches. State opening quantity, refill trigger and quantity, latest safe/preparation cutoff, expected daypart sales, closing usable stock, and estimated waste. Reduce production on low-attendance days. Keep shelf-stable packaged stock available throughout the day and apply FIFO. Include safe holding/storage guidance; do not recommend selling food held outside safe limits.
9. If demand exceeds forecast, specify a safe, affordable response and what cannot be replenished in time. If demand is below forecast, stop further batches and describe safe markdown/disposal handling. Never assume zero waste.
10. If constraints cannot all be satisfied, preserve food safety and the cash ceiling, state the shortfall and trade-off, and revise the plan to a feasible pilot. Do not inflate margins or hide a deficit.
11. Set next-day adjustment separately for fresh meals, fresh snacks, packaged snacks, and beverages using (actual sales - forecast sales) / forecast sales. Define behavior when forecast is zero, cap each adjustment (for example, at +/-20%), and account for attendance class before changing stock.
</rules>

<required_output>
Start with assumptions, price-basis caveat, and the demand formula. Then provide these compact tables, all reconciled to one daily ledger:
1. Menu and unit economics: item, category, availability, estimated unit cost, selling price, contribution/unit, demand class.
2. Lunch rotation: day, meals offered, price range, meal capacity.
3. Demand and stock by daypart: item, morning/lunch/post-lunch forecast, initial batch/opening stock, refill(s), daily capacity, expected sales, closing stock/waste. Give detailed batch rows for fresh items and concise grouped rows for packaged items.
4. Five-day operating plan: day, attendance class, menu/stock or quantity by category, expected sales, procurement spending.
5. Budget ledger: category, opening purchases, replenishment, total planned cash spending, reserve held, remaining cash; reconcile exactly to ₹10,000 or show a lower spend and its unspent balance.
6. Weekly revenue and surplus: category, units sold, revenue, COGS, waste cost, gross operating surplus. Separately show ending usable packaged inventory and any reserve use.
7. Waste and adjustment rules: expected perishable waste quantity/percentage, packaged-stock treatment, and next-day adjustment thresholds/caps.

End with a short arithmetic audit confirming: daily spending sums to weekly spending; category spending reconciles to the budget; daypart quantities sum to daily totals; units sold do not exceed available stock; revenue equals units sold × price; COGS and waste use the stated unit costs; and surplus follows the defined formula. Finish with a concise manager's summary of service capacity, lunch-peak response, all-day availability, waste controls, and reserve protection.
</required_output>
```
