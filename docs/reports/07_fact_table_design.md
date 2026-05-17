# 07 --- Fact Table Design

## 1\. Phase Objective

The purpose of this phase is to design the analytical fact tables that power the Global Supply Chain & Trade Disruption Analytics Platform.

This phase transforms cleaned and standardized datasets into structured Kimball-style analytical fact tables optimized for:

- Power BI dashboarding

- Historical trend analysis

- Operational KPI reporting

- Cross-domain supply chain intelligence

The warehouse architecture follows Kimball dimensional modeling principles using conformed dimensions and business-aligned analytical grains.

---

# 2\. Fact Modeling Strategy

The analytical warehouse follows a star schema architecture where:

- Dimensions provide business context

- Fact tables store measurable operational activity

- Conformed dimensions ensure semantic consistency

- Fact tables remain optimized for Power BI analytics

The design emphasizes:

- analytical simplicity,

- scalable joins,

- deterministic grain enforcement,

- and high-performance dashboard reporting.

---

# 3\. Conformed Dimensions Used

| Dimension          | Purpose                                    |
| ------------------ | ------------------------------------------ |
| dim_date           | Temporal analytics and trend reporting     |
| dim_geography      | Port and regional logistics analysis       |
| dim_industry       | Industry vulnerability and trade analytics |
| dim_disruption_era | Historical disruption segmentation         |
| dim_trade_category | Trade classification and grouping          |

---

# 4\. Fact Table Architecture Overview

| Fact Table             | Business Process                 | Grain                                       |
| ---------------------- | -------------------------------- | ------------------------------------------- |
| fact_freight_market    | Global freight market monitoring | One row per calendar month                  |
| fact_port_logistics    | Port congestion operations       | One row per port per reporting week         |
| fact_trade_volume      | Bilateral trade activity         | One row per exporter-importer-category-year |
| fact_industry_exposure | Industry vulnerability analysis  | One row per industry per year               |

---

# 5\. Fact_Freight_Market

## Business Process

Captures global freight market behavior and macro-level supply chain stress indicators across the 2000--2024 historical period.

Supports:

- freight trend analysis,

- shipping inflation monitoring,

- disruption-era comparison,

- and market volatility analysis.

---

## Source System

- cleaned_shipping_rates.csv

---

## Grain

One row per calendar month representing aggregated global freight market conditions.

---

## Primary Key

freight_fact_sk

---

## Foreign Keys

| Foreign Key | References         |
| ----------- | ------------------ |
| date_key    | dim_date           |
| era_sk      | dim_disruption_era |

---

## Measures

| Measure                        | Description                      | Additivity    |
| ------------------------------ | -------------------------------- | ------------- |
| baltic_dry_index               | Global dry bulk demand index     | Additive      |
| container_rate_usd_40ft        | Spot container freight rate      | Additive      |
| air_cargo_rate_usd_kg          | Air freight market rate          | Additive      |
| tanker_rate_aframax_usd_day    | Tanker market rate               | Additive      |
| bulk_carrier_handysize_usd_day | Bulk carrier market rate         | Additive      |
| supply_chain_pressure_index    | Global operational stress metric | Semi-additive |
| on_time_delivery_pct           | Delivery reliability KPI         | Semi-additive |
| rolling_6m_container           | Smoothed freight trend metric    | Semi-additive |
| bdi_mom_change_pct             | Monthly BDI percentage change    | Non-additive  |
| container_yoy_pct              | Annual container inflation rate  | Non-additive  |

---

## Engineering Notes

The fact table acts as the central freight-market operational layer within the analytical mart.

Monthly grain was selected because:

- the source dataset is monthly,

- freight trends evolve over time,

- and executive reporting focuses on long-term operational behavior.

The table integrates directly with:

- dim_date

- dim_disruption_era

Direct event-level joins were intentionally avoided to prevent many-to-many relationship inflation caused by overlapping disruption events.

---

## Analytical Use Cases

The table enables:

- freight inflation analysis,

- COVID-era shipping comparison,

- container volatility tracking,

- supply chain stress monitoring,

- and long-term freight market storytelling.

---

# 6\. Fact_Port_Logistics

