# ⚡ Fintech Transaction Pipeline

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/PySpark-4.0.4-orange?logo=apachespark&logoColor=white" />
  <img src="https://img.shields.io/badge/Delta_Lake-4.4.1-00A36C?logo=databricks&logoColor=white" />
  <img src="https://img.shields.io/badge/Architecture-Medallion-8A2BE2" />
  <img src="https://img.shields.io/badge/Status-Completed-success" />
</p>

An end-to-end fintech transaction data pipeline built using **Python, PySpark, and Delta Lake**, implementing the **Medallion Architecture (Bronze, Silver, and Gold)**.

The project simulates transaction ingestion, data quality issues, validation, quarantine handling, deduplication, and analytical aggregation.

---

## 📊 Project Highlights

| Metric | Result |
|:---|---:|
| 📥 Original transactions | **100,000** |
| 🔄 Incoming records with issues | **107,000** |
| 🥉 Bronze records | **107,000** |
| 🚨 Quarantined records | **5,000** |
| 🥈 Clean Silver records | **100,000** |
| 🥇 Gold analytical records | **360** |

---

## 🏗️ Pipeline Architecture

The pipeline follows a Medallion Architecture with separate processing stages for raw ingestion, validation, cleaning, and aggregation.

<table>
  <tr>
    <td align="center" bgcolor="#1E3A8A">
      <strong><font color="white">📄 RAW DATA</font></strong><br/>
      <font color="white">100K Transactions</font>
    </td>
  </tr>
  <tr><td align="center">⬇️</td></tr>
  <tr>
    <td align="center" bgcolor="#2563EB">
      <strong><font color="white">🥉 BRONZE LAYER</font></strong><br/>
      <font color="white">107K Incoming Records · Delta Lake</font>
    </td>
  </tr>
  <tr><td align="center">⬇️</td></tr>
  <tr>
    <td align="center" bgcolor="#7C3AED">
      <strong><font color="white">🔍 DATA VALIDATION</font></strong><br/>
      <font color="white">Required fields · Amounts · Currency · Status</font>
    </td>
  </tr>
  <tr>
    <td align="center">
      <table>
        <tr>
          <td align="center" bgcolor="#047857">
            <strong><font color="white">✅ VALID RECORDS</font></strong><br/>
            <font color="white">102K</font>
          </td>
          <td align="center" bgcolor="#B91C1C">
            <strong><font color="white">🚨 QUARANTINE</font></strong><br/>
            <font color="white">5K Invalid Records</font>
          </td>
        </tr>
      </table>
    </td>
  </tr>
  <tr><td align="center">⬇️ Valid records</td></tr>
  <tr>
    <td align="center" bgcolor="#0E7490">
      <strong><font color="white">🧹 DEDUPLICATION</font></strong><br/>
      <font color="white">2K Duplicate Rows Removed</font>
    </td>
  </tr>
  <tr><td align="center">⬇️</td></tr>
  <tr>
    <td align="center" bgcolor="#047857">
      <strong><font color="white">🥈 SILVER LAYER</font></strong><br/>
      <font color="white">100K Clean Records · Delta Lake</font>
    </td>
  </tr>
  <tr><td align="center">⬇️</td></tr>
  <tr>
    <td align="center" bgcolor="#0E7490">
      <strong><font color="white">📊 GOLD AGGREGATION</font></strong><br/>
      <font color="white">Daily · Currency · Transaction Status</font>
    </td>
  </tr>
  <tr><td align="center">⬇️</td></tr>
  <tr>
    <td align="center" bgcolor="#15803D">
      <strong><font color="white">🥇 GOLD LAYER</font></strong><br/>
      <font color="white">360 Analytical Records · Delta Lake</font>
    </td>
  </tr>
</table>

### 🥉 Bronze Layer
- Ingests incoming transaction records using PySpark.
- Stores raw data in Delta Lake format.
- Profiles missing values, invalid records, and duplicate transaction IDs.

### 🥈 Silver Layer
- Applies data quality validation.
- Separates invalid records into a quarantine table.
- Removes duplicate transactions using PySpark window functions.
- Stores validated and deduplicated records in Delta Lake.

### 🥇 Gold Layer
- Generates daily transaction summaries.
- Groups data by transaction date, currency, and transaction status.
- Calculates transaction counts, total amounts, and average amounts.

