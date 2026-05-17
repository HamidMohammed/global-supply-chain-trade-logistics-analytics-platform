# 06 --- Talend ETL Pipeline Architecture & Implementation

## 1\. Phase Objective

The purpose of this phase is to implement an enterprise-style ETL pipeline using Talend in order to:

- ingest cleaned source datasets,

- apply business transformations,

- generate analytical dimensions,

- construct warehouse-ready fact tables,

- and prepare curated outputs for Power BI reporting.

The ETL layer acts as the core integration engine of the Global Supply Chain & Trade Disruption Analytics Platform.

---

# 2\. ETL Architecture Overview

The ETL workflow follows a layered analytical architecture.

| Layer              | Description                           |
| ------------------ | ------------------------------------- |
| Raw Layer          | Original operational CSV datasets     |
| Cleaned Layer      | Standardized and validated datasets   |
| Curated Layer      | Analytical dimensions and fact tables |
| Presentation Layer | Power BI semantic reporting layer     |

---

# 3\. ETL Workflow Strategy

The Talend implementation consists of modular ETL pipelines responsible for:

- conformed dimension generation,

- analytical fact table preparation,

- business-rule transformation,

- KPI enrichment,

- and warehouse export generation.

The ETL architecture emphasizes:

- centralized business logic,

- reusable transformations,

- dimensional consistency,

- and scalable analytical integration.

---

# 4\. Source Dataset Ingestion

The ETL process ingests cleaned datasets generated during the preprocessing and standardization phases.

## Source Datasets

| Source File                   | Purpose                          |
| ----------------------------- | -------------------------------- |
| cleaned_shipping_rates.csv    | Freight market analytics         |
| cleaned_port_congestion.csv   | Port operational metrics         |
| cleaned_trade_flows.csv       | Bilateral trade analytics        |
| cleaned_disruption_events.csv | Historical disruption monitoring |
| cleaned_industry_exposure.csv | Industry vulnerability analysis  |

---

# 5\. ETL Pipeline Categories

| Pipeline Type          | Purpose                           |
| ---------------------- | --------------------------------- |
| Dimension Pipelines    | Generate conformed dimensions     |
| Fact Loading Pipelines | Build analytical fact tables      |
| Validation Pipelines   | Perform data quality verification |
| Enrichment Pipelines   | Generate derived business KPIs    |

---

# 6\. Dimension ETL Pipelines

# 6.1 Dim_Date Pipeline

## Objective

Generate a complete calendar dimension supporting standardized temporal analytics across all business processes.

---

## Talend Components Used

| Component            | Purpose                            |
| -------------------- | ---------------------------------- |
| tRowGenerator        | Generate continuous calendar dates |
| tMap                 | Attribute derivation               |
| tFileOutputDelimited | Curated dimension export           |

---

## Key Transformations

Generated:

- year,

- quarter,

- month,

- week number,

- weekday,

- and weekend indicators.

Surrogate keys were generated using:

`tMap Numeric.sequence()`

---

## Output

- dim_date.csv

---

# 6.2 Dim_Geography Pipeline

## Objective

Create a conformed geography dimension supporting global port and regional logistics analytics.

---

## Talend Components Used

| Component            | Purpose                   |
| -------------------- | ------------------------- |
| tFileInputDelimited  | CSV ingestion             |
| tUniqRow             | Duplicate removal         |
| tMap                 | Geography standardization |
| tFileOutputDelimited | Curated dimension export  |

---

## Key Transformations

- Standardized port names

- Applied regional mapping logic

- Removed duplicate records

- Generated surrogate keys

---

## Engineering Notes

The geography dimension intentionally remains lightweight to:

- preserve star schema simplicity,

- improve Power BI performance,

- and reduce ETL complexity.

---

## Output

- dim_geography.csv

---

# 6.3 Dim_Industry Pipeline

## Objective

Generate a business-enriched industry dimension supporting:

- vulnerability segmentation,

- resilience analytics,

- operational dependency monitoring,

- and executive risk reporting.

---

## Talend Components Used

| Component            | Purpose                                    |
| -------------------- | ------------------------------------------ |
| tFileInputDelimited  | CSV ingestion                              |
| tUniqRow             | Deduplication                              |
| tMap                 | Business transformation and classification |
| tFileOutputDelimited | Curated dimension export                   |

---

## Key Transformations

Several derived analytical classifications were generated during ETL processing.

### Vulnerability Classification

Industries categorized into:

- High

- Medium

- Low

based on operational exposure metrics.

---

### Resilience Segmentation

Derived classifications:

- Fragile

- Sensitive

- Resilient

using:

- overall vulnerability,

- supplier concentration,

- and Just-in-Time dependency indicators.

---

### Supply Chain Dependency

Operational dependency categories generated using:

- supplier concentration,

- and JIT operational exposure.

### Generated Categories

- High Dependency

- Moderate Dependency

- Low Dependency

---

### Disruption Exposure Level

Composite disruption classifications generated using:

