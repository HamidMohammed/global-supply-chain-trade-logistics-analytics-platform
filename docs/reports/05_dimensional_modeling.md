# 05 --- Dimensional Modeling Design

# 1\. Enterprise Bus Matrix

The project follows a Kimball Bus Architecture approach where multiple analytical fact tables share standardized conformed dimensions.

This architecture ensures:

- consistent KPI calculations,

- reusable analytical dimensions,

- scalable reporting,

- semantic consistency,

- and integrated cross-domain analytics.

---

## Enterprise Bus Matrix

| Business Process          | Fact Table             | Date | Geography | Industry | Trade Category | Disruption Era |
| ------------------------- | ---------------------- | ---- | --------- | -------- | -------------- | -------------- |
| Freight Market Monitoring | fact_freight_market    | ✔    |           |          |                | ✔              |
| Port Operations           | fact_port_logistics    | ✔    | ✔         |          |                | ✔              |
| Trade Activity            | fact_trade_volume      | ✔    | ✔         | ✔        | ✔              | ✔              |
| Industry Risk Exposure    | fact_industry_exposure | ✔    |           | ✔        |                | ✔              |

---

# 2\. Dimension Design Strategy

The warehouse follows a Kimball dimensional modeling methodology where:

- dimensions provide descriptive business context,

- fact tables store measurable operational activity,

- and conformed dimensions enable cross-domain analytical consistency.

The dimensional architecture emphasizes:

- star schema simplicity,

- deterministic joins,

- centralized business semantics,

- and Power BI optimization.

The project intentionally avoids:

- excessive snowflake normalization,

- overlapping event joins,

- ambiguous many-to-many relationships,

- and unnecessary dimensional complexity.

---

# 3\. Conformed Dimensions

# 3.1 Dim_Date

## Business Purpose

The Date dimension provides standardized temporal analysis across all business processes and analytical fact tables.

It enables:

- trend analysis,

- period comparison,

- disruption-era segmentation,

- and time-based KPI aggregation.

---

## Grain

One row per calendar date.

---

## Primary Key

date_key (integer surrogate key in YYYYMMDD format)

---

## Source Systems

Generated internally during ETL processing.

---

## Core Attributes

| Attribute   | Description       |
| ----------- | ----------------- |
| date_key    | Surrogate key     |
| full_date   | Calendar date     |
| year        | Calendar year     |
| quarter     | Quarter number    |
| month       | Month number      |
| month_name  | Full month name   |
| week_number | ISO week number   |
| day_of_week | Weekday name      |
| is_weekend  | Weekend indicator |

---

## Analytical Role

Conformed dimension shared across all analytical fact tables.

---

# 3.2 Dim_Geography

## Business Purpose

Provides standardized geographical classifications for global port congestion and logistics analytics.

Supports:

- regional congestion monitoring,

- operational bottleneck analysis,

- logistics KPI segmentation,

- and executive operational reporting.

---

## Grain

One row per unique global port.

---

## Primary Key

geo_sk

---

## Source Systems

Derived from:

- cleaned_port_congestion.csv

Regional classifications were enriched programmatically during preprocessing.

---

## Core Attributes

| Attribute          | Description                      |
| ------------------ | -------------------------------- |
| geo_sk             | Surrogate key                    |
| port               | Standardized port name           |
| region             | Global geographical region       |
| strategic_hub_flag | Major logistics hub indicator    |
| trade_route_type   | Pacific / Atlantic / Asia-Europe |

---

## Hierarchy Structure

Region → Port

This hierarchy enables:

- regional aggregation,

- port-level drill-down,

- and dashboard filtering.

---

## Engineering Notes

The source datasets do not contain extensive geographic hierarchy metadata.

To support dimensional consistency:

- region classifications were programmatically derived,

- port names were standardized,

- and surrogate keys were introduced for warehouse integration.

The dimension intentionally remains lightweight to preserve:

- star schema simplicity,

- Power BI performance,

- and ETL maintainability.

---

## Analytical Role

Conformed operational dimension supporting:

- fact_port_logistics

- fact_trade_volume

---

# 3.3 Dim_Industry

## Business Purpose

The Dim_Industry dimension provides standardized industry classifications for analyzing:

- supply chain vulnerability,

- operational dependency,

- geopolitical exposure,

- and disruption sensitivity across global industries.

The dimension transforms raw exposure metrics into reusable business-oriented analytical categories optimized for:

- executive dashboard reporting,

- resilience benchmarking,

- risk segmentation,

- and strategic supply chain intelligence.

---

## Grain

