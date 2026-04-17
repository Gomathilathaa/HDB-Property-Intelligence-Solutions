# HDB Property Intelligence Dashboard

A modern HDB data warehouse built with a Medallion Architecture (Bronze → Silver → Gold) for analyzing Singapore resale, rental, investment and property supply data.

## Overview

This project enables analytics for:

- Property prices
- Rental trends
- Supply vs demand
- Investment insights by town

## Architecture

### Bronze Layer
Raw ingested data from source systems.

Tables:

- `bronze_resale`
- `bronze_property`
- `bronze_rental`

### Silver Layer (Data Cleaning & Standardization)
Cleaned, typed, and standardized datasets with derived metrics.

Key transformations:

- Lowercasing and trimming text fields
- Standardizing flat types
- Casting numeric fields
- Deriving metrics such as `price_per_sqm` and `property_age`

Tables:

- `resale_clean`
- `rental_clean`
- `property_clean`
- `property_flat_supply`
- `property_usage`

### Gold Layer (Analytics Model)
Optimized star schema for BI and analytics.

Dimensions:

- `dim_property`
- `dim_date`
- `dim_flat_type`

Fact tables:

- `fact_resale` → resale transactions
- `fact_rental` → rental transactions
- `fact_supply` → supply distribution

## Data Model

Star schema design with facts linked to dimensions via surrogate keys, optimized for Power BI and analytics workflows.

### Grain Definition

| Table | Grain |
|---|---|
| `fact_resale` | One resale transaction |
| `fact_rental` | One rental transaction |
| `fact_supply` | One property + flat type supply record |

## Key Transformations

1. Flat Type Standardization
   - Converted inconsistent values into standardized categories such as `1 ROOM`, `2 ROOM`, ..., `EXECUTIVE`, `MULTI GENERATION`
2. Data Normalization
   - Used `stack()` to convert wide columns into long format for:
     - Room distribution → `property_flat_supply`
     - Usage flags → `property_usage`
3. Derived Metrics
   - `price_per_sqm`
   - `property_age`
   - Investment scoring metrics

## Advanced Analytics

Investment metrics include:

- Average resale price
- Rental averages
- Demand (transaction count)
- Supply (units sold)
- Investment score with normalized components:
  - `price_score`
  - `demand_score`
  - `rent_score`
  - `supply_score`

## Tools & Technologies

- SQL (Databricks / Spark SQL)
- Delta Lake
- Medallion Architecture
- Data Modeling (Star Schema)
- Analytics-ready dataset design

## Key Learnings

- Define grain before modeling
- Handle duplicates in transactional data
- Design scalable dimension tables
- Build analytics-ready datasets for BI consumption



