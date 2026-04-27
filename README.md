# 🚋 Melbourne Myki Tram Network Analysis (2017)

![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458.svg)
![NumPy](https://img.shields.io/badge/NumPy-Numerical%20Computing-013243.svg)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-orange.svg)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626.svg)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

---

## 📌 Overview

This project presents a comprehensive data analysis of Melbourne’s tram network using **Myki smart card data (2017)**. The analysis focuses on uncovering **passenger demand patterns, route performance, rider behavior, and operational insights** to support data-driven transport planning.

The dataset includes:

* **1.9M+ touch-on transactions**
* **600K+ touch-off records**
* Coverage across **20 tram routes**
* Time period: **Jan 2017 – Jan 2018**

---

## 🎯 Objectives

* Analyze **temporal demand patterns** (daily, weekly, monthly)
* Identify **high-demand stops and routes**
* Evaluate **peak vs off-peak usage**
* Understand **passenger segmentation and behavior**
* Measure **data quality (touch-off completion)**
* Explore **network efficiency and transfer behavior**

---

## 📊 Key Insights

### 🚦 Demand & Usage

* **Friday is the busiest day**, with ~2.2× higher demand than Sunday
* **November peak demand is 93% higher than June**, driven by university cycles
* Tram usage is **heavily commuter-driven**

### 🗺️ Network Performance

* **Route 109** is the busiest route (1.6× higher than others)
* Demand is **CBD-centric**, especially around universities and transit hubs
* Top stop demand drops **3.4× from rank 1 to rank 10**

### ⏰ Peak Patterns

* **Afternoon peak (16:00–18:00)** is ~14% higher than morning
* **85% of routes** experience higher afternoon demand
* Route 59 is a **key anomaly (morning-heavy demand)**

### 👥 Passenger Behavior

* **75% of users are low-frequency riders (≤5 trips/year)**
* **2.5% of users drive a large portion of demand**
* Only **8.6% of trips involve transfers**, indicating efficient direct routing

### ⚠️ Data Quality

* Only **32.1% of trips include touch-off data**
* Free Tram Zone reduces incentive to tap off → impacts travel time analysis

---

## 🧠 Analytical Approach

### Data Processing

* Cleaned and filtered invalid records
* Joined touch-on and touch-off data using:

  * `card_id`
  * `date`
  * `route`

### Feature Engineering

* Derived:

  * Travel time (minutes)
  * Peak vs off-peak windows
  * Day-of-week and monthly aggregations
  * Route-level metrics (mean, std, CoV)

### Metrics Used

* **Average Daily Patronage**
* **Peak-to-Off-Peak Ratio**
* **Coefficient of Variation (CoV)**
* **Touch-Off Completion Rate**
* **Rider Frequency Distribution**

---

## 📈 Key Analyses Performed

* Top 10 busiest tram stops
* Route-level demand comparison
* Travel time distribution analysis
* Hourly demand patterns
* Morning vs afternoon peak comparison
* Weekday vs weekend behavior
* Monthly seasonality trends
* Passenger segmentation
* Transfer (multi-route) behavior

---

## 🛠️ Tech Stack

* **Python (Pandas, NumPy)**
* **Matplotlib / Seaborn**
* **Jupyter Notebook**
* **Statistical Analysis**

---

## 📂 Project Structure

```
├── data/
├── notebooks/
│   └── SIT731_Task4P.ipynb
├── report/
│   └── Melbourne_myki_analysis_2017.docx
├── images/
├── README.md
```

---

## 🚀 How to Run

```bash
git clone https://github.com/kawserutshopro321/Melbourne-Myki-Tram-Network-Analysis-Main-2017.git
cd Melbourne-Myki-Tram-Network-Analysis-Main-2017
pip install pandas numpy matplotlib seaborn
jupyter notebook
```

---

## 📌 Business Impact

This analysis supports:

* **Transport authorities** → optimize scheduling
* **Urban planners** → demand concentration insights
* **Revenue teams** → fare structure evaluation
* **Operations teams** → peak load management

---

## 🔮 Future Improvements

* Add **forecasting models (ML)**
* Integrate **weather data**
* Build **interactive dashboards (Tableau/Power BI)**
* Perform **OD (origin-destination) analysis**

---

## 👤 Author

**Md. Kawser Islam**
Business Intelligence Analyst

🔗 GitHub: https://github.com/kawserutshopro321

---

## ⭐ If you found this project useful, consider giving it a star!
