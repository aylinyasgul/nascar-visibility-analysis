# NASCAR Sponsorship ROI Analysis

Which NASCAR sponsorship actually returns the most value per dollar? Built for **NY Racing**
as an IE University externship, covering the full 2024 NASCAR Cup Series season.

## Headline Finding

**The most visible sponsorship is the least efficient one.**

Ranking the four sponsors by raw visibility puts NAPA Auto Parts first. Ranking them by
visibility *per dollar spent* inverts the table completely: Love's Travel Stops, dead last
on visibility, returns **45% more value per dollar** than NAPA.

| Sponsor | Visibility score | Visibility rank | Est. cost ($M) | Value per $M | Efficiency rank |
|---|---:|:---:|---:|---:|:---:|
| NAPA Auto Parts | 1218.8 | **1** | 21.5 | 56.7 | **4** |
| FedEx | 1215.4 | 2 | 18.5 | 65.7 | 2 |
| McDonald's | 913.6 | 3 | 14.0 | 65.3 | 3 |
| Love's Travel Stops | 698.8 | **4** | 8.5 | 82.2 | **1** |

**Recommendation: FedEx**, as the balanced option. It captures 99.7% of the maximum
exposure available at 80% of the best efficiency in the field. Love's is the pick if the
objective is pure cost-efficiency; NAPA only if the objective is reach at any price.

The ranking is robust: across four alternative weighting configurations, no sponsor moves
more than one position, and only NAPA and FedEx ever swap the top two.

## Deliverables

- **16-page report (PDF)** — [`outputs/project5/NASCAR_Sponsorship_ROI_Report.pdf`](outputs/project5/NASCAR_Sponsorship_ROI_Report.pdf)
- **10-slide executive deck** — `outputs/project5/NASCAR_Sponsorship_Presentation.pptx`, with speaker notes
- **Methodology** — [`outputs/project4/methodology.md`](outputs/project4/methodology.md)
- **Per-sponsor recommendation briefs** — `outputs/project5/recommendation_*.md`

## Data

144 sponsor-race records: 4 sponsors x 36 races of the 2024 Cup Series.

| Source | What it provides | Collection method | Records |
|---|---|---|---:|
| Kaggle NASCAR 2017-2024 Cup dataset | Finish position, laps led, wins | Downloaded, filtered to 2024 | 144 |
| Google Trends | Public interest score (social proxy) | `pytrends` | 104 |
| YouTube Data API v3 | Views, video counts, engagement | API, quota-batched | 104 |
| News coverage | Sponsor mentions per race | Manual Google Search, structured log | 89 |

### Note on the social data source

The project was designed around Reddit as the social signal. **Reddit's API returned HTTP
403 for every request**, blocked at IP level despite correct User-Agent headers. Per mentor
guidance, **Google Trends search interest was substituted** as the public-interest proxy.
The abandoned attempt is preserved in `code/test_reddit_api.ipynb` for transparency.

**The `reddit_*` columns in `master_dataset.csv` hold Google Trends interest scores, not
Reddit data.** The names were kept so the collection notebooks, `scoring_config.json`, and
every downstream output stay consistent with each other; renaming them now would silently
break the scoring pipeline. Column-by-column definitions are in `docs/data_dictionary.md`.

Because Google Trends returns interest at the search-term level, the social signal is
weaker per sponsor than originally planned. This is why **news carries the highest single
weight (30%)** in the scoring model: it was the only exposure channel that varied cleanly
by sponsor and produced a statistically significant link to on-track performance
(Laps Led to News Mentions, r = +0.181, p = 0.030).

## Method

1. **Collect** race results, Google Trends interest, YouTube engagement, and news mentions
2. **Clean and merge** into one 144-record analytical dataset (`data/processed/master_dataset.csv`)
3. **Explore** — correlation analysis and EDA to establish which signals are usable
4. **Score** — a composite 0-100 visibility score, min-max normalized, weighted across race
   performance (40%), media (30%), social (20%), and special events (10%)
