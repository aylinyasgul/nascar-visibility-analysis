# Race Data Collection Summary
**NASCAR Visibility Analysis**
Prepared by: Aylin Yasgul | Date: 2026-06-09

---

## Data Source
| Field | Detail |
|---|---|
| Primary Source | Kaggle – "NASCAR 2017-2024 Full Race & Points Data – Cup" |
| URL | https://www.kaggle.com/datasets/umerhaddii/nascar-2017-2024-full-race-points-data-cup |
| Selection Rationale | Clean, structured dataset covering the full 2024 Cup Series season (36 points races). No scraping required. Filtered to 2024 in `load_kaggle_data.ipynb`. |

---

## Dataset Overview
| Metric | Value |
|---|---|
| Total Records | 1,398 |
| Unique Races (points) | 36/36 |
| Unique Drivers | 62 |
| Unique Teams | 20 |
| Date Range | 2024 season (no exact dates in source data) |

---

## Target Sponsor Coverage
| Sponsor | Driver | Race Entries | Avg Finish |
|---|---|---|---|
| FedEx | Denny Hamlin | 37 | 13.59 |
| NAPA Auto Parts | Chase Elliott | 37 | 11.46 |
| McDonald's | Bubba Wallace | 37 | 15.16 |
| Ally | Alex Bowman | 37 | 14.43 |
| Busch Light | Ross Chastain | 37 | 14.84 |
| Jordan Brand | Tyler Reddick | 37 | 12.86 |
| Shell/Pennzoil | Joey Logano | 37 | 16.84 |

---

## Data Quality

**Completeness**
- Races with complete data: 36/36
- Missing races: None — all 36 points races present (race_num 1–36)

**Cleaning Applied**
1. Standardized column names (`fin` → `Finish_Position`, `track` → `Race_Name`, `team_name` → `Team`, etc.)
2. Cleaned driver and team names: whitespace normalization, title-case, abbreviation corrections (JGR → Joe Gibbs Racing, HMS → Hendrick Motorsports, etc.)
3. Added `Primary_Sponsor` via driver–team mapping; calculated `Positions_Gained`, `Race_Number`, driver-level stats, and sponsor-level aggregates

**Known Limitations**
- Source data contains no `Race_Date` column — exact race dates not available without supplemental scraping
- Dataset includes non-points races (Clash race_num -2, Duel race_num 0) in addition to 36 points races, inflating per-driver entry counts to 37

---

## Files Produced
| File | Location | Description |
|---|---|---|
| `race_results_2024_clean.csv` | `data/processed/` | Complete cleaned race results |
| `driver_stats_2024.csv` | `data/processed/` | Driver season statistics |
| `sponsor_stats_2024.csv` | `data/processed/` | Sponsor performance summary |
| `race_results_targets_2024.csv` | `data/processed/` | Target sponsors only |

---

## Validation Results
- [x] All 36 races present
- [x] No missing critical fields (Race_Name, Driver, Team, Finish_Position)
- [x] Sponsor mapping verified for all 7 target sponsors
- [x] Performance metrics calculated correctly
- [x] No duplicate entries