One row per unique industry sector.

---

## Primary Key

industry_sk

---

## Source Systems

Derived primarily from:

- cleaned_industry_exposure.csv

- cleaned_trade_flows.csv

---

## Core Attributes

| Attribute                 | Description                                |
| ------------------------- | ------------------------------------------ |
| industry_sk               | Surrogate key                              |
| industry_name             | Standardized industry name                 |
| industry_group            | High-level sector grouping                 |
| vulnerability_band        | Overall vulnerability classification       |
| risk_profile              | Strategic operational risk category        |
| logistics_sensitivity     | Logistics dependency classification        |
| is_pandemic_sensitive     | Pandemic-sensitive industry flag           |
| geopolitical_risk         | Geopolitical exposure score                |
| tariff_impact             | Tariff exposure score                      |
| overall_vulnerability     | Composite vulnerability score              |
| resilience_level          | Operational resilience classification      |
| supply_chain_dependency   | Supplier and JIT dependency classification |
| disruption_exposure_level | Composite disruption exposure category     |
| nearshoring_readiness     | Nearshoring adaptation readiness           |
| strategic_industry_flag   | Critical strategic industry indicator      |
| operational_risk_driver   | Dominant operational disruption source     |

---

## Industry Hierarchy Structure

Industry Group → Industry Name

Example:

- Technology → Semiconductors

- Manufacturing → Automotive

- Consumer Goods → Retail & Apparel

---

## Derived Business Classifications

### Vulnerability Band

| Band   | Logic                                 |
| ------ | ------------------------------------- |
| High   | Overall vulnerability ≥ 7             |
| Medium | Overall vulnerability between 5 and 7 |
| Low    | Overall vulnerability < 5             |

---

### Risk Profile

| Profile  | Description                     |
| -------- | ------------------------------- |
| Critical | Severe operational fragility    |
| High     | Elevated disruption sensitivity |
| Moderate | Manageable operational risk     |

---

### Logistics Sensitivity

| Classification | Logic                              |
| -------------- | ---------------------------------- |
| Extreme        | Logistics exposure ≥ 7             |
| High           | Logistics exposure between 5 and 7 |
| Standard       | Logistics exposure < 5             |

---

### Resilience Level

| Level     | Logic                                   |
| --------- | --------------------------------------- |
| Fragile   | Overall vulnerability ≥ 7.5             |
| Sensitive | Overall vulnerability between 6 and 7.5 |
| Resilient | Overall vulnerability < 6               |

---

### Supply Chain Dependency

Derived using:

- Just-in-Time dependency

- Supplier concentration exposure

| Dependency Level    | Criteria                           |
| ------------------- | ---------------------------------- |
| High Dependency     | High JIT or concentrated suppliers |
| Moderate Dependency | Moderate operational dependency    |
| Low Dependency      | Distributed sourcing               |

---

### Disruption Exposure Level

Composite classification derived from:

- geopolitical exposure,

- tariff exposure,

- and natural disaster exposure.

| Exposure Level      | Description                     |
| ------------------- | ------------------------------- |
| Severe Exposure     | Extremely disruption-sensitive  |
| Elevated Exposure   | Moderate exposure concentration |
| Controlled Exposure | Lower operational exposure      |

---

### Nearshoring Readiness

| Classification        | Description                            |
| --------------------- | -------------------------------------- |
| Nearshore Ready       | Strong regional supply chain readiness |
| Transition Candidate  | Partial adaptation readiness           |
| Globalized Dependency | Heavy global sourcing dependency       |

---

## Engineering Design Decisions

Several analytics engineering principles were implemented during dimension construction.

### Centralized Business Logic

Business classifications are generated during ETL processing rather than inside Power BI.

Benefits include:

- semantic consistency,

- reusable KPI logic,

- reduced DAX complexity,

- and improved maintainability.

---

### Denormalized Analytical Design

The dimension intentionally stores enriched descriptive attributes directly inside the dimension table.

Benefits include:

- simplified semantic modeling,

- faster analytical querying,

- and improved dashboard usability.

---

## Analytical Role

Conformed strategic dimension supporting:

- fact_industry_exposure

- fact_trade_volume

The dimension enables:

- cross-industry comparison,

- resilience benchmarking,

- disruption storytelling,

- and executive reporting.

---

# 3.4 Dim_Disruption_Era

## Business Purpose

The Dim_Disruption_Era dimension provides standardized historical segmentation for analyzing global supply chain behavior across major disruption periods.

The dimension enables:

- long-term historical comparison,

- disruption-driven KPI segmentation,

