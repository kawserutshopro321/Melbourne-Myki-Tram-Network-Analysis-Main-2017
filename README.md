<div align="center">
# 🚋 Melbourne Myki Tram Network Analysis (2017)
 
### A comprehensive data analytics study of Melbourne's tram patronage using Myki smart card transaction data
 
[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0%2B-150458?style=flat-square&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.24%2B-013243?style=flat-square&logo=numpy&logoColor=white)](https://numpy.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.7%2B-11557C?style=flat-square)](https://matplotlib.org/)
[![Seaborn](https://img.shields.io/badge/Seaborn-0.12%2B-79C5E2?style=flat-square)](https://seaborn.pydata.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)]()
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Unit](https://img.shields.io/badge/Unit-SIT731%20Deakin-red?style=flat-square)]()
 
---
 
**1.9M+ Touch-On Records · 20 Tram Routes · 12 Months · 10 Analytical Investigations**
 
[Overview](#-overview) · [Dataset](#-dataset) · [Key Findings](#-key-findings) · [Analyses](#-analyses-performed) · [Setup](#-installation--setup) · [Structure](#-repository-structure) · [Methodology](#-methodology) · [Author](#-author)
 
</div>
---
 
## 📌 Overview
 
This project presents a complete end-to-end data analysis of Melbourne's tram network using **Myki smart card transaction data** spanning the full 2017 calendar year. Starting from raw touch-on and touch-off transaction files, the analysis builds an enriched, joined trip table and then conducts **10 analytical investigations** covering temporal demand patterns, stop and route-level performance, data quality assessment, passenger segmentation, and journey profiling.
 
The work was produced as part of the **SIT731 (Postgraduate Data Analysis) unit at Deakin University** and is structured to deliver operationally actionable insights for transport planners, network operators, and revenue analysts.
 
> **Scope:** 1 January 2017 – 6 January 2018 | 20 highest-patronage tram routes | Metropolitan Melbourne
 
---
 
## 📦 Dataset
 
Three source files form the analytical foundation of this project:
 
| File | Records | Description |
|------|---------|-------------|
| `myki_2017_top20_tram_routes_ScanOnTransaction.csv` | **1,904,721** | Touch-on (boarding) transactions — one row per tap |
| `myki_2017_top20_tram_routes_ScanOffTransaction.csv` | **611,293** | Touch-off (alighting) transactions — one row per tap |
| `stop_locations.txt` | **27,614** | Pipe-delimited stop reference: ID, name, suburb, coordinates, type |
 
### Schema
 
**Transaction files share the following columns:**
 
| Column | Type | Description |
|--------|------|-------------|
| `card_id` | int32 | Anonymised Myki card identifier |
| `card_type` | int8 | Fare category (1=Full, 2=Concession, 9=Free) |
| `business_date` | date | Service date (not clock date) |
| `date_time` | datetime | Exact timestamp of tap event |
| `stop_id` | int32 | Foreign key to stop reference file |
| `parent_route` | int16 | Canonical tram route number |
| `vehicle_id` | Int32 | Tram vehicle identifier (nullable) |
 
> ⚠️ **Data not included in this repository.** The raw Myki dataset is subject to PTV/DoT data licensing. Place the three source files in the directory configured by `DATA_DIR` in the notebook before running. See [Installation & Setup](#-installation--setup).
 
---
 
## 🔑 Key Findings
 
### Network-Level KPIs
 
| Metric | Value |
|--------|-------|
| **Peak day** | Friday — 3,302 avg. touch-ons (2.2× Sunday) |
| **Peak month** | November — 6,287 avg. daily touch-ons |
| **Quietest month** | June — 3,261 avg. daily touch-ons (93% below November) |
| **Touch-off completion** | 32.1% network-wide |
| **Full-fare share** | 62.9% of all boardings |
| **Multi-route transfer rate** | 8.6% of card-days |
 
### Route-Level Highlights
 
| Finding | Detail |
|---------|--------|
| **Busiest route** | Route 109 (Box Hill ↔ Port Melbourne) — 599 pax/day, 65% more than any other route |
| **Most consistent** | Route 3 — Coefficient of Variation (CoV) of 29.1% |
| **Most volatile** | Route 58 — CoV of 69.6%, suggesting event-driven demand spikes |
| **Biggest weekend drop** | Route 59 — −66% on weekends vs weekdays |
| **Most weekend-resilient** | Route 3 — only −44% on weekends |
| **Highest touch-off rate** | Route 75 — 32.1% completion (most reliable travel-time data) |
| **Longest average journey** | Route 75 — 46.9 min mean, 37.7 min median |
 
### Passenger Behaviour
 
| Segment | Cards | Share |
|---------|-------|-------|
| One-off riders (1 trip/year) | 107,008 | 38.7% |
| Occasional (2–5 trips) | 100,428 | 36.3% |
| Regular (6–20 trips) | 47,391 | 17.1% |
| Frequent (21–50 trips) | 14,988 | 5.4% |
| Very Frequent (51+ trips) | 6,852 | **2.5%** — drives disproportionate patronage volume |
 
> 75% of Myki cardholders made **5 or fewer trips** across the entire year. One card recorded **469 trips** (~1.3 trips/day on average).
 
---
 
## 📊 Analyses Performed
 
The notebook is structured as six numbered sections (Q1–Q6 plus six extended analyses), each with a rationale, methodology, results table, and chart.
 
### Q1 — Data Loading & Joining
Loads the three source files and constructs a single enriched trip table via a two-step merge:
1. Touch-on records are left-joined to stop locations on `stop_id` to attach boarding stop coordinates and names.
2. The enriched touch-on table is then left-joined to touch-off records on the composite key `(card_id, business_date, parent_route)` to attach alighting stop details and compute travel time.
**Result:** 1,904,721 rows × 22 columns. 611,293 rows (32.1%) have a matched touch-off.
 
---
 
### Q2 — Top 10 Busiest Stops by Daily Touch-Ons
Average daily touch-ons per stop across all calendar days.
 
| Rank | Stop | Avg Daily Touch-Ons |
|------|------|---------------------|
| 1 | Melbourne University Stop 1 | **175.5** |
| 2 | Flinders Street Station Stop 1 | 131.3 |
| 3 | Flinders Street Station Stop 13 | 116.0 |
| 4 | Melbourne Central Station Stop 1 | 98.4 |
| 5 | RMIT University Stop 1 | 87.2 |
| … | … | … |
| 10 | Bourke Street Mall | 51.0 |
 
> The gap between rank 1 and rank 10 is **3.4×**, illustrating a steep demand fall-off beyond the inner CBD corridor.
 
---
 
### Q3 — Top 10 Busiest Routes by Daily Patronage
Average touch-on passengers per service day.
 
| Rank | Route | Avg Daily Passengers |
|------|-------|----------------------|
| 1 | **Route 109** | **599.0** |
| 2 | Route 59 | 363.7 |
| 3 | Route 19 | 362.7 |
| 4 | Route 75 | 342.6 |
| 5 | Route 16 | 336.1 |
| 6 | Route 1 | 330.7 |
| 7 | Route 12 | 281.5 |
| 8 | Route 67 | 298.2 |
| 9 | Route 6 | 264.9 |
| 10 | Route 58 | 256.9 |
 
> Route 109 carries **1.6×** more daily passengers than the second busiest route.
 
---
 
### Q4 — Top 10 Longest Routes by Travel Time
Travel times computed from matched touch-on/touch-off records (1–180 min filter applied). Both mean and median reported to characterise distributional skew.
 
| Rank | Route | Mean (min) | Median (min) | Skew (mean − median) |
|------|-------|-----------|--------------|----------------------|
| 1 | Route 75 | 46.9 | 37.7 | +9.2 |
| 2 | Route 109 | 45.6 | 36.4 | +9.2 |
| 3 | Route 64 | 44.8 | 35.9 | +8.9 |
| 4 | Route 86 | 44.1 | 35.2 | +8.9 |
| 5 | Route 59 | 43.5 | 34.8 | +8.7 |
 
> The **mean exceeds the median on every route** — confirming right-skewed travel time distributions. The **median is the more planning-appropriate metric** for typical passenger journey time.
 
---
 
### Q5 — Hourly Touch-On Patterns Across the Network
Network-wide average touch-ons computed per hour of day.
 
| Period | Hours | Avg Network Touch-Ons | Peak Hour |
|--------|-------|----------------------|-----------|
| Morning peak | 07:00–09:00 | **20.7** | 08:00 — 24.3 |
| Midday | 10:00–15:00 | 15.4 | — |
| **Afternoon peak** | **16:00–18:00** | **23.7** | **17:00 — 26.4** |
| Evening | 19:00–23:00 | 8.6 | — |
 
> The afternoon peak is **14.2% higher** than the morning peak and **broader in duration**, sustaining elevated demand from 15:00 through 18:00.
 
---
 
### Q6 — Morning vs Afternoon Peak by Route
Using peak windows defined in Q5 (morning: 07:00–09:00, afternoon: 16:00–18:00), a diverging bar chart visualises the afternoon–morning difference per route.
 
| Route | Morning Avg | Afternoon Avg | Difference | % Change |
|-------|------------|---------------|------------|----------|
| **109** | 124.8 | 152.6 | **+27.9** | +22.3% |
| 75 | 61.4 | 82.5 | +21.0 | +34.3% |
| 57 | 23.8 | 33.0 | +9.2 | +38.7% |
| 67 | 46.2 | 45.2 | −1.0 | −2.2% |
| **59** | **96.8** | **80.5** | **−16.3** | **−16.8%** |
 
> **Route 59 is the only route with significantly more morning than afternoon boardings** — reflecting strong inbound commuter demand from Airport West and western suburbs. **17 of 20 routes** are afternoon-dominant.
 
---
 
### Extended Analysis 1 — Day-of-Week Demand
 
| Day | Avg Daily Touch-Ons |
|-----|---------------------|
| Monday | 2,550 |
| Tuesday | 2,750 |
| Wednesday | 3,135 |
| Thursday | 3,153 |
| **Friday** | **3,302** |
| Saturday | 1,978 |
| Sunday | 1,499 |
 
> Monday's anomalously low figure (relative to other weekdays) is attributed to the concentration of Australian public holidays on Mondays.
 
---
 
### Extended Analysis 2 — Monthly Patronage Seasonality
 
> Seasonal swing of **93%** between November (peak: 6,287/day) and June (trough: 3,261/day). The two-trough pattern in January and June aligns precisely with Deakin/Melbourne university semester break periods, implicating student commuting as the dominant seasonal driver.
 
---
 
### Extended Analysis 3 — Weekday vs Weekend Drop by Route
 
| Route | Weekday Avg | Weekend Avg | Weekend Drop |
|-------|------------|-------------|--------------|
| **59** | 403.4 | 137.1 | **−66.0%** |
| 64 | 258.2 | 91.1 | −64.7% |
| 86 | 260.8 | 93.5 | −64.1% |
| … | … | … | … |
| **3** | 250.0 | 140.0 | **−43.8%** (most resilient) |
 
---
 
### Extended Analysis 4 — Passenger Fare Category Breakdown
 
| Category | Transactions | Share |
|----------|-------------|-------|
| Full Fare | 1,198,247 | **62.9%** |
| Concession | 186,425 | 9.8% |
| Free Travel | 176,871 | 9.3% |
| Unknown/Unregistered | 94,351 | 5.0% |
| Other/Institutional | 248,827 | 13.1% |
 
---
 
### Extended Analysis 5 — Rider Frequency Segmentation
Unique Myki cards: **276,667**. Distribution follows a classic long-tail: 75% of cards used 5 or fewer times across the year, yet a 2.5% core of very frequent riders contributes disproportionately to total volume.
 
---
 
### Extended Analysis 6 — Route Ridership Consistency (CoV)
Coefficient of Variation (CoV = std / mean × 100) across all observed daily counts per route.
 
| Route | CoV (%) | Classification |
|-------|---------|----------------|
| Route 3 | 29.1% | Most Consistent ✅ |
| Route 75 | 34.6% | Consistent |
| Route 109 | 38.8% | Moderate |
| Route 55 | 50.9% | Volatile |
| Route 58 | **69.6%** | Most Volatile ⚠️ |
 
---
 
## 🛠️ Tech Stack
 
| Tool | Version | Purpose |
|------|---------|---------|
| Python | 3.9+ | Core language |
| Pandas | 2.0+ | Data loading, merging, aggregation |
| NumPy | 1.24+ | Numerical utilities |
| Matplotlib | 3.7+ | Chart production |
| Seaborn | 0.12+ | Statistical visualisations, colour palettes |
| Jupyter Notebook | 6.5+ | Analysis environment |
 
---
 
## 📂 Repository Structure
 
```
Melbourne-Myki-Tram-Network-Analysis-Main-2017/
│
├── 📓 Myki_analysis_2017.ipynb   # Main analysis notebook (Q1–Q6 + 6 extended analyses)
│
├── 📁 Images/                    # All chart exports referenced in the report
│   ├── fig_q2_busiest_stops.png
│   ├── fig_q3_busiest_routes.png
│   ├── fig_q4_travel_times.png
│   ├── fig_q5_hourly_patterns.png
│   ├── fig_q6_peak_diverging.png
│   ├── fig_ext1_day_of_week.png
│   ├── fig_ext2_monthly_seasonality.png
│   ├── fig_ext3_weekend_drop.png
│   ├── fig_ext4_card_type.png
│   ├── fig_ext5_rider_frequency.png
│   └── fig_ext6_cov_consistency.png
│
├── 📄 README.md                  # This file
├── 📄 LICENSE                    # MIT License
├── 📄 .gitignore
└── 📄 .gitattributes
```
 
> **Note:** The raw data files (`ScanOnTransaction.csv`, `ScanOffTransaction.csv`, `stop_locations.txt`) are **not committed** to this repository due to data licensing restrictions. Update the `DATA_DIR` variable in the notebook to point to your local copy.
 
---
 
## ⚙️ Installation & Setup
 
### Prerequisites
 
- Python 3.9 or higher
- pip or conda package manager
- The three Myki data files (sourced separately — see note above)
### Step 1 — Clone the Repository
 
```bash
git clone https://github.com/kawserutshopro321/Melbourne-Myki-Tram-Network-Analysis-Main-2017.git
cd Melbourne-Myki-Tram-Network-Analysis-Main-2017
```
 
### Step 2 — Install Dependencies
 
**Using pip:**
```bash
pip install pandas numpy matplotlib seaborn jupyter
```
 
**Using conda:**
```bash
conda create -n myki-env python=3.11
conda activate myki-env
conda install pandas numpy matplotlib seaborn jupyter
```
 
### Step 3 — Configure Data Path
 
Open `Myki_analysis_2017.ipynb` and update the `DATA_DIR` variable in the first code cell:
 
```python
DATA_DIR = r'C:\path\to\your\data\folder'   # Windows
# or
DATA_DIR = '/path/to/your/data/folder'       # macOS / Linux
```
 
Ensure the following three files are present in `DATA_DIR`:
 
```
DATA_DIR/
├── myki_2017_top20_tram_routes_ScanOnTransaction.csv
├── myki_2017_top20_tram_routes_ScanOffTransaction.csv
└── stop_locations.txt
```
 
### Step 4 — Launch the Notebook
 
```bash
jupyter notebook Myki_analysis_2017.ipynb
```
 
Then run **Kernel → Restart and Run All** to reproduce all outputs.
 
---
 
## 🧠 Methodology
 
### Data Pipeline
 
```
ScanOnTransaction.csv  ─┐
                         ├──► Enriched Touch-On ──► Joined Trip Table (1,904,721 rows × 22 cols)
stop_locations.txt     ─┘                 ↑
                                          │
ScanOffTransaction.csv ────────────────────┘
```
 
**Join key for touch-off matching:** `(card_id, business_date, parent_route)`
 
### Key Methodological Decisions
 
| Decision | Rationale |
|----------|-----------|
| **Touch-on counts as patronage proxy** | Available for 100% of trips; touch-off data covers only 32.1% |
| **Travel time filter: 1–180 min** | Removes zero-duration double-taps and forgotten touch-offs from prior days |
| **Daily averages as demand metric** | Normalises across routes with different operating calendars |
| **Median preferred over mean for travel time** | All routes exhibit right-skewed distributions (mean > median by 8–10 min) |
| **CoV for consistency measurement** | Scale-independent; enables fair comparison between high and low patronage routes |
| **Peak windows: 07:00–09:00 and 16:00–18:00** | Defined empirically from hourly demand profile in Q5, not assumed |
 
### Data Limitations
 
1. **Touch-off coverage (32.1%)** — travel-time analyses are based on a non-random subsample. Routes with very low completion rates (e.g., Route 1 at 14.7%) may produce biased travel-time estimates.
2. **Card ≠ Person** — `card_id` identifies a Myki card, not a unique individual. Multiple people may share a card; one person may own multiple cards. Rider-frequency figures are card-level.
3. **Dataset period includes an extra week** — the dataset runs to 6 January 2018, giving January slightly more observations than other months.
4. **Free Tram Zone bias** — passengers in the Free Tram Zone have no financial incentive to touch off, systematically depressing completion rates on inner-city route segments.
---
 
## 🔮 Future Work
 
- [ ] **Demand forecasting** — Apply regression or time-series models (ARIMA, Prophet) incorporating semester dates, public holidays, and weather to enable forward-looking demand prediction
- [ ] **Weather integration** — Merge Bureau of Meteorology daily weather data to explain residual variance in patronage, particularly Route 58's high CoV (69.6%)
- [ ] **Origin-Destination (OD) mapping** — Use GPS/AVL stop-sequence data to produce stop-pair flow matrices and identify key transfer corridors
- [ ] **Stop clustering** — Apply k-means or hierarchical clustering to group stops by hourly demand signature for data-driven service zone classification
- [ ] **Interactive dashboard** — Port key visualisations to Tableau or Power BI for stakeholder exploration
- [ ] **Transfer pair analysis** — Map the most frequent two-route combination days (from the 8.6% multi-route share) to identify interchange nodes requiring priority infrastructure investment
---
 
## 👤 Author
 
**MD Kawser Islam**
Business Intelligence Analyst | Data Analytics
 
- 🔗 GitHub: [@kawserutshopro321](https://github.com/kawserutshopro321)
- 🎓 Unit: SIT731 — Postgraduate Data Analysis, Deakin University
- 📧 s225586507@deakin.edu.au
---
 
## 📄 License
 
This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.
 
The underlying Myki transaction data is owned by Public Transport Victoria (PTV) / Department of Transport (DoT), Victoria, Australia and is subject to their data licensing terms. This repository contains only analytical code and outputs.
 
---
 
<div align="center">
⭐ **If you found this project useful, consider giving it a star!** ⭐
 
*Melbourne Myki Tram Network Analysis · MD Kawser Islam · SIT731 Deakin University · 2024*
 
</div>

🔗 GitHub: https://github.com/kawserutshopro321

---

## ⭐ If you found this project useful, consider giving it a star!
