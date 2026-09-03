## 1.3 Visibility Scoring Model

We built a composite visibility score that combines four categories into a single 0-100 metric.
Category weights start from industry sponsorship-ROI frameworks and are refined by the exploratory
analysis. News carries the highest single weight because it was the only sponsor-varying exposure
channel with a statistically significant link to on-track performance.

| Category | Variable | Effective Weight | Transform |
|---|---|---|---|
| Race Performance | finish_position | 28% | invert_position |
| Race Performance | laps_led | 12% | normalize |
| Media Coverage | news_weighted_mentions | 30% | normalize |
| Social Engagement | reddit_mentions | 10% | normalize |
| Social Engagement | youtube_sponsor_views | 10% | log_normalize |
| Special Events | is_win | 6% | binary |
| Special Events | is_playoff | 4% | binary |

**Normalization.** All variables are scaled to a 0-100 range with min-max normalization computed
season-wide (across all 144 rows) so every sponsor sits on one shared scale.

- Direct variables (higher is better): `score = (value - min) / (max - min) * 100`.
- Inverse variables (finish position, lower is better): `score = (max - value) / (max - min) * 100`,
  so a race win (P1) maps to 100.
- YouTube views are log-transformed (`log(x + 1)`) before normalizing to prevent a single viral
  video from dominating.
- Binary flags (wins, playoff races) scale directly to 0 or 100.
- Edge case: if every value is identical, the variable returns 50 (no differentiation possible).

**Aggregation.** The weekly visibility score is the weighted sum of the normalized variables. The
season total is the sum of the 36 weekly scores (theoretical maximum 100 x 36 = 3,600). Efficiency
is the season total divided by the estimated cost in millions of dollars.

## 1.4 Validation

The model was validated with directional checks (do the rankings match expected patterns), event
checks (do wins and playoff races spike), and anomaly detection. Every check passed.

| Check | Result | Expected | Pass |
|---|---|---|---|
| Finish position vs. total visibility | r = -0.93 | Negative (better finish, higher score) | Yes |
| Wins vs. total visibility | r = +0.76 | Positive | Yes |
| Laps led vs. total visibility | r = +0.70 | Positive | Yes |
| Win vs. non-win weekly score | 1.9x | 1.5-3x | Yes |
| Top-5 vs. bottom-20 weekly score | 2.5x | Above 1.5x | Yes |
| Anomalies (scores over 100, impossible values) | None | None | Yes |

A weight-sensitivity test across four configurations (Section 2.4) moved the ranking at most one
position, confirming the model is stable.
