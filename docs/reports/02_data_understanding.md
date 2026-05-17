# Data Understanding & Source System Analysis

## 1. Phase Objective

The purpose of this phase is to analyze the structure, granularity, and quality of all source datasets before beginning transformation and dimensional modeling activities.

This phase focuses on:

- Understanding dataset relationships
- Identifying analytical grain
- Detecting data quality issues
- Evaluating integration opportunities
- Preparing the foundation for ETL and star schema modeling

---

# 2. Source System Overview

The project dataset consists of five CSV-based source systems representing various aspects of global supply chain operations and trade disruptions between 2000 and 2024.

| Dataset               | Description                                     | Granularity   |
| --------------------- | ----------------------------------------------- | ------------- |
| shipping_rates.csv    | Monthly freight and shipping indicators         | Monthly       |
| port_congestion.csv   | Weekly congestion metrics across major ports    | Weekly        |
| trade_flows.csv       | Bilateral trade relationships between countries | Annual        |
| disruption_events.csv | Major supply chain disruption events            | Event-level   |
| industry_exposure.csv | Industry vulnerability exposure scores          | Industry-Year |

---

# 3. Dataset-by-Dataset Analysis

## 3.1 Shipping Rates Dataset

### Business Purpose

Tracks historical freight market conditions, shipping costs, and overall supply chain pressure indicators.

### Granularity

One row per month.

### Key Metrics

- Baltic Dry Index (BDI)
- Container Freight Rate
- Air Cargo Rate
- Supply Chain Pressure Index
- On-Time Delivery Percentage

### Initial Observations

- Strong time-series structure
- Suitable for trend analysis
- Useful as a central analytical fact table
- Contains multiple derived metrics already calculated

### Potential Issues

- Requires date standardization
- Some metrics may require datatype conversion
- Temporal alignment needed with weekly datasets

---

## 3.2 Port Congestion Dataset

### Business Purpose

Measures operational congestion across major international ports.

### Granularity

One row per port per week.

### Key Metrics

- Average Wait Days
- Vessels at Anchor
- Port Utilization %
- Congestion Level

### Initial Observations

- High analytical value for operational KPIs
- Strong candidate for operational fact table
- Enables trend and disruption impact analysis

### Potential Issues

- Weekly granularity differs from monthly datasets
- Potential naming inconsistencies across ports
- Date harmonization required

---

## 3.3 Trade Flows Dataset

### Business Purpose

Represents bilateral trade relationships between exporting and importing countries.

### Granularity

One row per exporter-importer relationship per year.

### Key Metrics

- Trade Value (Billion USD)
- YoY Growth %
- Effective Tariff Rate
- Trade Activity Flag

### Initial Observations

- Highly valuable for geopolitical analysis
- Strong dimensional modeling opportunities
- Suitable for trade analytics dashboards

### Potential Issues

- Annual granularity differs from other datasets
- Country standardization may be required
- Trade categories may require normalization

---

## 3.4 Disruption Events Dataset

### Business Purpose

Captures major global disruption events affecting supply chains.

### Granularity

One row per disruption event.

### Key Metrics

- Disruption Type
- Severity
- Freight Shock %
- Recovery Months
- Port Closure Indicator

### Initial Observations

- Critical contextual dataset
- Enables event-driven analytics
- Supports disruption era classification

### Potential Issues

- Event dates may require normalization
- Severity classifications should be standardized
- Some binary indicators may require datatype conversion

---

## 3.5 Industry Exposure Dataset

### Business Purpose

Measures industry-level vulnerability to supply chain disruptions.

### Granularity

One row per industry per year.

### Key Metrics

- Vulnerability Score
- Geopolitical Exposure
- Logistics Dependency
- Just-in-Time Dependency

### Initial Observations

- Excellent for risk analytics
- Enables cross-industry comparison
- Supports strategic business storytelling

### Potential Issues

- Score normalization validation needed
- Potential categorical standardization required

---

# 4. Data Quality Assessment

The initial audit process included:

- Schema inspection
- Datatype validation
- Missing value analysis
- Duplicate detection
- Descriptive statistical profiling

## Preliminary Findings

| Quality Check      | Observation                         |
| ------------------ | ----------------------------------- |
| Missing Values     | Minimal missing values observed     |
| Duplicate Rows     | Very low duplicate presence         |
| Schema Consistency | Mostly structured and standardized  |
| Granularity        | Mixed temporal granularity detected |
| Naming Standards   | Minor normalization may be required |

---

# 5. Granularity Analysis

One of the most important findings from this phase is the existence of multiple temporal granularities across datasets.

| Dataset           | Granularity |
| ----------------- | ----------- |
| Shipping Rates    | Monthly     |
| Port Congestion   | Weekly      |
| Trade Flows       | Annual      |
| Disruption Events | Event-level |
| Industry Exposure | Yearly      |

## Engineering Implication

This requires:

- date dimension standardization,
- temporal alignment strategies,
- and careful fact table design during dimensional modeling.

---

# 6. Preliminary Relationship Mapping

The datasets demonstrate several integration opportunities:

| Source Dataset  | Join Candidate    | Join Type            |
| --------------- | ----------------- | -------------------- |
| shipping_rates  | disruption_events | Date-based           |
| port_congestion | disruption_events | Date-based           |
| trade_flows     | industry_exposure | Industry/Year        |
| trade_flows     | shipping_rates    | Year/Date            |
| all datasets    | dim_date          | Temporal Integration |

---

# 7. Potential Fact and Dimension Candidates

## Potential Fact Tables

- Fact_Shipping_Rates
- Fact_Port_Congestion
- Fact_Trade_Flows
- Fact_Disruption_Events

## Potential Dimensions

- Dim_Date
- Dim_Port
- Dim_Country
- Dim_Industry
- Dim_Disruption_Type
- Dim_Trade_Category

---

# 8. Key Engineering Considerations

Several important engineering considerations were identified:

- Temporal granularity alignment
- Standardized naming conventions
- Surrogate key generation
- Historical trend preservation
- Integration of event-driven datasets
- KPI consistency across analytical layers

---

# 9. Deliverables Produced

The following outputs were generated during this phase:

- Python auditing notebook
- Schema inspection results
- Dataset profiling summaries
- Initial quality assessment
- Preliminary relationship analysis

---

# 10. Conclusion

The source datasets are well-structured and highly suitable for analytical engineering workflows.

The data demonstrates:

- strong business relevance,
- rich analytical potential,
- and excellent compatibility with dimensional modeling techniques.

The next phase will focus on Exploratory Data Analysis (EDA) and detailed statistical profiling using Python.
