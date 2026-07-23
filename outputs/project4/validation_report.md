# Visibility Scoring Model - Validation Report

Generated: 2026-07-23 16:08

## Summary

| Check Category | Passed | Total |
|---|:---:|:---:|
| Performance Correlation | 3 | 3 |
| Event Impact | 3 | 3 |
| Anomalies (high severity) | 0 | - |

## Sponsor Rankings

| Rank | Sponsor | Total | Avg/Race | Consistency | Wins | Avg Finish |
|:---:|---|:---:|:---:|:---:|:---:|:---:|
| 1 | NAPA Auto Parts | 1218.8 | 33.9 | 92.7 | 1 | 11.7 |
| 2 | FedEx | 1215.4 | 33.8 | 0.0 | 3 | 13.9 |
| 3 | McDonald's | 913.6 | 25.4 | 52.8 | 0 | 15.3 |
| 4 | Love's Travel Stops | 698.8 | 19.4 | 100.0 | 0 | 21.3 |

## Directional Checks

- **finish_visibility_correlation**: PASS (value -0.932; expected negative (better finish = higher visibility))
- **wins_visibility_correlation**: PASS (value 0.763; expected positive (more wins = higher visibility))
- **laps_visibility_correlation**: PASS (value 0.697; expected positive (more laps led = higher visibility))

## Event Impact Checks

- **win_impact**: PASS (1.90; expected win score 1.5-3x non-win)
- **playoff_impact**: PASS (13.75; expected playoff scores higher (positive))
- **top5_vs_bottom20**: PASS (2.46; expected top 5 at least 1.5x bottom 20)

## Anomalies

No anomalies detected.

## Notable Finding: Playoff Boost Reversal

The raw EDA composite showed playoff visibility about 23% LOWER than the regular season. The scoring model instead shows a playoff boost of +13.7%. This is expected: the model adds an explicit playoff bonus (4% weight) and weights finish position heavily, so playoff races with strong finishes score higher, whereas the EDA composite was dominated by race-level social buzz that happened to dip late in the season.

## Recommendation

**Model validation PASSED.** All directional and event checks passed and no high-severity anomalies were detected. The model produces sensible, defensible rankings and is ready for the ROI efficiency analysis in Module 4.3.

## Caveats

- reddit_mentions is a Google Trends proxy and is race-level, so it adds the same amount to every sponsor in a race and does not differentiate the ranking.
- youtube_sponsor_views is non-zero in only 4 of 144 rows, so its 10% weight contributes little in practice.
- NAPA (#1) and FedEx (#2) are nearly tied; the ranking there is sensitive to the finish-vs-wins weighting and should be revisited in the Module 4.3 sensitivity analysis.