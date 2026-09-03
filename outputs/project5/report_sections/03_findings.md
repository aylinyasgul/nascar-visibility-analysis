# 2. Findings

## 2.1 Exploratory Analysis

Before building the model we examined how race performance relates to the exposure channels.
Contrary to the common assumption that better results drive more buzz, the performance-to-visibility
relationship is **weak** in this dataset.

![Correlation among candidate variables. Multicollinearity (news_total vs news_weighted r = 0.98; reddit_total vs engagement r = 0.94) guided which metrics entered the score.](figures/correlation_matrix_variables.png)

| Relationship | Correlation (r) | p-value | Significant? |
|---|---|---|---|
| Laps led vs. news mentions | +0.181 | 0.030 | Yes |
| Finish position vs. YouTube views | +0.124 | 0.139 | No |
| Finish position vs. Reddit mentions | -0.024 | 0.773 | No |

![Performance vs. exposure correlations. The near-white cells linking performance to exposure indicate weak relationships.](figures/correlation_heatmap.png)

**Insight.** The single reliable performance-to-visibility link is laps led to news coverage. The
main reason finish position shows almost no relationship is that Reddit and YouTube were collected
at the race level (identical for all sponsors in a race), so they cannot separate sponsors. This is
precisely why the scoring model weights news - the only sponsor-varying exposure channel - most
heavily.

**Descriptive baseline.** The sponsors differ sharply in on-track performance, which sets up the
visibility results that follow.

| Sponsor | Mean Finish | Median | Std | Laps Led | Wins | Top-5 Rate |
|---|---|---|---|---|---|---|
| NAPA Auto Parts | 11.7 | 10 | 8.5 | 431 | 1 | 31% |
| FedEx | 13.9 | 10 | 11.6 | 943 | 3 | 33% |
| McDonald's | 15.3 | 14 | 9.6 | 139 | 0 | 17% |
| Love's Travel Stops | 21.3 | 22 | 10.4 | 256 | 0 | 6% |

## 2.2 Visibility Rankings

| Rank | Sponsor | Total Visibility | Key Driver |
|---|---|---|---|
| 1 | NAPA Auto Parts | 1,219 | Consistent top-10 finishes (most, 19); steady exposure |
| 2 | FedEx | 1,215 | Most wins (3) and laps led; boom-or-bust superspeedway peaks |
| 3 | McDonald's | 914 | Mid-pack performance |
| 4 | Love's Travel Stops | 699 | Back-of-field running; lowest news coverage |

NAPA and FedEx are effectively tied at the top - a 3-point gap on a roughly 1,200-point scale. The
dashboard below shows season totals, the spread of weekly scores, cumulative accumulation across the
season, and the relationship between wins and total visibility.

![Season totals, weekly score distributions, cumulative trends, and wins vs. visibility.](figures/sponsor_dashboard.png)

**What drove each score.** Because Reddit and total YouTube are race-level, the sponsor-level
differences in the composite come mainly from news coverage and on-track performance. The channel
mix below shows how large the news contribution is for the top two sponsors relative to the others.

![Channel mix within the composite score. The news slice is what separates sponsors; Reddit and YouTube are race-level and identical.](figures/stacked_channel_breakdown.png)

## 2.3 Efficiency Analysis

Adding the cost dimension changes the picture entirely.

| Rank | Sponsor | Efficiency (pts/$M) | Est. Cost | Visibility Rank |
|---|---|---|---|---|
| 1 | Love's Travel Stops | 82.2 | $8.5M | 4 |
| 2 | FedEx | 65.7 | $18.5M | 2 |
| 3 | McDonald's | 65.3 | $14.0M | 3 |
| 4 | NAPA Auto Parts | 56.7 | $21.5M | 1 |

**Key insight.** The efficiency ranking inverts the visibility ranking. Love's, last in raw
exposure, is first in value per dollar (a cheaper Front Row deal); NAPA, first in exposure, is last
in efficiency (a premium Hendrick deal). FedEx is the only sponsor top-2 on both dimensions.

![Efficiency (visibility per dollar, left) and cost per visibility point (right) by sponsor.](figures/efficiency_comparison.png)

![Visibility vs. cost. Dashed lines are equal-efficiency isolines; a steeper position means better value.](figures/visibility_cost_scatter.png)

## 2.4 Sensitivity Analysis

Because both the weights and the costs are judgment calls, we tested how sensitive the rankings are
to each. Weights were varied across four configurations (baseline 40/30/20/10, performance-heavy,
media-heavy, equal). The ranking is **robust**: the maximum movement is one position, and only the
top two (NAPA and FedEx) swap. McDonald's (#3) and Love's (#4) never move. Efficiency ranks were
also stable across low, mid, and high cost estimates.

![Sponsor ranks under different weight configurations. Only the top two swap; the rest are fixed.](figures/sensitivity_analysis.png)
