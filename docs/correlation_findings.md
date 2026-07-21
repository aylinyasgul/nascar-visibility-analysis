# Correlation Findings — NASCAR Sponsorship Visibility

**Project:** NY Racing sponsor visibility scoring model
**Module:** 3.1 — Correlation Analysis (Week 4)
**Prepared for:** Sarah Mitchell
**Dataset:** 2024 NASCAR Cup Series · 4 sponsors × 36 races = 144 observations
**Sponsors:** FedEx (Denny Hamlin), NAPA Auto Parts, McDonald's (Bubba Wallace), Love's Travel Stops (Michael McDowell)

> **Data note:** The `Reddit_Mentions` column is a **Google Trends** weekly search-interest
> score (0–100), used as the social-buzz proxy because Reddit's public API returned HTTP 403
> during collection (see data dictionary). Google Trends interest is a national, race-period
> signal — it is the same for all four sponsors in a race — which is central to the
> granularity caveat below.

---

## Executive Summary

I tested whether on-track performance (finish position, laps led) is linearly related to
off-track visibility (Reddit mentions, YouTube views, news mentions). **Of the six key
performance→visibility relationships, only one is statistically significant:**
**Laps Led → News Mentions** (r = +0.181, p = 0.030), a *weak positive* relationship.

Finish position — the metric I expected to drive visibility most strongly — shows **no
statistically significant linear correlation with any exposure channel** at the pooled
level. The single most important reason is a **data-granularity issue**: Reddit and YouTube
metrics were collected at the **race level** (identical for all four sponsors within a race),
so by construction they cannot explain sponsor-to-sponsor differences in the same race.
This is the headline caveat that must shape how we build — and how much we trust — the
scoring model.

---

## Question 1 — Which performance metrics correlate most strongly with visibility?

Pearson correlation matrix (all pairs):

|                  | Finish_Position | Laps_Led | Reddit | YouTube | News |
|------------------|:---:|:---:|:---:|:---:|:---:|
| **Finish_Position** | 1.00 | -0.21 | -0.02 | +0.12 | -0.05 |
| **Laps_Led**        | -0.21 | 1.00 | -0.03 | -0.04 | +0.18 |
| **Reddit_Mentions** | -0.02 | -0.03 | 1.00 | +0.06 | +0.02 |
| **YouTube_Views**   | +0.12 | -0.04 | +0.06 | 1.00 | +0.32 |
| **News_Mentions**   | -0.05 | +0.18 | +0.02 | +0.32 | 1.00 |

**Ranking of performance → visibility links by strength:**

1. **Laps Led → News Mentions: r = +0.181** (weak positive) — the strongest and only
   significant link. Leading laps puts a driver into race recaps and headlines.
2. Finish Position → YouTube Views: r = +0.124 (weak, *wrong direction*, not significant).
3. Finish Position → News Mentions: r = -0.048 (negligible).
4. Laps Led → YouTube: r = -0.041 (negligible).
5. Laps Led → Reddit: r = -0.032 (negligible).
6. Finish Position → Reddit: r = -0.024 (negligible).

> Note: the strongest correlation anywhere in the matrix is **YouTube ↔ News (+0.32)** — but
> that is exposure-to-exposure (both rise for marquee races), not performance-to-visibility,
> so it does not help weight the model.

---

## Question 2 — Are those correlations statistically significant, or just noise?

Significance key: `***` p<0.01 · `**` p<0.05 · `*` p<0.10 · (blank) not significant.

| Relationship | Correlation (r) | P-Value | Significance | Interpretation | Implication for Model |
|---|:---:|:---:|:---:|---|---|
| Finish Position vs Reddit | -0.024 | 0.773 | — | No meaningful relationship | Do **not** weight |
| Finish Position vs YouTube | +0.124 | 0.139 | — | Weak positive (wrong direction) | Do **not** weight |
| Finish Position vs News | -0.048 | 0.567 | — | No meaningful relationship | Do **not** weight |
| Laps Led vs Reddit | -0.032 | 0.705 | — | No meaningful relationship | Do **not** weight |
| Laps Led vs YouTube | -0.041 | 0.626 | — | No meaningful relationship | Do **not** weight |
| **Laps Led vs News** | **+0.181** | **0.030** | **\*\*** | **Weak positive** | **Include as a modest weight** |

**Verdict:** Five of six relationships are statistical noise (p ≥ 0.14). Only **Laps Led →
News Mentions** clears the p < 0.05 bar, and even that is *weak* in magnitude (r ≈ 0.18,
explaining ~3% of variance). We do **not** yet have strong quantitative evidence that finish
position drives visibility — which is a meaningful result in itself.

---

## Question 3 — What surprised me?

**Surprise 1 — Better finishes did NOT generate more social buzz.**
I expected a clear *negative* correlation between finish position and Reddit/YouTube (better
finish = lower number = more buzz). Instead the correlations are essentially zero
(-0.024 and +0.124). The expected relationship simply is not visible in the pooled data.

