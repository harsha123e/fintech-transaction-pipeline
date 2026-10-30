# Fintech Transaction Data Pipeline

An end-to-end batch data engineering project built with **PySpark and Delta Lake** to simulate how fintech transaction data can be ingested, cleaned, validated, deduplicated, and transformed into analytical datasets.

The project is implemented in two environments: **Google Colab and Databricks**.

## Technology Stack

- **Languages:** Python, SQL
- **Data Processing:** PySpark, Apache Spark
- **Storage:** Delta Lake
- **Libraries:** Pandas, NumPy
- **Platforms:** Google Colab, Databricks

## Pipeline Architecture

The pipeline follows the Medallion Architecture, separating raw data, cleaned records, and aggregated outputs.

```text
Synthetic Transaction Data
          |
          ▼
    Bronze Layer
          |
          ▼
 Data Validation
          |
     ┌────┴────┐
     ▼         ▼
   Silver   Quarantine
     |
     ▼
 Deduplication
     |
     ▼
   Gold Layer
     |
     ▼
Daily Transaction Summary
```

## What the Pipeline Does

1. **Data Generation:** Creates 100,000 synthetic fintech transactions and introduces data quality issues.
2. **Bronze Layer:** Ingests and stores the incoming transaction records in Delta format.
3. **Data Validation:** Identifies missing values, negative amounts, and invalid transaction statuses.
4. **Quarantine:** Separates invalid records for inspection instead of allowing them into the cleaned dataset.
5. **Deduplication:** Identifies duplicate transaction IDs and retains one record per transaction.
6. **Silver Layer:** Stores validated and deduplicated transaction records.
7. **Gold Layer:** Generates daily transaction summaries grouped by date, currency, and status.
8. **Reconciliation:** Compares aggregate amounts between Silver and Gold to verify data consistency.

## Pipeline Results

| Metric | Result |
|---|---:|
| Original transactions | 100,000 |
| Incoming records | 107,000 |
| Records quarantined | 5,000 |
| Duplicate rows removed | 2,000 |
| Final Silver records | 100,000 |
| Gold summary records | 360 |

## Validation Results

- No duplicate transaction IDs in the Silver layer.
- No invalid records in the final Silver dataset.
- No duplicate keys in the Gold summary.
- Silver and Gold aggregate amounts matched, with a difference of **0.00**.
- Pipeline validation completed successfully.

*Note: Reconciliation verifies aggregate amount preservation; it does not perform currency conversion.*

## Project Notebooks

Explore the implementation in either environment:

- [Google Colab Notebook](./fintech_pipeline_colab.ipynb)
- [Databricks Notebook](./fintech_pipeline_databricks.ipynb)
