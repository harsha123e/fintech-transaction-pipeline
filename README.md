# Fintech Transaction Pipeline

An end-to-end fintech transaction data engineering project built using **Python, PySpark, and Delta Lake**, implementing the Medallion Architecture (Bronze, Silver, and Gold).

## Project Overview

This project simulates a fintech transaction data pipeline that ingests transaction records, introduces realistic data-quality issues, validates incoming data, quarantines invalid records, removes duplicates, and generates analytical summaries.

The pipeline processes synthetic transaction data through three layers:

- **Bronze:** Stores incoming transaction records in Delta Lake.
- **Silver:** Validates records, quarantines invalid data, and removes duplicate transactions.
- **Gold:** Produces daily transaction summaries grouped by currency and transaction status.

## Architecture

```text
Synthetic Transaction Data
          |
          v
    Raw CSV Source
          |
          v
    Bronze Layer
    (Delta Lake)
          |
          v
   Data Validation
       /     \
      v       v
   Valid    Quarantine
      |
      v
  Deduplication
      |
      v
   Silver Layer
   (Delta Lake)
      |
      v
  Gold Aggregation
      |
      v
    Gold Layer
   (Delta Lake)
      |
      v
Daily Transaction
     Summary
```

## Technology Stack

| Technology | Purpose |
|---|---|
| Python | Data generation and pipeline logic |
| PySpark 4.0.4 | Distributed data processing |
| Delta Lake 4.4.1 | Transactional data storage |
| Pandas & NumPy | Synthetic data generation |
| Google Colab | Cloud-based development environment |
| Jupyter Notebook | Pipeline implementation |

## Pipeline Workflow

### 1. Data Generation

- Generated 100,000 synthetic fintech transactions.
- Included transaction IDs, customer IDs, amounts, currencies, statuses, and timestamps.
- Used four currencies: USD, INR, EUR, and GBP.

### 2. Data Quality Issue Injection

Introduced controlled data-quality issues to simulate an imperfect incoming dataset:

- 2,000 duplicate rows.
- 1,000 missing customer IDs.
- 1,000 missing amounts.
- 1,000 invalid transaction statuses.
- 1,000 negative transaction amounts.
- 1,000 missing currencies.

The resulting dataset contained 107,000 records.

### 3. Bronze Layer

- Ingested incoming records using PySpark.
- Persisted raw records in Delta Lake format.
- Profiled the data to identify missing values, invalid records, and duplicate transaction IDs.

### 4. Validation and Quarantine

Applied validation rules to identify missing required fields, invalid amounts, unsupported currencies, and invalid transaction statuses.

- Valid records were passed to the Silver processing stage.
- Invalid records were stored separately in a Delta Lake quarantine table.
- Rejection reasons were captured for each quarantined record.

### 5. Silver Layer

- Removed duplicate transactions using PySpark window functions.
- Applied deterministic ordering using transaction timestamp and additional fields.
- Validated the resulting dataset for duplicate IDs, missing customer IDs, invalid amounts, missing currencies, and invalid statuses.
- Persisted the cleaned records in Delta Lake.

### 6. Gold Layer

Generated daily analytical summaries grouped by:

- Transaction date
- Currency
- Transaction status

Calculated:
- Total transaction count
- Total transaction amount
- Average transaction amount

Persisted the aggregated results in Delta Lake.

### 7. Pipeline Validation and Reporting

- Validated record distribution between valid and quarantined datasets.
- Checked Silver and Gold data quality.
- Compared Silver and Gold aggregate amounts.
- Generated a JSON pipeline validation report.

## Execution Results

| Stage | Records |
|---|---:|
| Original synthetic dataset | 100,000 |
| Incoming dataset with injected issues | 107,000 |
| Bronze | 107,000 |
| Quarantined | 5,000 |
| Valid records before deduplication | 102,000 |
| Silver after deduplication | 100,000 |
| Gold aggregated records | 360 |

**Validation results:**
- Silver duplicate transaction IDs: 0
- Silver invalid amounts: 0
- Silver missing customer IDs: 0
- Silver missing currencies: 0
- Silver invalid statuses: 0
- Gold duplicate aggregation keys: 0
- Pipeline validation status: PASSED

## How to Run

1. Open the notebook using the link below.
2. Select **Runtime → Run all**.
3. Allow the notebook to install the required dependencies.
4. Execute the pipeline in Google Colab.

[Open Notebook in Google Colab](https://colab.research.google.com/github/harsha123e/fintech-transaction-pipeline/blob/main/Fintech_Transaction_Pipeline.ipynb)

## Project Scope and Limitations

- Transaction data is synthetically generated and does not represent real customer or payment information.
- The pipeline runs in Google Colab using its available compute environment.
- Delta Lake tables are stored in the notebook's execution environment; this is not a persistent cloud data lake deployment.

## Key Concepts Demonstrated

PySpark data processing, Delta Lake, Medallion Architecture, data ingestion, data validation, data-quality checks, quarantine handling, duplicate detection, window functions, analytical aggregation, and pipeline reconciliation.
