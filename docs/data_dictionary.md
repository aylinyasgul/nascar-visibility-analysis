# Data Dictionary: NASCAR Sponsorship ROI Master Dataset

**File**: `master_dataset.csv`  
**Created**: 2026-06-22  
**Records**: 144 sponsor-race combinations  
**Coverage**: 2024 NASCAR Cup Series (Races 1-36)  
**Sponsors**: FedEx, NAPA Auto Parts, McDonald's, Love's Travel Stops

---

## Data Sources

| Source | Type | Collection Method | Records |
|--------|------|-------------------|---------|
| Race Results | Performance | Kaggle/Racing Reference | 144 |
| Public Interest | Social Proxy | Google Trends (pytrends) | 104 |
| YouTube | Video | YouTube Data API v3 | 104 |
| News | Traditional Media | Manual Google Search | 89 |

**Note**: Public interest data uses Google Trends (search interest score 0-100) as a
proxy for Reddit engagement. Reddit's public JSON API returned HTTP 403 for all requests;
per mentor guidance, Google Trends was substituted as the social visibility signal.

---

## Column Definitions

### Identifier Columns

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `race_number` | integer | Sequential race number in 2024 season (1-36) | 1 |
| `race_name` | string | Abbreviated race/track name | "Daytona" |
| `race_date` | date (YYYY-MM-DD) | Approximate race date | "2024-02-18" |
| `sponsor` | string | Primary sponsor name (standardized) | "FedEx" |
| `driver` | string | Driver name | "Denny Hamlin" |
| `team` | string | Team name | "Joe Gibbs Racing" |
| `merge_key` | string | Unique join key (race_number_sponsor) | "1_FedEx" |

### Race Performance Columns

| Column | Type | Description | Range |
|--------|------|-------------|-------|
| `finish_position` | integer | Final finishing position | 1-40+ |
| `laps_led` | integer | Laps leading the race | 0-500+ |

**Notes**:
- Lower `finish_position` = better (1st place is best)
- `laps_led` indicates on-track visibility (car at front of field)

### Reddit / Public Interest Columns

| Column | Type | Description | Source |
|--------|------|-------------|--------|
| `reddit_mentions` | float | Weekly Google Trends interest score (0-100) | pytrends |
| `reddit_total_score` | float | Cumulative interest across weeks in race period | Calculated |
| `reddit_avg_score` | float | Average weekly interest in race period | Calculated |
| `reddit_total_comments` | integer | Placeholder (0 - Google Trends has no comment data) | N/A |
| `reddit_engagement_per_mention` | float | Average interest score per week | Calculated |

**Notes**:
- Values represent Google Trends relative search interest, not raw Reddit post counts
- 0 indicates no measurable interest in that race period
- Scores are relative within the 2024 season (100 = peak interest)

### YouTube Exposure Columns

| Column | Type | Description | Source |
|--------|------|-------------|--------|
| `youtube_total_views` | integer | Total views on all race highlight videos | YouTube API |
| `youtube_sponsor_videos` | integer | Count of videos featuring sponsor/driver | YouTube API |
| `youtube_sponsor_views` | integer | Total views on sponsor-relevant videos | YouTube API |
| `youtube_sponsor_likes` | integer | Total likes on sponsor-relevant videos | YouTube API |
| `youtube_view_share` | float | % of race views on sponsor videos | Calculated |

**Notes**:
- `total_views` represents race-level exposure; `sponsor_views` is sponsor-specific
- 0 = no videos found; YouTube data available for races 1-34 only
- View share = (sponsor_video_views / total_race_views) × 100

### News Exposure Columns

| Column | Type | Description | Source |
|--------|------|-------------|--------|
| `news_total_mentions` | integer | Total news articles mentioning sponsor | Manual Google search |
| `news_primary_mentions` | integer | Articles where sponsor was main focus | Manual classification |
| `news_weighted_mentions` | float | Mentions weighted by source tier | Calculated |

**Notes**:
- Source tiers: Tier 1 (ESPN, NASCAR.com) = 1.0, Tier 2 = 0.8, Tier 3 = 0.6, Tier 4 = 0.4
- Primary vs secondary classification based on article headline and body focus

---

## Missing Value Treatment

All exposure metrics use **0** for missing values:
- Reddit: no measurable Google Trends interest in that period
- YouTube: no videos found for that sponsor/race
- News: no articles found in search

This is distinct from NULL, which would indicate data was not collected.

---

## Known Limitations

1. **Reddit proxy**: Google Trends used (not Reddit) - interest score is relative, not a raw count
2. **YouTube timing**: Views counted at collection time (June 2026), not at race time
3. **News depth**: Limited to ~10 results per search; may miss niche outlets
4. **Race dates**: Approximated linearly across season - source data has no Race_Date column

---

## Example Queries

```python
import pandas as pd
master_df = pd.read_csv('data/processed/master_dataset.csv')

# Filter to one sponsor
fedex_df = master_df[master_df['sponsor'] == 'FedEx']

# Find all race wins
wins = master_df[master_df['finish_position'] == 1]

# Total season exposure by sponsor
season_exposure = master_df.groupby('sponsor').agg({
    'reddit_mentions': 'sum',
    'youtube_sponsor_views': 'sum',
    'news_total_mentions': 'sum'
})

# Compare exposure: wins vs non-wins
wins = master_df[master_df['finish_position'] == 1]
non_wins = master_df[master_df['finish_position'] > 1]
print(f"Avg Reddit interest - Wins: {wins['reddit_mentions'].mean():.1f}")
print(f"Avg Reddit interest - Non-wins: {non_wins['reddit_mentions'].mean():.1f}")

# Exposure trend over season
weekly = master_df.groupby('race_number').agg({
    'reddit_mentions': 'sum',
    'youtube_total_views': 'sum'
})
```

---

## File Manifest

| File | Location | Description |
|------|----------|-------------|
| `master_dataset.csv` | `data/processed/` | Final analysis-ready dataset |
| `data_dictionary.md` | `docs/` | This document |
| `data_quality_report.md` | `docs/` | Validation results |
| `gap_analysis.json` | `data/processed/` | Machine-readable limitations |
| `standardization_log.json` | `data/processed/` | Format standardization rules |
| `merge_statistics.json` | `data/processed/` | Merge operation summary |

### Project 3.1 - Correlation Analysis outputs

| File | Location | Description |
|------|----------|-------------|
| `correlation_analysis.ipynb` | `code/` | Descriptive stats + correlation analysis notebook |
| `correlation_findings.md` | `docs/` | Correlation findings write-up (deliverable) |
| `sponsor_summary_statistics.csv` | `data/processed/` | Performance + exposure stats by sponsor |
| `correlation_matrix.csv` | `data/processed/` | Pearson correlations, all metric pairs |
| `correlation_results.csv` | `data/processed/` | Key-pair correlations with p-values |
| `sponsor_correlations.csv` | `data/processed/` | Finish-vs-exposure correlations per sponsor |
| `sponsor_exposure_extremes.csv` | `data/processed/` | Highest/lowest exposure race per sponsor |
| `correlation_heatmap.png` | `output/figures/` | Correlation matrix heatmap |

---
*Last updated: 2026-07-21*
