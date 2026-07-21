# Visibility Score: Variable Selection

**Project:** NY Racing sponsor visibility scoring model
**Module:** 4.1 (methodology design), Week 5
**Dataset:** 2024 NASCAR Cup Series, 4 sponsors x 36 races = 144 rows

Every candidate variable was scored against three tests: Relevance (does it measure sponsor
visibility?), Independence (does it add new information?), and Reliability (is it measured
consistently across all sponsors and races?). Decisions are grounded in the Project 3 EDA.

## Selected Variables (7)

### Race Performance (2 variables)

1. **Finish Position** (inverted for scoring)
   - Rationale: The most direct measure of on-track success. 100% coverage (144/144), varies
     by sponsor within a race.
   - Note: will be inverted so higher = better (P1 becomes the top score).
   - EDA caveat: in our data finish position had a weak, non-significant correlation with every
     exposure channel. It is still the foundational performance metric, but this weakness is
     flagged for sensitivity testing (see weighting rationale).

2. **Laps Led**
   - Rationale: Measures race dominance beyond the finish. Distinct from finish position
     (r = -0.21), so it adds information. Non-zero in 67/144 rows.
   - EDA support: Laps Led to News was the one statistically significant performance-to-visibility
     link (r = +0.181, p = 0.030).

### Social Engagement (2 variables)

3. **Reddit Mentions**
   - Rationale: r/NASCAR is the largest online fan community. Cleanest available social metric
     (104/144 coverage).
   - Why not Reddit total score / engagement-per-mention? Both are sparse (9/144) and redundant
     (total score vs engagement r = +0.94). Mentions are more robust.
   - Caveat: this column is a Google Trends proxy and is race-level (identical across sponsors
     within a race), so it cannot distinguish sponsors on its own.

4. **YouTube Sponsor Views**
   - Rationale: Views on sponsor-relevant video content measure visual brand exposure, and it is
     sponsor-specific.
   - Why not total views? Total views are race-level, not sponsor-specific.
   - Caveat: extremely sparse (non-zero in only 4/144 rows), so it will contribute little in
     practice. A log transform limits outlier distortion. Flagged for re-collection.

### Media Coverage (1 variable)

5. **News Weighted Mentions**
   - Rationale: Quality-weighted news mentions (Tier 1 sources count more). Sponsor-specific,
     89/144 coverage.
   - Why not total mentions? Redundant with weighted (r = +0.98); weighted accounts for source
     quality.
   - EDA support: news was the only exposure channel that cleanly varied by sponsor and produced
     the significant performance link. It receives the highest single variable weight.

### Special Events (2 binary flags)

6. **is_win** - 1 if finish_position == 1. Wins generate disproportionate visibility spikes.
7. **is_playoff** - 1 if race_number >= 27. Playoff races carry higher stakes and viewership.

## Excluded Variables

| Variable | Reason for Exclusion | Evidence |
|---|---|---|
| reddit_total_score | Sparse and redundant | 9/144 non-zero; r = +0.94 with engagement |
| reddit_engagement_per_mention | Sparse and volatile | 9/144 non-zero |
| youtube_total_views | Not sponsor-specific | Race-level (same for all 4 sponsors in a race) |
| youtube_view_share | Derivative and sparse | 4/144 non-zero; derived from sponsor_views |
| news_total_mentions | Redundant with weighted | r = +0.98 with news_weighted_mentions |

## Decision Matrix (Relevance / Independence / Reliability, 1-5)

| Variable | Rel | Ind | Rel'y | Include | Rationale |
|---|:---:|:---:|:---:|:---:|---|
| finish_position | 5 | 5 | 5 | YES | Direct performance; full coverage; sponsor-specific |
| laps_led | 4 | 3 | 5 | YES | Race dominance; distinct from finish |
| reddit_mentions | 4 | 4 | 4 | YES | Cleanest social metric |
| youtube_sponsor_views | 5 | 4 | 2 | YES | Sponsor-specific but sparse (4/144) |
| news_weighted_mentions | 5 | 4 | 4 | YES | Quality-weighted; the real EDA signal |
| reddit_total_score | 3 | 1 | 2 | NO | Sparse; redundant (r=0.94) |
| reddit_engagement_per_mention | 3 | 2 | 2 | NO | Sparse; volatile |
| youtube_total_views | 4 | 2 | 5 | NO | Race-level, not sponsor-specific |
| youtube_view_share | 3 | 2 | 2 | NO | Derivative; sparse |
| news_total_mentions | 4 | 2 | 4 | NO | Redundant with weighted (r=0.98) |

## Multicollinearity Found (|r| >= 0.8)

- reddit_total_score and reddit_engagement_per_mention: r = +0.941 (kept neither; both sparse)
- news_total_mentions and news_weighted_mentions: r = +0.978 (kept weighted only)

Config saved to `data/processed/score_variables.json`. Correlation matrix figure:
`output/figures/correlation_matrix_variables.png`.
