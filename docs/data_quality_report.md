# Data Quality Report: NASCAR Sponsorship Master Dataset

**Generated**: 2026-06-22 19:51  
**Analyst**: Aylin Yasgul

---

## Dataset Overview

| Metric | Value |
|--------|-------|
| Total Records | 144 |
| Unique Races | 36 |
| Unique Sponsors | 4 |
| Date Range | 2024-02-18 to 2024-11-10 |

## Completeness Assessment

- **Expected**: 36 races x 4 sponsors = 144 records
- **Actual**: 144 records
- **Status**: COMPLETE

## Consistency Checks

| Check | Status | Notes |
|-------|--------|-------|
| Finish positions 1–45 | PASS | Range: 1–38 |
| Laps led non-negative | PASS | Range: 0–163 |
| Dates in 2024 season  | PASS | All dates within expected range |
| YouTube view share 0–100% | PASS | |

## Outlier Summary

Statistical outliers (z-score > 3):
- Reddit mentions: 8 records
- YouTube views: 4 records
- News mentions: 8 records

Outliers are expected for major races and race wins.

## Known Limitations

1. **Reddit proxy**: Google Trends used instead of Reddit API (403 blocked). Interest score (0–100) is relative, not a raw mention count.
2. **YouTube timing**: Views counted at collection time (June 2026), not at race time.
3. **News depth**: Manual search; limited to ~10 results per race per sponsor.
4. **Race dates**: Approximated — source data has no Race_Date column.

## Recommendation

**Data quality is SUFFICIENT for analysis.** All critical consistency checks pass.

---
*NASCAR Sponsorship Visibility Analysis — Module 2.3*
