# Appendices

## Appendix A: Full Data Tables

### A.1 Combined Rankings

| Sponsor | Total Visibility | Vis Rank | Efficiency (pts/$M) | Eff Rank | Est. Cost |
|---|---|---|---|---|---|
| NAPA Auto Parts | 1219 | 1 | 56.7 | 4 | $21.5M |
| FedEx | 1215 | 2 | 65.7 | 2 | $18.5M |
| McDonald's | 914 | 3 | 65.3 | 3 | $14.0M |
| Love's Travel Stops | 699 | 4 | 82.2 | 1 | $8.5M |

### A.2 Descriptive Statistics by Sponsor

| Sponsor | Mean Finish | Median | Std | Best | Worst | Laps Led | Wins | Top-5 Rate |
|---|---|---|---|---|---|---|---|---|
| NAPA Auto Parts | 11.7 | 10 | 8.5 | 1 | 36 | 431 | 1 | 31% |
| FedEx | 13.9 | 10 | 11.6 | 1 | 38 | 943 | 3 | 33% |
| McDonald's | 15.3 | 14 | 9.6 | 3 | 36 | 139 | 0 | 17% |
| Love's Travel Stops | 21.3 | 22 | 10.4 | 2 | 38 | 256 | 0 | 6% |

### A.3 Cost Estimates (benchmark-based; confidential deals)

| Sponsor | Team | Tier | Low | Mid | High | Confidence |
|---|---|---|---|---|---|---|
| NAPA Auto Parts | Hendrick Motorsports | top | $18M | $21.5M | $25M | medium |
| FedEx | Joe Gibbs Racing | top | $15M | $18.5M | $22M | medium-high |
| McDonald's | 23XI Racing | mid-to-top | $10M | $14.0M | $18M | medium |
| Love's Travel Stops | Front Row Motorsports | mid | $5M | $8.5M | $12M | low-medium |

### A.4 Multi-Criteria Evaluation (0-100 scores)

Recommendation score = 0.4 visibility + 0.4 efficiency + 0.2 consistency.

| Sponsor | Visibility Score | Efficiency Score | Consistency Score | Rec Score |
|---|---|---|---|---|
| NAPA Auto Parts | 100 | 25 | 93 | 68.5 |
| FedEx | 75 | 75 | 0 | 60.0 |
| McDonald's | 50 | 50 | 53 | 50.5 |
| Love's Travel Stops | 25 | 100 | 100 | 70.0 |

## Appendix B: Methodology Details

### B.1 Scoring Configuration

| Variable | Category | Weight in Category | Effective Weight | Direction | Transform |
|---|---|---|---|---|---|
| finish_position | Race Performance | 70% | 28% | inverse | invert_position |
| laps_led | Race Performance | 30% | 12% | direct | normalize |
| news_weighted_mentions | Media Coverage | 100% | 30% | direct | normalize |
| reddit_mentions | Social Engagement | 50% | 10% | direct | normalize |
| youtube_sponsor_views | Social Engagement | 50% | 10% | direct | log_normalize |
| is_win | Special Events | 60% | 6% | direct | binary |
| is_playoff | Special Events | 40% | 4% | direct | binary |

### B.2 Variable Selection Decision

Seven variables were selected from ten candidates on three tests (relevance, independence,
reliability). Five were excluded.

**Selected:** finish_position, laps_led (performance); news_weighted_mentions (media);
reddit_mentions, youtube_sponsor_views (social); is_win, is_playoff (special events).

**Excluded and why:** reddit_total_score and reddit_engagement_per_mention (sparse, 9 of 144
rows; redundant, r = 0.94); youtube_total_views (race-level, not sponsor-specific);
youtube_view_share (derivative and sparse); news_total_mentions (redundant with weighted news,
r = 0.98).

### B.3 Weighting Rationale

Category weights (Race Performance 40%, Media 30%, Social 20%, Special Events 10%) come from
industry sponsorship-ROI frameworks (on-field performance 30-40%, media 25-35%, social 20-30%,
activation 10-20%), with the unmeasurable activation weight redistributed. The split was then
refined by the exploratory analysis: news receives the highest single variable weight (30%)
because it was the only sponsor-varying channel that produced a statistically significant link to
performance. Finish position is weighted highest within performance on industry grounds, with the
open caveat (Section 4) that its observed correlation with exposure was weak in this dataset.

## Appendix C: Code Repository Guide

All analysis is reproducible in the project repository.

| File / Folder | Purpose |
|---|---|
| `code/clean_race_data.ipynb`, `code/data_merge_master.ipynb` | Data cleaning and merge (Project 2) |
| `code/correlation_analysis.ipynb` | Correlation analysis (Project 3.1) |
| `code/visualization_analysis.ipynb` | EDA and visualizations (Project 3.2) |
| `code/scoring_methodology.ipynb` | Variable selection, weighting, normalization design (Project 4.1) |
| `code/visibility_scoring_model.ipynb` | Scoring model implementation and validation (Project 4.2) |
| `code/roi_efficiency_analysis.ipynb` | Cost research and efficiency analysis (Project 4.3) |
| `code/strategy_recommendations.ipynb` | Recommendation briefs and scenarios (Project 5.1) |
| `code/final_report.ipynb` | This report assembly (Project 5.2) |
| `outputs/project4/`, `outputs/project5/` | Generated results, tables, and charts |

*End of report. Prepared by Aylin Yasgul for NY Racing. Based on 2024 season data.*
