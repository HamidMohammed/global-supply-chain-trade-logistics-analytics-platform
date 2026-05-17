# Exploratory Data Analysis (EDA) & Statistical Profiling

## 1. Phase Objective

The purpose of this phase is to perform exploratory data analysis on the integrated supply chain datasets in order to:

- Understand historical behavior and trends
- Detect anomalies and disruptions
- Evaluate operational patterns
- Identify correlations between KPIs
- Support future dimensional modeling and dashboard design

This phase combines statistical profiling with business-oriented analytical interpretation.

---

# 2. EDA Scope

The exploratory analysis focused on the following datasets:

| Dataset               | Analysis Focus                         |
| --------------------- | -------------------------------------- |
| shipping_rates.csv    | Freight volatility and market pressure |
| port_congestion.csv   | Operational congestion behavior        |
| trade_flows.csv       | Trade growth and tariff impact         |
| disruption_events.csv | Event severity and recovery patterns   |
| industry_exposure.csv | Vulnerability and risk exposure        |

---

# 3. Shipping Rates Analysis

## 3.1 Historical Freight Trend Analysis

The historical freight analysis revealed a long period of relative stability between 2000 and 2019, followed by an unprecedented spike during the COVID-19 disruption era.

### Key Findings

- Container rates remained near baseline levels for nearly two decades
- Freight pricing experienced exponential growth during 2020–2022
- Shipping markets demonstrated extreme volatility during global disruption periods

### Business Interpretation

This confirms the structural transformation of global supply chains from stable "Just-in-Time" operations into highly disruption-sensitive logistics systems.

---

## 3.2 Supply Chain Pressure Index Behavior

The Supply Chain Pressure Index showed strong alignment with periods of elevated freight pricing and operational stress.

### Key Findings

- Pressure index peaks align directly with COVID-era congestion
- High pressure periods correspond to reduced delivery reliability
- Market normalization begins after late 2022

### Engineering Implication

The pressure index is a high-value analytical KPI candidate for dashboarding and trend monitoring.

---

## 3.3 Correlation Analysis

A correlation heatmap was generated to evaluate relationships between operational shipping metrics.

### Key Findings

- Strong positive correlation between container rates and pressure index
- Moderate relationship between freight rates and delivery performance
- Lower correlation observed between air cargo and bulk shipping indicators

### Business Interpretation

The results suggest that ocean freight disruption is more sensitive to systemic congestion than air cargo markets.

---

## 3.4 Distribution & Outlier Analysis

Distribution analysis revealed significant right-skewness in freight pricing metrics.

### Key Findings

- COVID-era records represent extreme operational outliers
- Outliers are business-significant events rather than data errors
- Freight volatility increased dramatically after 2020

### Engineering Implication

Outlier handling should preserve disruption events rather than remove them during preprocessing.

---

# 4. Port Congestion Analysis

## 4.1 Congestion Severity Analysis

Port congestion metrics revealed sustained operational bottlenecks during major disruption periods.

### Key Findings

- Significant increases in vessel waiting times occurred during 2021–2022
- Major global ports experienced persistent congestion plateaus
- Port utilization remained above historical baselines for extended periods

### Business Interpretation

The findings demonstrate the fragility of globally interconnected shipping hubs.

---

## 4.2 Strategic Port Analysis

Several ports emerged as critical global bottlenecks.

### Key Findings

- High-volume ports demonstrated disproportionate congestion impact
- Congestion in strategic hubs amplified global freight instability
- Operational recovery periods varied significantly by port

### Analytical Value

These ports become strong candidates for operational KPI dashboards and geographical analysis.

---

## 4.3 Port Utilization Trend Analysis

Time-series analysis identified prolonged periods of elevated port utilization.

### Key Findings

- Utilization exceeded normal operational thresholds during disruption periods
- Recovery patterns were gradual rather than immediate
- Persistent utilization spikes aligned with freight cost escalation

### Business Interpretation

Sustained high utilization reflects reduced operational elasticity across global logistics networks.

---

# 5. Rolling Trend & Smoothing Analysis

Rolling averages were calculated to reduce short-term volatility and identify long-term market behavior.

### Key Findings

- Rolling trends clearly isolate disruption eras
- Long-term freight escalation becomes more visible after smoothing
- Market recovery periods remain volatile even after normalization

### Engineering Implication

Rolling metrics are valuable derived KPIs for the analytical data mart.

---

# 6. Peak Stress Event Analysis

Peak-stress months were identified using freight and congestion metrics.

### Key Findings

- Highest stress periods align with COVID-era supply shortages
- Freight spikes coincide with elevated congestion indicators
- Disruption recovery required extended stabilization periods

### Business Interpretation

The data confirms that global disruptions generate compound operational effects across multiple supply chain dimensions.

---

# 7. Statistical Profiling Summary

| Analysis Area          | Observation                            |
| ---------------------- | -------------------------------------- |
| Trend Stability        | Stable pre-2020 market conditions      |
| Outlier Presence       | Significant disruption-driven outliers |
| Correlation Strength   | Strong logistics KPI interdependence   |
| Distribution Shape     | Right-skewed operational metrics       |
| Operational Congestion | Severe during disruption periods       |

---

# 8. Key Business Insights

The EDA phase generated several important business insights:

- COVID-19 represented the most disruptive supply chain event in the dataset
- Freight markets became highly sensitive to congestion and operational shocks
- Strategic ports acted as global bottlenecks during disruption periods
- Recovery timelines extended beyond the initial disruption window
- Supply chain stress indicators strongly correlate with freight inflation

---

# 9. Engineering Implications for ETL & Modeling

The exploratory analysis directly informs the next engineering phases.

## Key Implications

- Temporal modeling is critical due to mixed granularities
- Rolling metrics should be incorporated into fact tables
- Outlier preservation is required for disruption analysis
- Time-series KPIs should support dashboard storytelling
- Derived business metrics add analytical value

---

# 10. Deliverables Produced

The following outputs were produced during this phase:

- Python EDA notebook
- Correlation analysis
- Time-series trend visualizations
- Outlier profiling
- Congestion analysis
- Business insight summaries

---

# 11. Conclusion

The exploratory analysis confirms that the datasets provide strong analytical value for supply chain intelligence and operational monitoring.

The datasets demonstrate:

- meaningful disruption patterns,
- strong KPI relationships,
- and excellent suitability for dimensional modeling and business intelligence reporting.

The next phase will focus on data cleaning, standardization, and preprocessing for ETL integration.
