# Visibility Score: Weighting Rationale

**Module:** 4.1 (methodology design), Week 5

## Overall Philosophy

The weights follow a hybrid approach: category weights start from established industry
frameworks (expert judgment) and are refined by our EDA findings. Three principles guide the
split:

1. On-track results determine the visibility opportunity (better finishes put the car on camera).
2. Traditional media still reaches the largest audience for NASCAR content.
3. Social engagement, while valuable, represents a subset of the total audience.

## Category Weights

| Category | Weight | Rationale |
|---|:---:|---|
| Race Performance | 40% | On-track success is the foundation of visibility |
| Media Coverage | 30% | Traditional media reaches the broadest audience |
| Social Engagement | 20% | Active fan engagement indicates brand connection |
| Special Events | 10% | Wins and playoff races generate disproportionate buzz |

Industry reference (Playbook Sports, Top 10 Metrics for Measuring Sponsorship ROI): on-field
performance typically 30-40%, media exposure 25-35%, social engagement 20-30%, activation
success 10-20%. We cannot measure activation (hospitality, B2B deals), so that weight is
redistributed across the measurable categories. Our 40/30/20/10 split sits inside these ranges.

## Effective Variable Weights

Effective weight = category weight x weight-within-category.

| Variable | Category | Effective Weight | Calculation |
|---|---|:---:|---|
| finish_position | Race Performance | 28.0% | 40% x 70% |
| laps_led | Race Performance | 12.0% | 40% x 30% |
| news_weighted_mentions | Media Coverage | 30.0% | 30% x 100% |
| reddit_mentions | Social Engagement | 10.0% | 20% x 50% |
| youtube_sponsor_views | Social Engagement | 10.0% | 20% x 50% |
| is_win | Special Events | 6.0% | 10% x 60% |
| is_playoff | Special Events | 4.0% | 10% x 40% |
| **TOTAL** | | **100.0%** | |

News weighted mentions carries the highest single weight (30%). This is deliberate and matches
our EDA: news was the only exposure channel that varied cleanly by sponsor and produced the one
significant performance link (Laps Led to News, r = +0.181).

## Why Not Other Approaches?

- **Equal weights (14.3% each):** would treat Reddit mentions as equally important as race
  wins and ignore the hierarchy of performance over social. Not defensible.
- **Data-driven regression weights:** would require a labeled "visibility outcome" variable that
  does not exist, and would be sensitive to sample size and outliers. Harder to explain to
  stakeholders. The hybrid approach is more transparent.

## Reconciliation With Our EDA (important caveat)

The industry frameworks place performance highest, and finish_position accordingly receives the
largest performance weight (28%). However, our own EDA found finish position had a weak,
non-significant linear correlation with every exposure channel, because Reddit and YouTube were
collected at the race level and could not separate sponsors. We keep the industry-informed
weight but flag this openly:

- The performance-to-visibility link is weaker in our data than the frameworks assume.
- News (30%) and laps led (12%) carry the signal our EDA actually supports.
- This tension is exactly what sensitivity testing (below) is designed to surface, and it is the
  main reason to re-collect Reddit and YouTube at the sponsor level before finalizing weights.

## Sensitivity Testing (to run in Module 4.2)

We will re-rank sponsors under alternative category weights and report whether the ranking is
stable:

- Performance-heavy: 50 / 25 / 15 / 10
- Media-heavy: 30 / 40 / 20 / 10
- Equal: 25 / 25 / 25 / 25

If the ranking changes materially under these scenarios, we will note the sensitivity and lean
on the channels the EDA supports (news, laps led).

Config saved to `data/processed/scoring_config.json`.