5. **Validate** — verification traces every config value through to output; correlation
   checks (finish r = -0.93, wins r = 0.76, laps r = 0.70) and weight sensitivity testing
6. **Cost overlay** — benchmark cost estimates convert visibility into value per dollar
7. **Recommend** — multi-criteria evaluation across three strategic options

## Repository Structure

```
code/      Analysis notebooks, numbered by module
data/raw/        Original collected files
data/processed/  Cleaned data and master_dataset.csv
data/external/   Kaggle source dataset
docs/      Data dictionary, source evaluation, findings write-ups
output/    Figures from Projects 3.x
outputs/   Generated artifacts from Projects 4.x and 5.x
```

## Setup

```bash
conda create -n nascar-visibility python=3.11
conda activate nascar-visibility
pip install pandas numpy matplotlib seaborn jupyter requests beautifulsoup4 \
            pytrends google-api-python-client python-pptx
```

A YouTube Data API v3 key is required to re-run collection; it is read from `config.py`,
which is gitignored. All processed data is committed, so the analysis notebooks run without
API access. (`praw` is only needed to reproduce the blocked Reddit attempt.)

## Modules
- **Project 2 (2.1-2.3):** Data collection, cleaning, and merge to `data/processed/master_dataset.csv`
- **Project 3.1 - Correlation Analysis:** `code/correlation_analysis.ipynb`
  - Descriptive statistics by sponsor to `data/processed/sponsor_summary_statistics.csv`
  - Pearson correlations + p-values to `data/processed/correlation_results.csv`, `correlation_matrix.csv`
  - Sponsor-level correlations to `data/processed/sponsor_correlations.csv`
  - Highest/lowest exposure races to `data/processed/sponsor_exposure_extremes.csv`
  - Heatmap to `output/figures/correlation_heatmap.png`
  - **Findings write-up:** `docs/correlation_findings.md` (deliverable: `Module 3.1/NASCAR_Project 3.1_Correlation Findings.docx`)
  - Key result: only *Laps Led to News Mentions* is statistically significant (r=+0.181, p=0.030); Google Trends and YouTube signals were collected at race level, which limits sponsor-level correlations.
- **Project 3.2 - Visualization & EDA:** `code/visualization_analysis.ipynb`
  - 8 publication-quality charts to `output/figures/` (scatter, scatter grid, regression scatter, sponsor bar, season line, weekly heatmap, stacked channel mix, correlation heatmap)
  - `docs/visualization_review.md` (chart-by-chart review), `docs/hypotheses_for_model.md` (H1-H5)
  - **Deliverable:** `Module 3.2/EDA_report.pdf` - full exploratory data analysis report
  - Key hypotheses: news (via top-5 finishes & laps led) is the reliable sponsor-level signal; use a discrete win/top-5 bonus (not linear finish weight); playoff visibility is *lower* (-22.8%), so no playoff boost yet.
- **Project 4.1 - Scoring Methodology Design:** `code/scoring_methodology.ipynb`
  - Variable selection (7 selected, 5 excluded), multicollinearity check, decision matrix
  - Weighting: categories 40/30/20/10; effective weights (finish 28, laps 12, news 30, trends 10, youtube 10, is_win 6, is_playoff 4)
  - Normalization: min-max to 0-100 (inverse for finish, log for youtube, binary x100); sum aggregation to season total
  - Outputs in `outputs/project4/`: `variable_selection.md`, `weighting_rationale.md`, `normalization_spec.md`, `score_variables.json`, `scoring_config.json`, `correlation_matrix_variables.png`
  - **Deliverable:** `Module 4.1/NASCAR_Project 4.1_Normalization Specification.pdf`
  - Design only; sponsor scoring is implemented in Module 4.2.