---

## 🛠️ Technology Stack

| Technology | Purpose |
|:---|:---|
| 🐍 Python | Data generation and pipeline logic |
| ⚡ PySpark 4.0.4 | Distributed data processing |
| 🟢 Delta Lake 4.4.1 | Transactional data storage |
| 🐼 Pandas & NumPy | Synthetic data generation |
| ☁️ Google Colab | Cloud-based development environment |
| 📓 Jupyter Notebook | Pipeline implementation |

---

## 🔄 Pipeline Workflow

**1. Data Generation**
- Generated 100,000 synthetic fintech transactions.
- Included transaction IDs, customer IDs, amounts, currencies, statuses, and timestamps.
- Used USD, INR, EUR, and GBP.

**2. Data Quality Issue Injection**

Introduced controlled data quality issues:

| Issue | Records |
|:---|---:|
| Duplicate rows | 2,000 |
| Missing customer IDs | 1,000 |
| Missing amounts | 1,000 |
| Invalid transaction statuses | 1,000 |
| Negative transaction amounts | 1,000 |
| Missing currencies | 1,000 |

**3. Bronze Ingestion**
- Loaded 107,000 incoming records.
- Persisted raw records using Delta Lake.
- Performed initial data profiling.

**4. Validation & Quarantine**
- Applied validation rules for required fields, amounts, currencies, and statuses.
- Routed 5,000 invalid records into a separate quarantine table.
- Captured rejection reasons for invalid records.
- Passed 102,000 valid records to the next stage.

**5. Silver Processing**
- Applied deterministic deduplication using PySpark window functions.
- Produced 100,000 unique transaction records.
- Performed data quality checks before persisting the Silver table.

**6. Gold Aggregation**
- Grouped transactions by date, currency, and status.
- Calculated total transaction count, total amount, and average amount.
- Generated 360 analytical records.

**7. Pipeline Validation & Reporting**
- Checked record distribution across processing stages.
- Validated Silver and Gold data quality.
- Compared Silver and Gold aggregate amounts.
- Generated an automated JSON validation report.

---

## ✅ Execution Results

| Pipeline Stage | Records | Status |
|:---|---:|:---:|
| Original synthetic dataset | 100,000 | 🟢 |
| Incoming dataset | 107,000 | 🟢 |
| Bronze | 107,000 | 🟢 |
| Quarantine | 5,000 | 🟢 |
| Valid records before deduplication | 102,000 | 🟢 |
| Silver after deduplication | 100,000 | 🟢 |
| Gold aggregated records | 360 | 🟢 |

### 🔎 Data Quality Results

| Validation Check | Result |
|:---|:---:|
| Duplicate transaction IDs in Silver | 🟢 0 |
| Invalid amounts in Silver | 🟢 0 |
| Missing customer IDs in Silver | 🟢 0 |
| Missing currencies in Silver | 🟢 0 |
| Invalid statuses in Silver | 🟢 0 |
| Duplicate Gold aggregation keys | 🟢 0 |
| Pipeline validation | 🟢 PASSED |

---

## 🚀 How to Run

The project runs in Google Colab and does not require local installation.

1. Open the notebook using the link below.
2. Select **Runtime → Run all**.
3. Allow the notebook to install the required dependencies.
4. Execute the pipeline and review the results.

<p align="center">
  <a href="https://colab.research.google.com/github/harsha123e/fintech-transaction-pipeline/blob/main/Fintech_Transaction_Pipeline.ipynb">
    <img src="https://img.shields.io/badge/Open%20in-Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white" />
  </a>
</p>

---

## ⚠️ Project Scope & Limitations

- Transaction data is synthetically generated and does not represent real customer or payment information.
- The pipeline runs in Google Colab using its available compute environment.
- Delta Lake tables are stored in the notebook execution environment; this is not a persistent cloud data lake deployment.
- No production cloud infrastructure, orchestration, or streaming implementation is included.
---

## 🎯 Key Concepts Demonstrated

`PySpark` · `Delta Lake` · `Medallion Architecture` · `Data Ingestion` · `Data Validation` · `Data Quality` · `Quarantine Handling` · `Deduplication` · `Window Functions` · `Analytical Aggregation` · `Data Reconciliation`

---

<p align="center">
  Built as a hands-on Data Engineering portfolio project.
</p>
