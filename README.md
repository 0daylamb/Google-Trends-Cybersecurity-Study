# Cybersecurity Attention Decay: A Data-Driven Analysis

## Overview

This project investigates whether major cyberattacks produce sustained public attention to cybersecurity, or whether concern fades quickly as media coverage declines. Using Google Trends data from 2017 to 2024, the analysis tracks public search behavior before and after three high-profile incidents: the **Equifax data breach (2017)**, the **SolarWinds hack (2020)**, and the **Colonial Pipeline ransomware attack (2021)**.

The central argument: public attention to cybersecurity is episodic, not cumulative. Incidents generate short, sharp spikes in concern that return to baseline within weeks — and that window is shrinking.

---

## Repository Structure

```
.
├── Visualizations:Outputs/
│   ├── Fig1_Time-Series-Viz.png      # Time-series: web interest vs. news coverage
│   ├── Fig2_Kmeans-Clus.png          # K-Means clustering of attention states
│   ├── Fig3_Regression.png           # Linear regression: media volume vs. public interest
│   └── Fig4_Decay-Chart.png          # Peak vs. 30-day decay bar chart
├── Lamboni - INST414 Project.ipynb   # Main analysis notebook
├── Lamboni - INST414 Project.html    # Exported notebook (HTML)
├── Lamboni - Project Final Report.pdf # Final written op-ed
└── README.md
```

---

## Tech Stack

| Library | Import | Purpose |
|---|---|---|
| Python 3 | — | Core analysis language |
| Jupyter Notebook | — | Development environment |
| pandas | `pandas` | Data manipulation and time-series structuring |
| NumPy | `numpy` | Numerical operations and array handling |
| Matplotlib | `matplotlib.pyplot` | All figure generation |
| Seaborn | `seaborn` | Regression plot (Figure 3) |
| pytrends | `pytrends.request.TrendReq` | Google Trends data collection via unofficial API |
| scikit-learn | `sklearn.cluster.KMeans`, `sklearn.preprocessing.StandardScaler` | K-Means clustering and feature scaling (Figure 2) |
| statsmodels | `statsmodels.api` | OLS linear regression and significance testing |
| urllib3 | `urllib3` | SSL warning suppression for pytrends requests |

---

## Analysis

### Part 1 — Time-Series Visualization

Tracks two aggregate indexes week-by-week from 2017 to 2024:
- **Web Interest Index**: average Google Trends search volume across five cybersecurity keywords via web search
- **News Media Coverage Index**: same keywords tracked via Google News search

Vertical markers identify the three key incidents. The plot tests the hypothesis visually — do spikes return to baseline, or do they produce lasting upward trends?

![Figure 1: Public Web Interest vs. News Media Coverage (2017-2024)](https://raw.githubusercontent.com/0daylamb/Google-Trends-Cybersecurity-Study/main/Visualizations:Outputs/Fig1_Time-Series-Viz.png)
*Figure 1. Web search interest spikes around each major incident then returns to baseline within weeks. Despite rising news media coverage since 2022, public search interest has remained flat, suggesting that increased reporting alone is no longer enough to sustain public engagement.*

---

### Part 2 — K-Means Clustering

Groups all weeks in the dataset into three behavioral states using K-Means clustering on scaled web interest and news volume features:

- **Baseline/Apathy**: the majority of weeks, low engagement across both indexes
- **Elevated Concern**: moderate public and media activity, typically surrounding but not during incidents
- **Peak Panic**: maximum public engagement, occurring only twice across the full 2017-2024 period

Clusters are assigned by ranking mean web interest across groups, ensuring consistent label mapping regardless of how K-Means assigns cluster numbers internally.

![Figure 2: K-Means Clusters of Public Attention States](https://raw.githubusercontent.com/0daylamb/Google-Trends-Cybersecurity-Study/main/Visualizations:Outputs/Fig2_Kmeans-Clus.png)
*Figure 2. K-Means clustering identifies three states of public attention across the 2017-2024 period: Baseline/Apathy, Elevated Concern, and Peak Panic. Only two weeks out of nearly 400 qualify as Peak Panic, highlighting how rarely cyber incidents drive public concern to its maximum level.*

---

### Part 3 — Linear Regression

Tests whether news media coverage volume is a statistically significant predictor of public search interest. OLS regression is run with news volume as the independent variable and web interest as the dependent variable.

Key findings:
- The relationship is statistically significant across the full dataset
- The relationship weakens after 2022, as news coverage trends upward while public interest plateaus
- R-squared and p-values are printed directly in the notebook output

![Figure 3: Linear Regression — Media Coverage Volume vs. Public Search Interest](https://raw.githubusercontent.com/0daylamb/Google-Trends-Cybersecurity-Study/main/Visualizations:Outputs/Fig3_Regression.png)
*Figure 3. News media coverage is a statistically significant predictor of public search interest, though this relationship has weakened since 2022, as rising coverage has failed to produce a corresponding rise in public engagement.*

---

### Part 4 — Attention Decay Table and Chart

Calculates the percentage drop in public search interest within 30 days of peak for each incident. A 60-day window is used for SolarWinds specifically, given the slower build of public awareness associated with that attack's gradual disclosure.

For each incident the analysis identifies:
1. The peak web interest value within the post-incident window
2. The web interest value 30 days after that peak
3. The percentage drop between the two

Results are displayed as both a summary table and a grouped bar chart with values labeled directly on each bar.

![Figure 4: Public Interest — Peak vs. 30 Days After Each Cyber Incident](https://raw.githubusercontent.com/0daylamb/Google-Trends-Cybersecurity-Study/main/Visualizations:Outputs/Fig4_Decay-Chart.png)
*Figure 4. Public search interest declined within 30 days of peak for all three incidents, suggesting that attention decay after cyberattacks is a consistent pattern rather than a case-by-case occurrence.*

---

## Key Findings

- All three incidents produced sharp spikes in public search interest that returned to pre-incident baseline within weeks
- Only **2 out of nearly 400 weeks** (2017-2024) qualified as "Peak Panic" under K-Means clustering
- News media coverage has trended upward since 2022 while public search interest has remained flat, indicating a weakening relationship between reporting and engagement
- The SolarWinds hack generated significantly less public response than Equifax or Colonial Pipeline, consistent with its gradual, non-consumer-facing nature
- Attention decay was consistent across all three incidents regardless of incident type or severity

---

## How to Run

1. Clone the repository
2. Install dependencies:
```bash
pip install pytrends pandas numpy matplotlib seaborn scikit-learn statsmodels
```
3. Open the notebook:
```bash
jupyter notebook Lamboni_-_INST414_Project.ipynb
```
4. Run all cells in order. Note: `pytrends` makes live requests to Google Trends. If rate limiting occurs, add a `time.sleep(5)` between payload builds.

---

## Notes

- `pytrends` is an unofficial API wrapper and may require `requests_args={'verify': False}` to bypass SSL issues in some environments
- Google Trends data is normalized to a 0-100 scale relative to the peak search period, not absolute volume
- Weekly data granularity is used throughout; daily data was not available for the full 2017-2024 range via this API