- **Project 4.2 - Scoring Model Implementation:** `code/visibility_scoring_model.ipynb`
  - Modular functions (normalize, score, verify) driven entirely by `outputs/project4/scoring_config.json`; runs end-to-end, verification traces config to output
  - Rankings: NAPA #1 (1218.8), FedEx #2 (1215.4), McDonald's #3 (913.6), Love's #4 (698.8)
  - All validation passes (finish r=-0.93, wins r=0.76, laps r=0.70; win ratio 1.9x, playoff +13.7%, top5/bottom20 2.46x); no anomalies
  - Outputs in `outputs/project4/`: `scored_dataset.csv`, `sponsor_rankings.csv`, `sponsor_dashboard.png`, `validation_report.md`
- **Project 4.3 - ROI Efficiency Analysis:** `code/roi_efficiency_analysis.ipynb`
  - Cost research (benchmark estimates for the 4 sponsors), efficiency = visibility per $M, cost per point
  - Efficiency ranking flips visibility: Love's #1 (82 pts/$M, cheap Front Row deal), FedEx #2, McDonald's #3, NAPA #4 (56 pts/$M, premium Hendrick deal)
  - Weight sensitivity across 4 configs: ROBUST (max rank move = 1; only NAPA/FedEx swap the top two). Cost sensitivity (low/mid/high) tested.
  - **Deliverables in `outputs/project4/`:** `rankings.csv`, `efficiency_rankings.csv`, `methodology.md` (primary), plus `cost_estimates.csv`, `cost_documentation.md`, `sensitivity_analysis.md`, `efficiency_comparison.png`, `visibility_cost_scatter.png`, `sensitivity_analysis.png`
- **Project 5.1 - Strategy & Recommendations:** `code/strategy_recommendations.ipynb`
  - Multi-criteria evaluation, top-by-visibility vs top-by-efficiency comparison, per-sponsor recommendation briefs
  - Three strategic options: Maximize Visibility (NAPA), Maximize Efficiency (Love's), Balanced (FedEx, recommended)
  - Key insight: FedEx captures 99.7% of max exposure at 80% of best efficiency, the standout compromise
  - **Deliverables in `outputs/project5/`:** `evaluation_matrix.csv`, `recommendation_fedex.md`, `recommendation_loves.md`, `recommendation_napa.md`, `scenario_comparison.md`, `scenario_comparison.png`; polished PDF in `Module 5.1/NASCAR_Project 5.1_Sponsorship Recommendations.pdf`
- **Project 5.2 - Final Report:** `code/final_report.ipynb`
  - SCQA executive summary; assembles report sections into `outputs/project5/report_sections/` + compiled `outputs/project5/final_report.md`
  - **Capstone deliverable:** `Module 5.2/NASCAR_Sponsorship_ROI_Report.pdf` - a standalone 16-page report (title, TOC, exec summary, methodology, findings with 8 embedded charts, recommendations, limitations, full appendices A/B/C); self-contained `outputs/project5/final_report.md` for pandoc/Jupyter export
- **Project 5.3 - Final Presentation:** built with python-pptx
  - **Deck deliverable:** `outputs/project5/NASCAR_Sponsorship_Presentation.pptx` - 10-slide executive deck (title, exec summary, methodology, data/limits, 3 key findings with charts, recommendations, trade-offs, next steps/Q&A) with speaker notes on each slide
  - Presentation-quality charts in `outputs/project5/presentation_charts/`; support docs: `slide_content_outline.md`, `speaker_notes.md`, `anticipated_questions.md`, `hero_number_templates.md`, `visual_style_guide.md`, `timing_worksheet.md`
  - User-owned steps: record the 10-minute delivery (Loom/video) and upload the 3 final deliverables (deck, report, recording) to Google Drive

> **Filing note:** Project 4 (Modules 4.1-4.3) and Project 5 write generated artifacts to `outputs/project4/` and `outputs/project5/`, per the module instructions (submission forms ask for those exact paths). Project 2/3 used the main folders (`data/processed/`, `output/figures/`, `docs/`). `master_dataset.csv` stays in `data/processed/` as the shared input.