- geopolitical exposure,

- tariff exposure,

- and natural disaster exposure.

### Generated Categories

- Severe Exposure

- Elevated Exposure

- Controlled Exposure

---

### Nearshoring Readiness

Strategic nearshoring classifications generated using normalized readiness scores.

### Generated Categories

- Nearshore Ready

- Transition Candidate

- Globalized Dependency

---

### Operational Risk Driver

Primary disruption source identified using:

- geopolitical exposure,

- logistics exposure,

- and energy exposure.

### Example Drivers

- Geopolitical

- Logistics

- Energy

---

## Surrogate Key Generation

Talend generated surrogate keys using:

`tMap Numeric.sequence()`

### Purpose

- support warehouse-style joins,

- improve dimensional consistency,

- and simplify Power BI relationships.

---

## Engineering Design Decisions

The ETL process intentionally centralizes business logic inside Talend rather than Power BI.

### Benefits

- reusable classifications,

- reduced DAX complexity,

- improved maintainability,

- and consistent analytical semantics.

---

## Output

- dim_industry.csv

---

# 6.4 Dim_Disruption_Era Pipeline

## Objective

Generate a historical segmentation dimension supporting disruption-era analysis and executive storytelling.

---

## Talend Components Used

| Component            | Purpose                           |
| -------------------- | --------------------------------- |
| tFixedFlowInput      | Era definition ingestion          |
| tMap                 | Era transformation and enrichment |
| tFileOutputDelimited | Curated dimension export          |

---

## Key Transformations

- Standardized disruption-era classifications

- Assigned historical period ranges

- Added market condition classifications

- Generated surrogate keys

---

## Era Categories

| Era                     | Period     |
| ----------------------- | ---------- |
| Normal                  | 2000--2007 |
| GFC Shock               | 2008--2009 |
| Stagnation              | 2010--2019 |
| COVID Spike             | 2020--2022 |
| Normalization + Red Sea | 2023--2024 |

---

## Engineering Notes

The dimension was introduced to replace direct event-level joins that caused overlapping temporal relationships and many-to-many relationship inflation.

The design improves:

- historical segmentation,

- relationship stability,

- and Power BI filter propagation.

---

## Output

- dim_disruption_era.csv

---

# 7\. Fact Table ETL Pipelines

# 7.1 Fact_Freight_Market Pipeline

## Objective

Load monthly freight market indicators and global shipping KPIs into the warehouse.

---

## Fact Grain

One row per reporting month.

---

## Talend Components Used

| Component                 | Purpose                 |
| ------------------------- | ----------------------- |
| tFileInputDelimited       | Source ingestion        |
| tMap                      | KPI transformation      |
| Dim_Date lookup           | Temporal integration    |
| Dim_Disruption_Era lookup | Historical segmentation |
| tFileOutputDelimited      | Curated fact export     |

---

## Measures

- container_rate_usd_40ft

- baltic_dry_index

- supply_chain_pressure_index

- on_time_delivery_pct

- rolling_6m_container

---

## Output

- fact_freight_market.csv

---

# 7.2 Fact_Port_Logistics Pipeline

## Objective

Load weekly operational logistics metrics into the warehouse.

---

## Fact Grain

One row per port per reporting week.

---

## Talend Components Used

| Component                 | Purpose                 |
| ------------------------- | ----------------------- |
| tFileInputDelimited       | Source ingestion        |
| tMap                      | KPI transformation      |
| Dim_Date lookup           | Temporal integration    |
| Dim_Geography lookup      | Geography integration   |
| Dim_Disruption_Era lookup | Historical segmentation |
| tFileOutputDelimited      | Curated fact export     |

---

## Measures

- avg_wait_days

- vessels_at_anchor

- utilization_pct

- congestion_level

---

## Output

- fact_port_logistics.csv

---

# 7.3 Fact_Trade_Volume Hybrid Pipeline

## Objective

Load annual bilateral trade metrics and supply chain trade indicators into the warehouse.

---

## Fact Grain

One row per exporter-importer-category-year combination.

---

## Architecture Decision

During implementation, complex bilateral geography mapping and role-playing dimension joins were identified as more suitable for preprocessing in Python prior to Talend orchestration.

A hybrid ETL architecture was adopted to:

- simplify Talend transformations,

- improve maintainability,

- reduce lookup complexity,

- and preserve warehouse grain consistency.

---

## Hybrid Workflow

### Python Preprocessing Layer

Responsibilities:

- exporter/importer extraction,

- bilateral geography mapping,

- surrogate key preparation,

- and curated trade dataset generation.

### Technologies Used

- pandas

- merge() joins

- preprocessing notebooks

---

### Talend Integration Layer

Responsibilities:

- dimensional lookups,

- fact loading,

- surrogate key integration,

- and warehouse export generation.

### Talend Components Used

| Component            | Purpose                   |
| -------------------- | ------------------------- |
| tFileInputDelimited  | Curated dataset ingestion |
| tMap                 | Lookup integration        |
| tFileOutputDelimited | Curated fact export       |

