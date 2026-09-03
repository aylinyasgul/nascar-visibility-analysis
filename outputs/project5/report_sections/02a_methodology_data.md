# 1. Methodology

## 1.1 Data Sources

The analysis draws on four data categories collected for the 2024 NASCAR Cup Series. Each was
verified for coverage and quality before use.

| Category | Source | Collection Method | Notes |
|---|---|---|---|
| Race performance | Kaggle NASCAR dataset, Racing Reference | Download + manual verification | Finish position, laps led; no missing races for tracked sponsors |
| Social (Reddit) | Google Trends proxy | Trends API | Reddit's public API returned HTTP 403, so Google Trends interest was substituted; race-level |
| Social (YouTube) | YouTube Data API | API | Sponsor-relevant video views; sparse (non-zero in 4 of 144 rows) |
| Media (news) | Manual search, tiered weighting | Search + classification | Tier 1 (ESPN, NASCAR.com) = 3, Tier 2 (regional) = 2, Tier 3 (blogs) = 1; sponsor-specific |
| Cost estimates | Forbes, Sports Business Journal, tier benchmarks | Estimates | Exact figures confidential; low / mid / high ranges recorded |

**Data quality notes.** The Kaggle race data was cross-checked against Racing Reference; sponsor
identification was added manually from team livery data. Social and news data were collected within
a consistent time window across all sponsors so the comparison is fair. The most important quality
caveat is that Reddit (a Google Trends proxy) and total YouTube views are *race-level* signals -
identical for all four sponsors within a race - and therefore cannot distinguish sponsors on their
own. Only news mentions and sponsor-specific YouTube views vary at the sponsor level.

## 1.2 Sponsor Selection

Four primary sponsors were analyzed. Selection criteria were: full-season primary sponsorship,
sufficient data across all four channels, and a deliberate mix of team tiers so the model is tested
against both elite and mid-pack operations.

| Sponsor | Team | Driver | Team Tier |
|---|---|---|---|
| FedEx | Joe Gibbs Racing | Denny Hamlin | Top |
| NAPA Auto Parts | Hendrick Motorsports | Chase Elliott | Top |
| McDonald's | 23XI Racing | Bubba Wallace | Mid-to-top |
| Love's Travel Stops | Front Row Motorsports | Michael McDowell | Mid |