- macro-level trend analysis,

- and executive storytelling.

Unlike individual disruption events, disruption eras represent stable non-overlapping historical periods, making them highly suitable for dimensional modeling.

---

## Grain

One row per disruption era.

---

## Primary Key

era_sk

---

## Source Systems

Derived from:

- cleaned_shipping_rates.csv

- cleaned_disruption_events.csv

The dimension is internally engineered during ETL processing.

---

## Era Definitions

| Era                     | Period     |
| ----------------------- | ---------- |
| Normal                  | 2000--2007 |
| GFC Shock               | 2008--2009 |
| Stagnation              | 2010--2019 |
| COVID Spike             | 2020--2022 |
| Normalization + Red Sea | 2023--2024 |

---

## Core Attributes

| Attribute            | Description                              |
| -------------------- | ---------------------------------------- |
| era_sk               | Surrogate key                            |
| era_name             | Historical disruption era                |
| start_year           | Era start year                           |
| end_year             | Era end year                             |
| defining_event       | Primary defining event                   |
| market_behavior      | Stable / Shock / Recovery classification |
| freight_environment  | Freight volatility classification        |
| congestion_profile   | Typical congestion behavior              |
| trade_environment    | Global trade condition                   |
| supply_chain_state   | Overall operational condition            |
| disruption_intensity | Aggregate disruption severity            |

---

## Design Rationale

The disruption datasets contain multiple overlapping events occurring simultaneously across different regions and timeframes.

Direct event-level joins introduce:

- many-to-many relationships,

- duplicate aggregations,

- KPI inflation,

- and Power BI ambiguity.

To solve this issue, the warehouse introduces a higher-level historical segmentation dimension:

Dim_Disruption_Era.

Each calendar period belongs to exactly one disruption era, enabling:

- clean one-to-many joins,

- stable KPI aggregation,

- simplified filtering,

- and efficient dashboard slicing.

---

## Relationship Strategy

The dimension connects directly to:

- fact_freight_market

- fact_port_logistics

- fact_trade_volume

- fact_industry_exposure

through:

- era_sk

---

## Analytical Benefits

The dimension enables:

- disruption-era comparison,

- freight normalization analysis,

- operational recovery analysis,

- macroeconomic segmentation,

- and executive historical storytelling.

---

## Power BI Benefits

Using Dim_Disruption_Era:

- avoids many-to-many relationships,

- simplifies DAX calculations,

- improves slicer usability,

- stabilizes KPI aggregation,

- and improves dashboard performance.

---

## Final Design Decision

The project intentionally separates:

- granular disruption events\
  from:

- macro historical operational periods.

This improves:

- scalability,

- maintainability,

- analytical clarity,

- and warehouse integrity.

---

# 3.5 Dim_Trade_Category

## Business Purpose

Provides standardized trade classification categories for analyzing:

- bilateral trade behavior,

- commodity exposure,

- tariff sensitivity,

- and strategic supply chain dependencies.

---

## Grain

One row per unique trade category.

---

## Primary Key

trade_category_sk

---

## Source Systems

Derived from:

- cleaned_trade_flows.csv

---

## Core Attributes

| Attribute            | Description                            |
| -------------------- | -------------------------------------- |
| trade_category_sk    | Surrogate key                          |
| trade_category       | Trade sector classification            |
| key_goods            | Major traded commodities               |
| market_type          | Commodity / Manufacturing / Technology |
| volatility_profile   | Stable / Cyclical / Volatile           |
| strategic_importance | Criticality classification             |

---

## Engineering Notes

The dimension centralizes commodity and trade descriptors into reusable analytical structures.

This avoids:

- repeated textual attributes,

- inconsistent trade labeling,

- and oversized fact tables.

---

## Analytical Role

Conformed business dimension supporting:

- fact_trade_volume

---

# 4\. Dimensional Modeling Principles Applied

The warehouse implementation follows core Kimball dimensional modeling principles.

## Principles Applied

- conformed dimensions,

- star schema architecture,

- surrogate key integration,

- business-aligned dimensional semantics,

- strict fact table grain enforcement,

- and centralized KPI logic.

The design intentionally avoids:

- excessive normalization,

- snowflake complexity,

- ambiguous relationship paths,

- and duplicated business logic inside Power BI.

---

# 5\. Power BI Optimization Considerations

The dimensional architecture was optimized for Power BI semantic modeling.

Benefits include:

- simplified DAX calculations,

- efficient slicer propagation,

- stable relationship behavior,

- and scalable dashboard performance.

The use of Dim_Disruption_Era instead of direct event-level