---

## Measures

- trade_value_bn_usd

- yoy_growth_pct

- effective_tariff_rate_pct

- concentration_risk

- supply_chain_integrated

---

## Outputs

- dim_trade_geography.csv

- curated_trade_fact_ready.csv

- fact_trade_volume.csv

---

# 7.4 Fact_Industry_Exposure Pipeline

## Objective

Load strategic industry vulnerability metrics and resilience indicators into the warehouse.

---

## Fact Grain

One row per industry per reporting year.

---

## Talend Components Used

| Component                 | Purpose                 |
| ------------------------- | ----------------------- |
| tFileInputDelimited       | Source ingestion        |
| tMap                      | KPI transformation      |
| Dim_Date lookup           | Temporal integration    |
| Dim_Industry lookup       | Industry integration    |
| Dim_Disruption_Era lookup | Historical segmentation |
| tFileOutputDelimited      | Curated fact export     |

---

## Measures

- overall_vulnerability

- geopolitical_exposure

- resilience_score

---

## Output

- fact_industry_exposure.csv

---

# 8\. Data Quality Validation

Several validation and integrity rules were enforced during ETL execution.

## Validation Rules

| Validation Type                  | Description                   |
| -------------------------------- | ----------------------------- |
| Null Validation                  | Missing value detection       |
| Duplicate Validation             | Duplicate record prevention   |
| Referential Integrity Validation | Dimension lookup verification |
| Range Validation                 | KPI boundary checks           |
| Classification Validation        | Business-rule consistency     |
| Datatype Validation              | Schema integrity verification |

---

## Error Handling Strategy

Rejected or unmatched records were redirected into validation outputs for troubleshooting and ETL auditing.

This approach improves:

- traceability,

- pipeline reliability,

- and warehouse data quality.

---

# 9\. Curated Output Layer

The ETL pipelines generate curated analytical datasets stored within the warehouse curated layer.

## Curated Outputs

| Output Dataset             | Purpose                           |
| -------------------------- | --------------------------------- |
| dim_date.csv               | Temporal dimension                |
| dim_geography.csv          | Geography dimension               |
| dim_industry.csv           | Industry dimension                |
| dim_disruption_era.csv     | Historical segmentation dimension |
| fact_freight_market.csv    | Freight analytics                 |
| fact_port_logistics.csv    | Operational logistics analytics   |
| fact_trade_volume.csv      | Trade analytics                   |
| fact_industry_exposure.csv | Industry risk analytics           |

---

# 10\. ETL Performance Considerations

Although the datasets are moderate in size, several engineering optimizations were applied.

## Optimization Strategies

- modular pipeline design,

- reusable transformation logic,

- centralized classification handling,

- staged processing architecture,

- lightweight dimensions,

- and simplified lookup strategies.

These improvements support:

- future scalability,

- maintainability,

- and efficient Power BI integration.

---

# 11\. Warehouse Design Principles

The warehouse implementation follows Kimball dimensional modeling principles.

## Core Principles Applied

- star schema architecture,

- conformed dimensions,

- surrogate keys,

- strict fact table grain enforcement,

- centralized business-rule engineering,

- and semantic consistency.

The architecture intentionally avoids:

- unnecessary snowflake complexity,

- overlapping event joins,

- ambiguous relationship paths,

- and excessive Power BI transformation logic.

---

# 12\. ETL Pipeline Benefits

The Talend ETL implementation provides:

- structured analytical integration,

- centralized business logic,

- reusable dimensional semantics,

- scalable warehouse architecture,

- and enterprise-style analytical processing.

The pipeline design improves:

- reporting consistency,

- dashboard usability,

- semantic maintainability,

- and long-term project scalability.

---

# 13\. Deliverables Produced

The following deliverables were generated during this phase:

- Talend ETL jobs

- Curated dimensional datasets

- Analytical fact tables

- Validation workflows

- Business classification pipelines

- Historical segmentation structures

- Warehouse-ready exports

---

# 14\. Documentation & Portfolio Evidence

Important ETL implementation screenshots should be stored within:

`docs/screenshots/`

## Recommended Screenshots

- Talend job workflows

- tMap transformation logic

- Lookup mappings

- ETL orchestration flows

- Schema relationships

- Curated output validation

- Power BI semantic model

- Dashboard integration

These screenshots provide:

- implementation evidence,

- portfolio documentation,

- and project presentation support.

---

# 15\. Conclusion

The Talend ETL phase successfully transformed fragmented operational supply chain datasets into curated analytical warehouse structures optimized for dimensional modeling and Power BI reporting.

The ETL architecture demonstrates:

- enterprise-style transformation workflows,

- centralized business-rule engineering,

- reusable analytical enrichment,

- scalable dimensional integration,

- and modern analytics engineering practices.

The resulting curated layer provides the foundation for:

- semantic modeling,

- executive dashboard development,

- KPI storytelling,

- and scalable analytical exploration.
