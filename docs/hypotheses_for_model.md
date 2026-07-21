# Hypotheses for the Visibility Scoring Model - Module 3.2

Five testable hypotheses drawn from the visualizations and correlation analysis, prioritized
for the Project 4 scoring model. Each will be validated with statistical tests next.

## Priority 1: Core Model Components

### H1: Performance to News Visibility (the one real signal)
- **Hypothesis:** Better on-track results (top-5 finishes, leading laps) generate more *news*
  coverage - but not more Reddit/YouTube buzz.
- **Evidence:** Top-5 finishes average **1.31×** the news mentions of P11+ finishes (t = 1.75,
  p = 0.084, marginal). Laps Led to News is the only statistically significant link (r = +0.181,
  p = 0.030, from Module 3.1). Reddit/YouTube show R² ≈ 0.
- **Model implication:** Weight **news** as the primary sponsor-level visibility driver.
  Include a modest **laps-led** contribution. Do **not** weight Reddit/YouTube at the sponsor
  level - they are race-level.

### H2: Win / Top-5 Bonus (discrete, not linear)
- **Hypothesis:** The performance to visibility relationship is a **threshold** effect (wins and
  top-5s), not a smooth linear one.
- **Evidence:** Wins earn **1.21×** the composite visibility of non-wins; the linear
  finish-position correlation is ~0 (r = -0.024). A discrete bonus fits better than a slope.
- **Model implication:** Add a discrete **top-5 / win bonus** rather than weighting raw finish
  position linearly.

## Priority 2: Modifiers and Adjustments

### H3: Playoff Effect - do NOT apply a boost
- **Hypothesis:** Playoff races do **not** boost visibility in this dataset (contrary to the
  usual assumption).
- **Evidence:** Playoff-race composite visibility is **-22.8%** vs the regular season.
- **Model implication:** Do **not** apply a playoff multiplier yet. Investigate the cause
  (likely a Google-Trends/YouTube collection-timing artifact) before trusting this.

### H4: Channel Weighting - news over social
- **Hypothesis:** News is more predictable from performance than Reddit or YouTube.
- **Evidence:** R² of finish position vs each channel - News 0.002, Reddit 0.001, YouTube 0.015
  (all weak; only news + laps-led reaches significance). News is also the only channel that
  varies by sponsor.
- **Model implication:** Weight **News highest** for sponsor-level scoring; treat Reddit/YouTube
  as race-context features (or re-collect them at the sponsor level).

## Priority 3: Sponsor-Specific Factors

### H5: Sponsor Tier / Consistency Baseline
- **Hypothesis:** Better-performing sponsors (NAPA, FedEx) carry a higher baseline visibility
  regardless of a single race.
- **Evidence:** NAPA and FedEx lead both performance (mean finish 11.7, 13.9) and composite
  visibility (65 each); Love's trails on both (P21, 35).
- **Model implication:** Consider a small **team/sponsor baseline offset**, but only if it
  survives testing - the current signal is confounded with news volume.

## Hypotheses to Test in Project 4
1. [ ] H1: Performance to news visibility (regression + top-5 t-test)
2. [ ] H2: Win / top-5 bonus quantification
3. [ ] H3: Playoff effect direction (re-check after fixing collection timing)
4. [ ] H4: Channel weight optimization (news-weighted composite)
5. [ ] H5: Sponsor baseline offset (if it survives controls)

**Primary hypothesis for the model:** H1 - news coverage (driven by top-5 finishes and laps
led) is the most reliable, sponsor-specific visibility signal in this dataset.