## Business Process

Captures operational congestion behavior across major global maritime ports between 2019 and 2024.

Supports:

- congestion monitoring,

- operational bottleneck detection,

- and logistics performance analysis.

---

## Source System

- cleaned_port_congestion.csv

---

## Grain

One row per port per reporting week.

---

## Primary Key

port_fact_sk

---

## Foreign Keys

| Foreign Key | References         |
| ----------- | ------------------ |
| date_key    | dim_date           |
| geo_sk      | dim_geography      |
| era_sk      | dim_disruption_era |

---

## Measures

| Measure           | Description                     | Additivity    |
| ----------------- | ------------------------------- | ------------- |
| avg_wait_days     | Average vessel waiting duration | Semi-additive |
| vessels_at_anchor | Number of anchored vessels      | Additive      |
| utilization_pct   | Port utilization rate           | Semi-additive |
| congestion_level  | Operational congestion severity | Semi-additive |

---

## Engineering Notes

This fact table represents the operational logistics layer of the warehouse.

Weekly grain was selected because:

- congestion behavior changes rapidly,

- weekly monitoring provides operational visibility,

- and the source dataset is reported weekly.

The table integrates geography and disruption-era dimensions to support:

- regional congestion analysis,

- crisis-period monitoring,

- and port performance benchmarking.

---

## Analytical Use Cases

The table enables:

- port congestion heatmaps,

- vessel backlog monitoring,

- regional bottleneck comparison,

- and disruption-driven operational analysis.

---

# 7\. Fact_Trade_Volume

## Business Process

Captures annual bilateral trade relationships between major trading economies.

Supports:

- trade flow monitoring,

- geopolitical analysis,

- tariff impact assessment,

- concentration risk analysis,

- and international trade trend reporting.

---

## Source System

- cleaned_trade_flows.csv

---

## Grain

One row per exporter-importer trade relationship per calendar year and trade category.

---

## Primary Key

trade_fact_sk

---

## Foreign Keys

| Foreign Key       | References         |
| ----------------- | ------------------ |
| date_key          | dim_date           |
| exporter_geo_sk   | dim_geography      |
| importer_geo_sk   | dim_geography      |
| industry_sk       | dim_industry       |
| trade_category_sk | dim_trade_category |
| era_sk            | dim_disruption_era |

---

## Measures

| Measure                   | Description                            | Additivity    |
| ------------------------- | -------------------------------------- | ------------- |
| trade_value_bn_usd        | Bilateral trade value in billions USD  | Additive      |
| yoy_growth_pct            | Year-over-year trade growth percentage | Non-additive  |
| effective_tariff_rate_pct | Applied tariff percentage              | Non-additive  |
| concentration_risk        | Supply dependency concentration score  | Semi-additive |

---

## Degenerate Attributes

| Attribute               | Purpose                             |
| ----------------------- | ----------------------------------- |
| exporter                | Exporting country                   |
| importer                | Importing country                   |
| key_goods               | Primary traded goods                |
| trade_active            | Active trade relationship indicator |
| supply_chain_integrated | Integrated supply chain flag        |

---

## Engineering Notes

This table captures strategic bilateral trade relationships rather than transactional shipping activity.

Annual grain was selected because:

- trade statistics are aggregated annually,

- geopolitical trends evolve over longer periods,

- and executive reporting prioritizes long-term trade shifts.

The implementation follows a hybrid ETL architecture:

### Python Preprocessing Layer

- exporter/importer extraction,

- bilateral geography mapping,

- surrogate key preparation,

- and curated trade dataset generation.

### Talend Integration Layer

- dimensional lookups,

- surrogate key integration,

- fact loading,

- and warehouse export generation.

The table supports:

- trade war analysis,

- tariff impact assessment,

- concentration risk monitoring,

- and supply chain dependency analytics.

Direct event joins were intentionally avoided because disruption events overlap across historical periods.

---

## Data Integrity Rules

- One row must exist per exporter-importer-category-year combination

- Trade values must remain non-negative

- Foreign keys must maintain referential integrity

- Annual grain consistency must be preserved

---

## Analytical Use Cases

The table enables:

