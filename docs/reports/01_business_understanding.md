# Business Understanding: Global Supply Chain & Trade Disruption Analytics V2

## 1. Project Title
**Global Supply Chain & Trade Disruption Analytics Platform**

## 2. Project Overview
This project provides a comprehensive analytical deep-dive into 25 years (2000–2024) of global trade and logistics data. It analyzes the volatility of freight rates, port congestion, and bilateral trade flows across various disruption eras—ranging from the stability of the early 2000s to the extreme shocks of COVID-19 and the Red Sea crisis.

**Why it matters:** Global supply chains have transitioned from a predictable "Just-in-Time" model to a highly volatile landscape. Businesses care because supply chain instability directly impacts their bottom line through increased shipping costs (which saw a 594% spike in 2021), delayed inventory, and lost customer trust.

## 3. Business Problem
The platform addresses critical questions regarding the fragility of modern commerce:
- **Disruption Impact:** How do geopolitical, pandemic, and natural events quantify into dollar-value freight shocks?
- **Vulnerability Mapping:** Which specific industries and regions are most exposed to trade wars and maritime bottlenecks?
- **Operational Delays:** What is the correlation between port congestion and the actual on-time delivery rate of goods?
- **Price Elasticity:** How did historical shocks like the 2008 Financial Crisis and COVID-19 redefine baseline costs?

## 4. Project Objectives
- **Data Integration:** Build robust ETL pipelines to merge shipping indices, port records, and trade flow data into a unified source of truth.
- **Analytical Data Mart:** Create a structured environment optimized for time-series analysis across five distinct "Disruption Eras."
- **Dimensional Modeling:** Design a Kimball-style dimensional data mart for analytical reporting and dashboard optimization.
- **Stress Modeling:** Analyze the relationship between the Supply Chain Pressure Index and market recovery timelines.
- **Executive Visualization:** Develop high-level dashboards for historical and trend-based monitoring.

## 5. Key Performance Indicators (KPIs)
| KPI | Business Significance |
| --- | ---------------------- |
| **Supply Chain Pressure Index** | Composite metric (0.5–5.0) indicating total market stress levels |
| **Container Rate Growth %** | Year-over-year change in spot rates (e.g., Shanghai to LA) |
| **Port Congestion Rate** | Weekly tracking of vessel waiting times at major global hubs |
| **Recovery Duration (MTTR)** | Average months taken for a sector to stabilize after a disruption |
| **On-Time Delivery (OTD) %** | Percentage of ocean shipments arriving within the promised window |

## 6. Tools & Technologies
- Language: Python (Data Science and statistical modeling)
- Data Processing: pandas and NumPy
- ETL Orchestration: Talend
- Business Intelligence: Power BI

## 7. Expected Outcomes
- **Interactive Dashboards:** A visual control center for monitoring historical trends in port health and freight volatility.
- **Trade Disruption Insights:** Data-driven insights into which event types cause the longest recovery times.
- **Integrated Analytics Dataset:** A "clean" master dataset following dimensional modeling principles.
- **ETL Workflow:** A repeatable process for updating and managing the supply chain data mart.

## 8. Project Scope & Limitations
- **Data Nature:** The dataset is historical and partially synthetic, calibrated to real-world sources.
- **Project Focus:** The focus is on analytics engineering and BI workflows rather than large-scale distributed processing.
- **Exclusions:** Real-time streaming and predictive ML modeling are outside the scope of this initial phase.

## 9. Stakeholders

This analytical platform is designed to support multiple stakeholders involved in logistics and international trade operations:

- Supply Chain Managers → monitor congestion and delivery performance
- Trade Analysts → evaluate tariff and disruption impacts
- Logistics Executives → identify operational bottlenecks
- Business Intelligence Teams → analyze trends and historical performance
- Strategy & Risk Departments → assess geopolitical and disruption exposure

## 10. Core Analytical Questions

The project aims to answer several high-value analytical questions:

- Which disruption events generated the highest freight rate shocks?
- Which ports experienced the longest congestion periods?
- How does congestion affect delivery reliability?
- Which industries demonstrate the highest supply chain vulnerability?
- Which trade routes recovered fastest after major disruptions?
- How did COVID-19 reshape global shipping economics?
- Which years showed the highest supply chain stress levels?

## 11. Data Architecture Vision

The project follows a layered analytical architecture:

1. Raw Layer
   - Original CSV files stored without modification

2. Cleaned Layer
   - Standardized schemas
   - Data quality handling
   - Consistent naming conventions

3. Curated Layer
   - Integrated analytical datasets
   - Fact and dimension modeling
   - Aggregated business metrics

4. Presentation Layer
   - Power BI dashboards
   - KPI reporting
   - Executive analytics


## 12. Success Criteria

The project will be considered successful if:

- ETL pipelines successfully integrate all source datasets
- Data quality issues are identified and resolved
- Dimensional models support efficient analytical querying
- Power BI dashboards provide actionable business insights
- The platform enables clear storytelling around global disruptions
- Documentation fully explains the project lifecycle and architecture


## 13. Risks & Assumptions

### Assumptions
- Source CSV files maintain consistent structure
- Historical data accurately reflects disruption periods
- Synthetic calibration remains representative of real-world trends

### Risks
- Temporal granularity differences between datasets
- Inconsistent country or port naming conventions
- Potential correlation misinterpretation without domain context


## 14. Project Methodology

The project follows a phased analytics engineering workflow:

1. Business Understanding
2. Data Understanding
3. Exploratory Data Analysis (EDA)
4. Data Cleaning & Standardization
5. Dimensional Modeling
6. ETL Pipeline Development
7. Dashboard Development
8. Reporting & Documentation