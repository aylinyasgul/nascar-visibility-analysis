# NASCAR Sponsorship Visibility Scoring Model - Methodology

## Executive Summary

This document describes the methodology used to score, rank, and evaluate the ROI efficiency of NASCAR Cup Series sponsors for the 2024 season. The model combines race performance, media coverage, and social engagement into a composite 0-100 visibility score, aggregates it to a season total, and divides by estimated sponsorship cost to measure value per dollar.

## 1. Variables and Weights

| Category | Variable | Description | Effective Weight |
|---|---|---|:---:|
| Race Performance | finish_position | Race finishing position (inverted) | 28% |
| Race Performance | laps_led | Number of laps leading | 12% |
| Media Coverage | news_weighted_mentions | Quality-weighted news articles | 30% |
| Social Engagement | reddit_mentions | r/NASCAR mentions (Google Trends proxy) | 10% |
| Social Engagement | youtube_sponsor_views | Views on sponsor-related videos | 10% |
| Special Events | is_win | Binary flag for race wins | 6% |
| Special Events | is_playoff | Binary flag for playoff races | 4% |
| **TOTAL** | | | **100%** |

**Weight rationale:** category weights (Race Performance 40%, Media 30%, Social 20%, Special Events 10%) come from industry sponsorship-ROI frameworks, refined by the Project 3 EDA. News carries the highest single weight (30%) because it was the only exposure channel that varied cleanly by sponsor and produced a statistically significant performance link (Laps Led to News, r = +0.181, p = 0.030).

## 2. Data Sources

| Source | Data | Collection |
|---|---|---|
| Racing Reference / Kaggle | Finish position, laps led | API / download |
| Reddit via Google Trends proxy | Social interest (r/NASCAR) | Trends API (Reddit API returned 403) |
| YouTube Data API | Sponsor video views | API |
| Manual news tracking | Weighted news mentions | Search + tiered weighting |
| Industry benchmarks (Forbes, SBJ) | Sponsorship cost estimates | Tier-based estimates |

## 3. Scoring Methodology

- **Normalization:** min-max to 0-100. `normalized = (value - min) / (max - min) * 100`, season-wide.
- **Special cases:** finish position inverted (P1 = 100); YouTube views log-transformed before normalizing; binary flags scaled to 0/100.
- **Weekly score:** `SUM(normalized_value * effective_weight)`.
- **Season total:** `SUM(weekly_scores)` (sum, not average, because all sponsors ran all 36 races).
- **Efficiency:** `total_visibility / estimated_cost_in_millions` (points per $M).

## 4. Assumptions and Limitations

1. Cost estimates are approximations - exact sponsorship values are confidential.
2. Reddit is a Google Trends proxy and, like total YouTube views, is race-level (identical across sponsors within a race), so it does not differentiate sponsors in the ranking.
3. youtube_sponsor_views is non-zero in only 4 of 144 rows, so its weight contributes little in practice.
4. Sponsorship packages differ (primary vs associate assets), so cost-per-visibility compares unlike packages.
5. Efficiency values are on this model internal 0-100 score scale, so absolute numbers are not directly comparable to external industry benchmarks; the relative ranking is what matters.
6. Intangibles (prestige, B2B/hospitality value, strategic fit) are not captured by efficiency.

## 5. Sensitivity Analysis

- **Weight sensitivity:** tested 4 configurations (baseline, performance-heavy, media-heavy, equal). Maximum rank movement was 1 position(s): the model is robust.
- **Cost sensitivity:** efficiency ranks were re-computed under low/mid/high cost estimates (max efficiency-rank movement 1 position(s)).

## 6. Results Summary

### 6.1 Visibility Rankings

| Rank | Sponsor | Total Visibility | Est. Cost |
|:---:|---|:---:|:---:|
| 1 | NAPA Auto Parts | 1219 | $21.5M |
| 2 | FedEx | 1215 | $18.5M |
| 3 | McDonald's | 914 | $14.0M |
| 4 | Love's Travel Stops | 699 | $8.5M |

### 6.2 Efficiency Rankings (value per dollar)

| Rank | Sponsor | Visibility/$M | Cost per Point | Value note |
|:---:|---|:---:|:---:|---|
| 1 | Love's Travel Stops | 82.2 | $12,164 | best value per dollar |
| 2 | FedEx | 65.7 | $15,221 | consistent value |
| 3 | McDonald's | 65.3 | $15,324 | consistent value |
| 4 | NAPA Auto Parts | 56.7 | $17,641 | premium for prestige/performance |

## 7. Appendix - Files Generated

| File | Description |
|---|---|
| `rankings.csv` | Final visibility and efficiency rankings |
| `efficiency_rankings.csv` | Detailed efficiency analysis |
| `scored_dataset.csv` | Full dataset with visibility scores |
| `cost_estimates.csv` | Sponsorship cost research |
| `sensitivity_analysis.md` | Weight and cost sensitivity results |
| `validation_report.md` | Model validation results (Module 4.2) |
| `methodology.md` | This document |

All analysis code is in `code/` (scoring_methodology, visibility_scoring_model, roi_efficiency_analysis).

*This methodology document supports the NASCAR Sponsorship ROI analysis for NY Racing.*