- China--US trade trend analysis,

- tariff impact comparison,

- trade resilience monitoring,

- strategic supply chain dependency analysis,

- and disruption-era trade reporting.

---

# 8\. Fact_Industry_Exposure

## Business Process

Captures industry-level vulnerability and operational exposure across multiple disruption scenarios.

Supports:

- resilience benchmarking,

- strategic risk assessment,

- and industry exposure analysis.

---

## Source System

- cleaned_industry_exposure.csv

---

## Grain

One row per industry per calendar year.

---

## Primary Key

industry_fact_sk

---

## Foreign Keys

| Foreign Key | References         |
| ----------- | ------------------ |
| date_key    | dim_date           |
| industry_sk | dim_industry       |
| era_sk      | dim_disruption_era |

---

## Measures

| Measure               | Description                   | Additivity    |
| --------------------- | ----------------------------- | ------------- |
| overall_vulnerability | Aggregate vulnerability score | Semi-additive |
| geopolitical_exposure | Geopolitical exposure score   | Semi-additive |
| resilience_score      | Industry resilience score     | Semi-additive |

---

## Degenerate Attributes

| Attribute  | Purpose                       |
| ---------- | ----------------------------- |
| risk_level | Executive risk classification |

---

## Engineering Notes

This fact table acts as the strategic resilience layer of the analytical platform.

The yearly grain aligns with:

- strategic reporting cycles,

- long-term industry evolution,

- and historical resilience tracking.

The table transforms operational exposure metrics into measurable analytical indicators optimized for:

- executive risk dashboards,

- resilience comparison,

- vulnerability benchmarking,

- and strategic supply chain planning.

Several business classifications were intentionally centralized within dim_industry rather than duplicated inside the fact table.

This improves:

- semantic consistency,

- ETL maintainability,

- and Power BI simplicity.

---

## Analytical Use Cases

The table enables:

- vulnerability benchmarking,

- resilience ranking,

- semiconductor exposure analysis,

- and strategic supply chain planning.

---

# 9\. Additivity Classification Strategy

The warehouse applies additive classification rules to ensure accurate KPI aggregation.

| Additivity Type | Description                              |
| --------------- | ---------------------------------------- |
| Additive        | Fully summable across all dimensions     |
| Semi-additive   | Summable across selected dimensions only |
| Non-additive    | Cannot be directly aggregated            |

---

# 10\. Grain Integrity Principles

Each fact table follows strict grain enforcement rules.

This ensures:

- deterministic joins,

- aggregation stability,

- analytical consistency,

- and scalable Power BI reporting.

The project intentionally avoids:

- mixed grains,

- overlapping event joins,

- ambiguous many-to-many relationships,

- and unnecessary snowflake complexity.

---

# 11\. Relationship Design Strategy

The warehouse follows a clean star schema architecture.

All fact tables connect directly to conformed dimensions using one-to-many relationships.

The design intentionally avoids:

- bridge tables,

- ambiguous relationship paths,

- and excessive normalization.

---

# 12\. Power BI Optimization Considerations

The fact table architecture was optimized for Power BI semantic modeling.

Benefits include:

- simplified DAX calculations,

- stable filter propagation,

- efficient slicer behavior,

- and improved dashboard performance.

The use of:

- dim_disruption_era

instead of direct disruption event joins significantly improves relationship stability and analytical consistency.

---

# 13\. Final Architecture Summary

The analytical mart contains:

- operational fact tables,

- strategic fact tables,

- conformed dimensions,

- and historically segmented analytics.

The architecture supports:

- enterprise-style reporting,

- disruption storytelling,

- KPI-driven dashboards,

- and scalable analytical exploration.

---

# 14\. Conclusion

The fact table design phase successfully transformed cleaned supply chain datasets into a structured analytical warehouse architecture following Kimball dimensional modeling principles.

The resulting design provides:

- scalable analytical structures,

- business-aligned granularity,

- optimized Power BI integration,

- centralized business semantics,

- and enterprise-grade reporting readiness.

The warehouse is now fully prepared for:

- Talend ETL implementation,

- star schema loading,

- semantic modeling,

- and executive dashboard development.
