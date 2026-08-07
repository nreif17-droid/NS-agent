# Substance Safety Screening — Risk Stratification Before Any Tapering Support

**New file, added 2026-08-06.** Triggered by a product-scope conversation naming "trying to quit drinking" as a target audience alongside nicotine/cannabis tapering. This file exists because that framing is **not safe to treat the same way** — it's the operational counterpart to the safety contrast already stated in `dysregulation-causes.md` §1.9 and `craving-management.md` §"Safety/framing." Read this before building, marketing, or extending any feature toward alcohol (or benzodiazepine) cessation.

> Source-quality key: 🟢 strong 🟡 moderate 🔴 weak/fringe — see `foundations.md`.

## 1. The core distinction, stated as plainly as possible

Nicotine and cannabis withdrawal are documented as uncomfortable but **not medically dangerous** (`dysregulation-causes.md` §1.9). **Alcohol (and benzodiazepine) withdrawal can be medically dangerous, including fatal, and this is not a fringe or edge-case risk** — it's a well-characterized, named clinical syndrome with real incidence rates:

- Withdrawal seizure risk peaks at 12–24 hours post-last-drink, occurring in roughly 3% of withdrawal cases. 🟢
- Delirium tremens (DTs) — the most severe withdrawal presentation — typically develops 48–96 hours after the last drink, occurs in roughly 5% of withdrawal cases, and carries a mortality rate of **1–4% even with treatment, and up to 15% if untreated.** 🟢
- The single strongest predictor of a dangerous withdrawal isn't how much someone drinks in the abstract — it's **history of prior withdrawal seizures or DTs**, which predicts future risk more strongly than current consumption volume alone. 🟢

([Alcohol Withdrawal Syndrome — StatPearls/NCBI](https://www.ncbi.nlm.nih.gov/books/NBK441882/), [AFP outpatient management guidance](https://www.aafp.org/pubs/afp/issues/2021/0900/p253.html), [risk factors for severe withdrawal — PMC11922238](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11922238/), [Cleveland Clinic — DTs mortality](https://my.clevelandclinic.org/health/diseases/25052-delirium-tremens))

**This agent's/product's stance:** an app built on this knowledge base must never position itself as a tool for helping someone stop drinking when they show signs of physical dependence. It can support general nervous-system regulation *alongside* medically supervised care, but it cannot be the primary or sole tool for alcohol cessation the way it reasonably can be for nicotine or cannabis.

## 2. Risk factors for a dangerous withdrawal (screening criteria)

Beyond the single strongest predictor (prior withdrawal seizure/DT history, §1), documented risk factors include:

- Heavy daily use: rough clinical benchmarks put men at meaningful risk around 8+ standard drinks/day sustained for a month, with a roughly 50/50 chance of major life-threatening withdrawal around 13+ drinks/day; women's thresholds run lower (~6 drinks/day for meaningful minor-withdrawal risk), reflecting body-weight and metabolism differences. 🟡 (these are rough clinical heuristics from review/patient-education sources, not a single precise RCT-derived cutoff — treat as directional, not exact)
- Age >65
- Comorbid illness, dehydration, electrolyte imbalance, abnormal liver function
- No abstinent days in the past month
- Co-use of benzodiazepines or other GABAergic substances
- Multiple prior withdrawal episodes (even without seizure/DT history)

([Risk factor synthesis — PMC11922238](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11922238/), [withdrawal amount/duration heuristics — clinical patient-education sources](https://www.thefreedomcenter.com/how-much-do-you-have-to-drink-to-get-alcohol-withdrawal-symptoms/) 🟡 lower-tier source, included only for the directional heuristic, not as a precise clinical cutoff)

## 3. A validated, brief screening instrument already exists — use it, don't invent one

**AUDIT-C** (Alcohol Use Disorders Identification Test – Concise): a validated 3-question screen (frequency of drinking, typical quantity per occasion, frequency of heavy-drinking episodes), scored 0–12. Positive screen: **≥4 for men, ≥3 for women.** Developed at WHO's request, extensively validated as a primary-care screening tool for hazardous drinking and active alcohol use disorder. 🟢 ([original validation — PubMed](https://pubmed.ncbi.nlm.nih.gov/9738608/), [WHO AUDIT guidelines](https://www.who.int/publications/i/item/WHO-MSD-MSB-01.6a))

**Severity classification once dependence is suspected:** CIWA-Ar (Clinical Institute Withdrawal Assessment for Alcohol, revised) is the standard clinical instrument — scores ≤8 mild, 8–15 moderate, >15 severe. This is a *clinical* assessment tool (typically administered by/for medical staff during monitored withdrawal), not something to build into consumer-app self-assessment — its presence here is to establish that severity stratification is a solved, standardized problem clinically, reinforcing why this app should route to that existing infrastructure rather than improvise its own. 🟢 ([CIWA-Ar scoring — StatPearls](https://www.ncbi.nlm.nih.gov/books/NBK441882/))

## 4. Operational rule for any product built on this knowledge base

1. **Any "quit drinking" framing, feature, or marketing copy requires an AUDIT-C (or equivalent validated screen) gate before onboarding into regulation content, full stop.**
2. **AUDIT-C positive (≥4 men / ≥3 women), OR any self-reported history of prior withdrawal seizures/DTs, OR heavy daily use (per §2 heuristics) → hard route to "talk to a doctor before stopping or cutting back," not into the app's self-regulation flow.** This is a safety-flag-list addition (see `README.md`), not a soft suggestion.
3. **Below AUDIT-C threshold and no dependence risk factors** (i.e., someone genuinely just moderating light/social drinking) — general nervous-system regulation content is reasonably in-scope, same tier as any other lifestyle-change support.
4. **Never present unsupervised alcohol cessation as something this app can help someone accomplish**, even implicitly through marketing copy or feature framing, if there's any signal of dependence. This is the line between "supportive tool alongside medical care" and "tool that could plausibly contribute to someone attempting a dangerous unsupervised detox."

## Flagged claims summary

- 🟡 The specific "8 drinks/day for a month" / "13 drinks/day = 50/50 major withdrawal" figures are directional clinical heuristics from patient-education-tier sources, not a single precise RCT-derived threshold — don't present them as exact cutoffs in product copy; use AUDIT-C (a validated instrument) as the actual operational gate, not these numbers.
- 🟢 Prior withdrawal seizure/DT history as the single strongest risk predictor — well-supported, should be weighted more heavily than consumption-volume self-report alone in any screening logic.

## Sources consulted

- [Alcohol Withdrawal Syndrome — StatPearls/NCBI Bookshelf](https://www.ncbi.nlm.nih.gov/books/NBK441882/)
- [Alcohol Withdrawal Syndrome: Outpatient Management — American Family Physician](https://www.aafp.org/pubs/afp/issues/2021/0900/p253.html)
- [Risk factors for severe alcohol withdrawal, prospective study — PMC11922238](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11922238/)
- [Delirium tremens overview and mortality — Cleveland Clinic](https://my.clevelandclinic.org/health/diseases/25052-delirium-tremens)
- [AUDIT-C original validation study — PubMed](https://pubmed.ncbi.nlm.nih.gov/9738608/)
- [WHO AUDIT guidelines for primary care use](https://www.who.int/publications/i/item/WHO-MSD-MSB-01.6a)

*Last research pass: 2026-08-06.*