**Surprise 2 — YouTube views correlate *positively* with finish position (+0.124).**
Taken at face value this says *worse* finishes come with *more* YouTube views. The likely
explanation is that YouTube_Views is a **race-level** number driven by marquee/crash races
(Daytona, Talladega) that draw huge highlight viewership regardless of how our specific
drivers finished. It reflects race prominence, not sponsor performance.

**Surprise 3 — News, the sparsest channel, produced the only real signal.**
I expected Reddit (the most race-focused community) to be the strongest visibility channel.
Instead the only significant performance link runs through **news mentions**, and via *laps
led* rather than *finish position*. Leading laps is what gets a driver named in write-ups.

**Surprise 4 — Laps led beat finish position as a visibility predictor.**
The module hinted the reverse ("laps led may show weaker correlations than finish position").
Here laps led is the *only* performance metric with a significant exposure link.

---

## Confounding Factors

The correlations above are almost certainly shaped by variables we did not fully isolate:

| Confounder | How it affects results | Mitigation / next step |
|---|---|---|
| **Metric granularity (biggest issue)** | Reddit_Mentions (a Google Trends proxy) and YouTube_Views are identical for all 4 sponsors within a race (race-level, not sponsor-level). They cannot distinguish which sponsor drove the buzz, washing out finish-position correlations. | Re-collect or re-key Reddit/YouTube to the driver/sponsor level before trusting these channels in the model. |
| **Marquee-race dominance** | Daytona (season opener, big crashes) and superspeedways draw disproportionate YouTube/Reddit volume regardless of finish. Daytona is the "highest exposure" race for every sponsor. | Add a race-prominence or superspeedway control; consider per-race normalization. |
| **Crashes / DNFs** | A bad finish in a dramatic wreck can *generate* buzz, pushing the finish↔exposure correlation toward zero or positive. | Add a DNF/incident flag and test separately. |
| **Team reputation** | Top teams (e.g., Hendrick/JGR) get coverage regardless of a single result. | Segment by team tier. |
| **Time of season / playoffs** | Playoff and championship races attract more attention. The dataset has **no `Is_Playoff` column**, so the module's playoff check could not be run. | Add a playoff indicator (races ~27–36) and re-test. |

---

## Sponsor-Level Correlations (Finish Position vs each channel)

Relationships differ by sponsor, and the signs are inconsistent — further evidence that the
pooled correlations are dominated by race-level noise rather than a stable sponsor-level law.

| Sponsor | Finish vs Reddit | Finish vs YouTube | Finish vs News |
|---|:---:|:---:|:---:|
| FedEx | +0.101 | +0.107 | +0.118 |
| Love's Travel Stops | **-0.331** | +0.235 | +0.333 |
| McDonald's | +0.173 | +0.135 | +0.110 |
| NAPA Auto Parts | -0.051 | +0.039 | -0.031 |

Only **Love's Travel Stops** shows the *expected* negative Finish→Reddit sign (-0.331, weak-
to-moderate). The other three are near zero or positive. No channel shows a consistent
direction across all four sponsors, so no single sponsor-level rule holds.

---

## Recommendations for Model Development

1. **Weight laps led as a visibility contributor (modestly).** It is the only performance
   metric with a statistically significant exposure link (→ news). Start it with a small,
   evidence-based weight and revisit as data improves.
2. **Do not (yet) weight finish position on the strength of these correlations.** The
   expected finish→buzz relationship is not statistically supported in the current data.
   This is a data problem, not necessarily a real-world absence of the effect.
3. **Fix the granularity problem before finalizing weights.** Re-key Reddit and YouTube to
   the driver/sponsor level. The current race-level collection is the primary reason the
   correlations are weak, and it caps how much we can trust any model built on them.
4. **Add control variables:** playoff indicator, superspeedway/marquee flag, and a DNF flag.
   Re-run correlations within each segment.
5. **Treat news as the most trustworthy sponsor-level channel for now** — it is the only
   exposure metric that varies by sponsor and produced the significant result.
6. **Keep correlation ≠ causation front of mind.** Even the significant laps-led→news link
   may be driven by team strength or race importance rather than a direct causal effect.

---

## Appendix — Descriptive Baseline (by sponsor)

| Sponsor | Mean Finish | Median | Std | Laps Led (season) | Top-5 Rate | Wins |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| NAPA Auto Parts | 11.72 | 9.5 | 8.50 | 431 | 30.6% | 1 |
| FedEx | 13.89 | 10.5 | 11.63 | 943 | 33.3% | 3 |
| McDonald's | 15.28 | 14.0 | 9.64 | 139 | 16.7% | 0 |
| Love's Travel Stops | 21.31 | 21.5 | 10.42 | 256 | 5.6% | 0 |

FedEx led the most laps (943) and won the most (3); NAPA was the most consistent (lowest std,
best mean finish). Love's ran at the back for most of the season (mean P21, one top-5).

---

*Reproducible in `code/correlation_analysis.ipynb`. Outputs: `sponsor_summary_statistics.csv`,
`correlation_matrix.csv`, `correlation_results.csv`, `sponsor_correlations.csv`,
`sponsor_exposure_extremes.csv`, `output/figures/correlation_heatmap.png`.*
