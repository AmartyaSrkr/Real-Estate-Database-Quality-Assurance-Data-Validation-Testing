# Kolkata Real Estate Data Analysis: Data Cleaning, Pricing Trends & Market Benchmarking (SQL)

End-to-end SQL analysis of 3,000+ Kolkata real estate listings. The project cleans raw, unstructured listing data, standardizes prices and areas, and uncovers pricing patterns, neighborhood benchmarks, and outlier properties using PostgreSQL.

## Project Overview

**Objective:** Turn a messy real estate dataset into clean, analysis-ready data and answer key market questions:

- How are listings distributed across property types and neighborhoods?
- Which neighborhoods are the most expensive on a price-per-sq.-ft. basis?
- Which properties are overpriced or underpriced compared to their local market?
- How much premium do commercial properties command over residential ones?

**Tools:** PostgreSQL 18, pgAdmin 4, SQL

## Analysis Workflow

### 1. Data Cleaning & Transformation

Raw listing data came as unstructured text and was converted into analysis-ready numeric fields:

- **Price standardization:** Converted text denominations ("Lac", "Cr") into exact numeric INR values
- **Area extraction:** Stripped units ("sqft") and cast strings to numeric values
- **Feature extraction:** Parsed `property_type` and `location` from listing title strings using regex, with 100% extraction accuracy
- **Data quality checks:** Detected missing or malformed values in key fields and confirmed all columns use correct data types (NUMERIC, VARCHAR, etc.)

**Result:** 100% numeric consistency across price and area fields.

### 2. Exploratory Data Analysis (EDA)

- **Market composition:** 2 BHK and 3 BHK apartments make up over 60% of total inventory
- **Price per sq. ft.:** Commercial properties carry a premium, averaging about ₹9,541 per sq. ft.
- **Neighborhood ranking:** Used `PERCENT_RANK()` to identify top-tier neighborhoods by price
- **Price quartiles:** Segmented the market into quartiles to understand the price distribution

### 3. Outlier & Anomaly Detection

- **Overvalued properties:** Flagged listings priced 30%+ above their neighborhood average
- **Undervalued properties:** Identified spacious properties priced below their neighborhood average, a possible opportunity for buyers
- **Micro-market inconsistencies:** Found pricing differences across nearby neighborhoods

Outliers were found in roughly 15 to 20% of listings.

## Key Findings

| Metric | Result |
|---|---|
| Listings analyzed | 3,000+ |
| Numeric consistency after cleaning | 100% |
| Listings flagged as outliers | ~15-20% |
| Records with critical data errors | 45 |
| Records with data quality issues | 120 |
| Overall data quality rate | 98.7% |

**Data issues found and resolved:**
- Inconsistent price formatting, fixed through standardization
- Missing area values, flagged for correction
- Outlier listings, documented for further review

## SQL Techniques Used

| Analysis Step | SQL Techniques |
|---|---|
| Data cleaning | `REPLACE()`, `CAST()`, `SUBSTRING()`, `CASE WHEN` |
| Aggregation & summaries | `GROUP BY`, `HAVING`, `COUNT(*) FILTER` |
| Multi-step analysis | Chained CTEs, Subqueries, JOINs |
| Ranking & benchmarking | `RANK()`, `NTILE()`, `PERCENT_RANK() OVER()` |
| Data loading & setup | `CREATE TABLE`, `ALTER TABLE`, `COPY`, `UPDATE` |

## Output Files

| File | Description |
|---|---|
| `kolkata_real_estate_analysis.sql` | Master SQL script with all cleaning and analysis queries |
| `Market_Quartiles.csv` | Price quartile segmentation of the market |
| `Undervalued_premium_properties.csv` | Properties priced below their neighborhood average |
| `PropertyRanking_by_Price_per_Location.csv` | Neighborhood ranking by price per sq. ft. |
| `PropertyType_PricingTrends.csv` | Pricing trends and commercial property premium |
| `Top_10_most_expensive_locations.csv` | Top 10 most expensive neighborhoods |
| `Oversized_properties.csv` | Properties with unusually large areas (outliers) |

## Sample Queries

**Cleaning: check for failed price conversions**
```sql
SELECT COUNT(*) AS failed_conversions
FROM properties
WHERE price_numeric IS NULL AND price_text IS NOT NULL;
```

**Cleaning: find unrealistic area values**
```sql
SELECT *
FROM properties
WHERE area_numeric < 100 OR area_numeric > 50000;
```

**Analysis: share of 2 BHK and 3 BHK apartments in
