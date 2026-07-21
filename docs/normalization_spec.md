# Visibility Score: Normalization Specification

**Project:** NY Racing sponsor visibility scoring model
**Module:** 4.1, Week 5
**Prepared for:** Sarah Mitchell

The selected variables have very different scales (finish position 1-40, laps led 0-400, Reddit
mentions 0 to ~500, YouTube views 0 to 10,000,000+, news mentions 0-50). They cannot be combined
until they are on a common scale. This document specifies how each variable is normalized before
weighting.

## Normalization Method

**Method:** Min-Max normalization to a 0-100 scale.

**Formulas:**
- Direct variables (higher is better): `score = (x - min) / (max - min) * 100`
- Inverse variables (lower is better): `score = (max - x) / (max - min) * 100`

**Output range:** 0-100 for every variable. We chose a 0-100 scale over 0-1 or z-scores for
interpretability: stakeholders read "82 out of 100" immediately, and higher always means better.

## Variable-Specific Transformations

| Variable | Direction | Pre-transform | Notes |
|---|---|---|---|
| finish_position | Inverse | None | P1 becomes 100, worst finish becomes 0 |
| laps_led | Direct | None | 0 laps = 0, most laps = 100 |
| reddit_mentions | Direct | None | Standard min-max |
| youtube_sponsor_views | Direct | log(x+1) | Log tames extreme outliers before min-max |
| news_weighted_mentions | Direct | None | Standard min-max |
| is_win | Direct | x * 100 | Binary: 0 stays 0, 1 becomes 100 |
| is_playoff | Direct | x * 100 | Binary: 0 stays 0, 1 becomes 100 |

### Why invert finish position?

Lower finishing positions are better (P1 wins). Inverse normalization maps P1 to the top of the
scale. Tested: positions [1, 5, 10, 20, 40] normalize (inverse) to [100.0, 89.7, 76.9, 51.3, 0.0].

### Why log-transform YouTube views?

A single viral video (millions of views) would otherwise dominate. log(x+1) compresses the range:
0 views to 0, then 1k / 10k / 100k / 1M map to roughly 50 / 67 / 83 / 100 within a sample. This
gives meaningful differentiation instead of one point at 100 and the rest near 0.

### Why binary variables need no min-max

is_win and is_playoff are already 0 or 1; multiplying by 100 puts them on the same 0-100 scale.

## Normalization Scope

**Scope:** Season-wide. Min and max are computed across the entire dataset (all 36 races, all 4
sponsors), not per-sponsor or per-race. This keeps every sponsor on one shared scale so scores are
directly comparable.

## Aggregation Method (Weekly to Season)

**Method:** Sum. Each sponsor's weekly (per-race) visibility scores are summed into a Season Total
Visibility Score.

- Rationale: all four sponsors competed in all 36 races (no missing-data bias), and sponsors care
  about total exposure delivered over the season, not average per race.
- Theoretical maximum: 100 * 36 = 3600 (a perfect score every race).
- Sum rewards both consistency and peaks; average would hide total value delivered.

## Edge Case Handling

| Scenario | Handling |
|---|---|
| All values identical | Return 50 (middle of scale); no differentiation possible |
| Zero values | Treated as valid (0 = no exposure) |
| Negative values | Should not occur; raise an error if found |
| Missing values | Should not occur; handled by data validation upstream |

## Implementation Reference

The normalization functions (`normalize_to_100`, `prepare_finish_position`, `prepare_binary`,
`prepare_youtube_views`) are defined and unit-tested in `code/scoring_methodology.ipynb`. The
full sponsor scoring that applies these transforms and the weights is implemented in Module 4.2.

## Design Properties

The choices above (0-100 scale, log transform for views, sum aggregation, season-wide scope)
produce a scoring system that is:

- Interpretable: 0-100 is intuitive and higher is always better.
- Fair: no single variable dominates because of scale differences.
- Transparent: every transformation is documented and reproducible.
