# Visualization Review - Module 3.2

Systematic review of what each chart reveals. Feeds the EDA report and the hypotheses.

> **Data note:** "Reddit" throughout this document refers to the `reddit_*` columns,
> which hold **Google Trends** weekly search-interest scores. Reddit's API returned HTTP 403
> during collection and Google Trends was substituted (see `docs/data_dictionary.md`).

## Scatter Plots

### Finish Position vs. Reddit Mentions
- **Observed pattern:** No trend. Points fall in horizontal bands (Reddit values 0, 7-8, 15, 30) and vertical bands (each race's 4 sponsors share one Reddit value). Trend line is flat.
- **Strength of relationship:** Negligible (r = -0.024, R² = 0.001).
- **Notable outliers:** A cluster near Reddit = 30 (marquee races - Daytona, playoffs) spread across all finish positions.
- **Potential hypothesis:** Reddit/Google-Trends buzz is driven by *which race it is*, not by our drivers' finish. (Reddit is race-level.)

### Finish Position vs. YouTube Views
- **Observed pattern:** Weak positive (wrong direction) - a few very-high-view races (Daytona ~120M) occur across finish positions.
- **Strength:** Negligible (r = +0.124, R² = 0.015).
- **Notable outliers:** Superspeedway races with 10-100× normal views.
- **Potential hypothesis:** YouTube views reflect race prominence, not sponsor performance.

### Finish Position vs. News Mentions
- **Observed pattern:** Discrete (news is mostly 0, 1, or 2). Slightly more 1-2 mention rows at better finishes.
- **Strength:** Negligible pooled (r = -0.048) but news is the only sponsor-varying channel.
- **Potential hypothesis:** Better finishes earn modestly more news coverage (tested as H1/H2).

### Laps Led vs. Social Mentions
- **Observed pattern:** No relationship with Reddit/YouTube; a weak positive with News.
- **Does dominance matter?** Only for news - leading laps to more write-ups (r = +0.181, p = 0.030, from Module 3.1). Social channels don't respond.

## Comparison Charts

### Sponsor Rankings (composite visibility)
- **Top visibility sponsors:** NAPA Auto Parts (65) and FedEx (65).
- **Bottom visibility sponsors:** McDonald's (35.8) and Love's Travel Stops (35).
- **Performance correlation:** Top-visibility sponsors (NAPA, FedEx) are also the best performers (mean finish 11.7 and 13.9) - but the composite ranking here is driven by news share, not Reddit/YouTube (which don't vary by sponsor).

### Season Trends
- **Peak visibility periods:** Marquee races - season opener (Daytona, race 1), races 24 and 28. Sharp spikes, not a smooth trend.
- **Playoff effect:** Composite visibility is *lower* in the playoffs (-22.8%), the opposite of the expected boost - likely a data-collection artifact (Google Trends/YouTube values later in the season).
- **Consistency patterns:** All four sponsors rise and fall together - visibility is race-driven, not sponsor-driven.

### Channel Mix
- **Reddit/YouTube-heavy:** All sponsors (raw magnitude), but these are race-level and identical.
- **News-heavy (differentiating):** NAPA and FedEx (news = 46% of their composite) vs McDonald's (2%) and Love's (0%).
- **Pattern explanation:** News is the only channel that distinguishes sponsors, so it carries all the sponsor-level signal.
