# Data Lakehouse – Final Project

Integrated data platform built on a **Data Lakehouse** architecture (**MinIO** for object storage, **DuckDB** for compute) following the **Medallion** pattern (Bronze → Silver → Gold). Data from WikiData and a second open data source is ingested, cleaned, versioned with **Delta Lake**, and modeled as an analytical **Data Warehouse** to answer business questions with SQL.

> Academic group project – Data Lakehouse course, Institut Teknologi Sepuluh Nopember (ITS), Surabaya, Indonesia (February – July 2026).

**Links:** [Presentation](./Presentation.pptx) · [Demo video](TODO: YouTube link)

---

## Context and goal

**Business context:** TODO (1–2 sentences: which topic, which questions the platform answers, e.g. "analyze … across Indonesian provinces").

**Goal:** build a secure, reliable and optimized pipeline from raw sources to an analytical Data Warehouse, acting as a team of Data Platform Engineers and Analytics Engineers.

**Team:** TODO (names / GitHub profiles, and your own contribution in one line).

---

## Data sources

| Source | Format | Content | Role |
|---|---|---|---|
| WikiData (API) | JSON | TODO | Main source |
| TODO (e.g. BPS / Kaggle / Satu Data Indonesia) | TODO | TODO | Supporting source |

---

## Architecture

```
 WikiData API ─┐
               ├─►  BRONZE (raw)  ─►  SILVER (clean, Delta Lake)  ─►  GOLD (Data Warehouse)  ─►  Analytical SQL
 Other source ─┘      MinIO              MinIO + delta_log              TODO: star / snowflake / galaxy       DuckDB
```

TODO: add an architecture diagram (image) here.

### Bronze – raw data
- Raw files pulled directly from the sources (JSON / other formats), stored in MinIO.
- **Idempotent ingestion:** a checksum check (MD5 / ETag) detects whether the source changed, so unchanged files are not ingested twice (see `checksums.csv`).

### Silver – cleaned data
- Cleaning of anomalous values, handling of missing values (e.g. `NA` / `NULL` strings), consistent data types, normalization.
- **Versioning and audit** with Delta Lake (`_delta_log`): change history, time-travel queries, auditability.

### Gold – analytical model
- Data modeled as a **TODO: star schema / snowflake schema / galaxy schema**.
- Fact table(s): TODO. Dimension tables: TODO.

---

## Tech stack

Python (Pandas) · SQL · DuckDB · MinIO (S3-compatible object storage) · Delta Lake · TODO (other: Docker, notebooks…)

---

## Repository structure

```
.
├── data/                 # TODO: describe (samples? local data?)
├── scripts/              # TODO: describe main scripts (ingestion, silver, gold…)
├── checksums.csv         # checksums used for idempotent ingestion
├── Presentation.pptx     # project presentation
└── README.md
```

---

## How to run

1. **Prerequisites:** TODO (Python version, MinIO running, e.g. via Docker).
2. **Install dependencies:** `pip install -r requirements.txt` (TODO: add this file).
3. **Configure MinIO:** TODO (endpoint, bucket names; never commit credentials – use environment variables).
4. **Run the pipeline:**
   ```bash
   # TODO: commands in order (bronze → silver → gold)
   ```

---

## Analytical queries

Each query uses at least an aggregation function and a `WHERE` filter, and runs on the Gold layer with DuckDB.

| # | Business question | Author |
|---|---|---|
| 1 | TODO | TODO |
| 2 | TODO | TODO |

Example (TODO: replace with one of your real queries):

```sql
-- TODO
```

TODO: add one short result (table or screenshot) to show what the query returns.

---

## What I learned / key points

- Designing a Medallion architecture on object storage.
- Making ingestion idempotent with checksums.
- Versioning and auditing data with Delta Lake.
- Modeling a Data Warehouse and answering business questions with SQL.

TODO: add 1–2 real difficulties you solved (e.g. a data quality issue in WikiData, a design choice).
