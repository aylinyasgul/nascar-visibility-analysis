# Data Source Evaluation
**NASCAR Visibility Analysis**
Prepared by: Aylin Yasgul | Date: 2026-06-09

> **This is the pre-collection planning document**, kept as a record of what was evaluated
> and why. One source did not survive contact with collection: the Reddit API was blocked,
> and Google Trends was substituted. See the Reddit section below and `README.md` for what
> was actually collected.

---

## Race Performance Data

| Element | Details |
|---|---|
| Primary Source | Kaggle – "NASCAR 2017–2024 Full Race & Points Data – Cup" |
| Backup Source | Racing-Reference.info (manual gap-filling and cross-verification) |
| Data Range | 2024 NASCAR Cup Series season (36 points races) |
| Access Status | Downloaded |
| Notes | Dataset scored 17/20 across evaluation criteria. No Race_Date column; dates not available without supplemental scraping. Filtered from 2017–2024 dataset to 2024 only in `load_kaggle_data.ipynb`. |

---

## Social Media Data

### Reddit

| Element | Details |
|---|---|
| API Access | Complete (PRAW Python library, free Reddit API credentials) |
| Planned Subreddits | r/NASCAR |
| Collection Approach | Search by sponsor name and driver keyword, filtered by date range |
| Concerns | Free tier rate limits restrict historical data volume; batch collection required across multiple days to cover full 2024 season |
| **Outcome (2026-06-22)** | **Not used.** Every request returned HTTP 403, blocked at IP level despite correct User-Agent headers. Replaced with Google Trends. |

**Substitution:** per mentor guidance, **Google Trends** weekly search interest (`pytrends`)
was adopted as the public-interest proxy, collected for the four sponsor search terms over
2024-02-18 to 2024-11-10 (1,068 weekly points). The `reddit_*` column names were retained
downstream for pipeline consistency; they hold Google Trends scores. The switch is recorded in
`data/processed/reddit_collection_metadata.json` and in `code/reddit_data_collection.ipynb`,
which contains the Google Trends collection that replaced it.

### YouTube

| Element | Details |
|---|---|
| API Access | Complete (YouTube Data API v3, Google Cloud Console) |
| Search Strategy | Race name + sponsor name; highlight videos; race recap content |
| Quota Considerations | Free tier: 10,000 units/day. Batch requests across multiple days; prioritise highest-value races (Daytona, Playoffs) if quota is hit before full season is captured |

### News Coverage

| Element | Details |
|---|---|
| Primary Approach | Manual Google search tracking |
| Search Pattern | Sponsor name + race name + race date |
| Tracking Method | Structured log; NewsAPI.org as optional supplement if manual tracking proves insufficient |

---

## Target Sponsors

| Sponsor | Team | Driver | Tier | Rationale |
|---|---|---|---|---|
| FedEx | Joe Gibbs Racing | Denny Hamlin (#11) | Top | 20-year primary sponsorship in its final season; 8th in 2024 points; richest historical data in the sport |
| NAPA Auto Parts | Hendrick Motorsports | Chase Elliott (#9) | Top | Most popular NASCAR driver; 26 primary races in 2024; highest Reddit and social signal volume of any driver |
| McDonald's | 23XI Racing | Bubba Wallace (#23) | Mid | 13th in 2024 points; Michael Jordan team ownership generates outsized social presence for a mid-tier team |
| Love's Travel Stops | Front Row Motorsports | Michael McDowell (#34) | Lower | 22nd in 2024 points; 12-year partnership with FRM; distinctive yellow livery; lower-tier baseline benchmark |

**Sponsors evaluated but excluded:** HendrickCars.com/Larson (co-primary with Valvoline; brand not independently searchable); Busch Light/Chastain (mid-tier slot covered more effectively by McDonald's/Wallace); Guaranteed Rate/Rick Ware Racing (driver rotation creates data consistency problems).

---

## Data Gaps & Limitations

- **No TV exposure data:** Nielsen viewership metrics are prohibitively expensive; all visibility measurements are proxies (social mentions, search volume) rather than actual broadcast airtime.
- **API rate limits:** Reddit and YouTube free tier quotas restrict historical data volume; collection must be batched over multiple days.
- **McDonald's partial-season coverage:** McDonald's appeared as primary sponsor on a subset of Wallace's 36 races; analysis must be filtered to confirmed McDonald's race weeks using Jayski paint scheme records.
- **No Race_Date in Kaggle dataset:** Exact race dates are unavailable without supplemental scraping from Racing Reference.

---

## Feasibility Assessment

Data collection plan is **realistic**. The Kaggle dataset provides clean race results without scraping. Reddit and YouTube APIs are free and sufficient for the 2024 season scope if batched carefully. Manual Google search covers news gaps. All four target sponsors have confirmed, searchable brand identities with sufficient online presence.

---

## Source Scoring Summary (Kaggle Dataset)

Primary race data source scored **17/20** across evaluation criteria (completeness, accessibility, data quality, sponsor coverage, historical depth). Racing Reference retained as backup for gap-filling. Nielsen TV data excluded due to cost.
