# Data Cleaning & Standardization

## 1. Phase Objective

The purpose of this phase is to prepare the source datasets for dimensional modeling, ETL integration, and business intelligence reporting.

This phase focuses on:

- Improving data consistency
- Standardizing schemas
- Handling structural quality issues
- Engineering derived analytical metrics
- Preparing datasets for warehouse integration

The transformations performed during this phase are designed to preserve business-critical disruption behavior while improving analytical reliability.

---

# 2. Cleaning Strategy

The cleaning process followed a structured analytics engineering approach consisting of:

1. Global schema standardization
2. Datatype normalization
3. Temporal enrichment
4. Business rule validation
5. Derived KPI engineering
6. Final quality assurance validation

---

# 3. Global Schema Standardization

## 3.1 Column Naming Convention

All datasets were standardized using a snake_case naming convention.

### Objectives

- Improve readability
- Ensure ETL consistency
- Simplify SQL and Power BI integration
- Standardize warehouse naming practices

### Example Transformations

| Original Name           | Standardized Name       |
| ----------------------- | ----------------------- |
| Container Rate USD 40FT | container_rate_usd_40ft |
| On Time Delivery %      | on_time_delivery_pct    |
| Port Utilization %      | port_utilization_pct    |

### Engineering Benefit

Consistent naming conventions reduce transformation complexity during ETL orchestration and semantic modeling.

---

# 4. Datatype Standardization

## 4.1 Date Conversion

Date-related fields were converted into proper datetime formats to support:

- time-series analysis,
- temporal joins,
- and date dimension integration.

### Datasets Standardized

- shipping_rates
- port_congestion
- disruption_events

### Engineering Benefit

Enables reliable temporal modeling and KPI trend analysis.

---

## 4.2 Boolean & Indicator Standardization

Binary indicator fields were normalized into consistent integer formats.

### Standardized Fields

- trade_active
- supply_chain_integrated
- port_closure
- straits_affected

### Engineering Benefit

Improves compatibility with Power BI measures and dimensional warehouse design.

---

# 5. Temporal Enrichment

Temporal attributes were derived to support dimensional modeling.

## Generated Fields

- year
- month
- quarter
- rolling trend metrics

### Example

The shipping dataset was enriched with:

- year extraction
- month extraction
- rolling 6-month freight averages

### Business Value

These attributes support trend analysis and executive dashboard filtering.

---

# 6. Shipping Rates Cleaning & Enhancement

## 6.1 Freight KPI Engineering

Derived metrics were created to improve long-term trend analysis.

### Generated Metrics

- rolling_6m_container
- monthly growth indicators
- trend-supporting attributes

### Analytical Purpose

Rolling metrics reduce short-term volatility and improve disruption visualization.

---

## 6.2 Null Value Validation

Null values were evaluated carefully rather than removed automatically.

### Key Finding

Some null values represented valid baseline conditions rather than data quality failures.

### Example

Initial year-over-year calculations naturally produce baseline nulls for early periods.

### Engineering Decision

Baseline nulls were preserved to maintain analytical integrity.

---

# 7. Port Congestion Cleaning

## 7.1 Utilization Scale Normalization

Port utilization values were standardized to a consistent percentage scale.

### Transformation

Decimal-scale utilization metrics were converted into 0–100 percentage format.

### Business Value

Improves interpretability for dashboard consumers and KPI reporting.

---

## 7.2 Date Harmonization

Weekly congestion records were aligned using standardized datetime formatting.

### Engineering Benefit

Supports integration with enterprise date dimensions and trend analysis.

---

# 8. Trade Flow Cleaning

## 8.1 Active Trade Filtering

Inactive trade relationships were removed from the analytical layer.

### Business Rule

Only active trade records contribute to current analytical reporting.

### Engineering Benefit

Reduces analytical noise and improves KPI relevance.

---

## 8.2 Boolean Standardization

Trade integration indicators were normalized into warehouse-friendly formats.

### Standardized Fields

- supply_chain_integrated

### Benefit

Improves semantic consistency within the future data mart.

---

# 9. Disruption Events Cleaning

## 9.1 Severity Scoring

Categorical disruption severity labels were transformed into numerical ranking scores.

### Severity Mapping

| Severity | Score |
| -------- | ----- |
| Low      | 1     |
| Medium   | 2     |
| High     | 3     |
| Extreme  | 4     |

### Business Value

Supports quantitative severity analysis and dashboard aggregation.

---

## 9.2 Event Standardization

Disruption event attributes were standardized for consistency across analytical reporting.

### Standardized Attributes

- disruption_type
- severity
- closure indicators

---

# 10. Industry Exposure Cleaning

## 10.1 Vulnerability Score Validation

Exposure and vulnerability metrics were normalized within a standardized scoring range.

### Rule Applied

All exposure metrics constrained to a 0–10 scale.

### Engineering Benefit

Improves comparability across industries and dashboards.

---

# 11. Data Quality Validation Framework

A formal validation framework was implemented to ensure dataset integrity.

## Validation Checks Performed

| Validation Type      | Description                 |
| -------------------- | --------------------------- |
| Null Validation      | Detection of missing values |
| Duplicate Validation | Duplicate row detection     |
| Logic Validation     | Business rule consistency   |
| Datatype Validation  | Standardized data typing    |
| Range Validation     | Metric boundary enforcement |

---

## Example Validation Rules

### Port Utilization Validation

- Utilization must remain within acceptable operational thresholds

### Trade Validation

- Only active trade records included

### Baseline Validation

- Baseline temporal nulls preserved intentionally

---

# 12. Final Quality Assessment

The cleaned datasets demonstrate:

- consistent schemas,
- reliable datatypes,
- standardized naming conventions,
- and validated business logic.

## Quality Outcome Summary

| Quality Area           | Status |
| ---------------------- | ------ |
| Schema Consistency     | Passed |
| Naming Standardization | Passed |
| Temporal Alignment     | Passed |
| KPI Integrity          | Passed |
| Validation Rules       | Passed |

---

# 13. Engineering Readiness for ETL

The cleaned datasets are now prepared for:

- Talend ETL integration
- Dimensional modeling
- Fact and dimension generation
- Power BI semantic modeling
- KPI dashboard development

---

# 14. Deliverables Produced

The following outputs were generated during this phase:

- Data cleaning notebook
- Standardized cleaned CSV datasets
- Derived analytical metrics
- Validation rule framework
- Quality assurance checks

---

# 15. Conclusion

The data cleaning and standardization phase successfully transformed the raw source datasets into structured, analytics-ready assets.

The transformation process improved:

- consistency,
- reliability,
- integration readiness,
- and analytical usability.

The datasets are now fully prepared for dimensional modeling and ETL pipeline development in the next project phase.
