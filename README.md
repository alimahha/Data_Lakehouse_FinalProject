# Data Lakehouse & Data Engineering Project

## Overview

This academic project demonstrates a simple Data Lakehouse pipeline built around the Medallion Architecture (Bronze, Silver, and Gold). It uses MinIO as an S3-compatible object store and Python to process file metadata and prepare data for analytical use.

The pipeline creates a metadata inventory of files stored in the Bronze layer, saves the inventory as a CSV file in Silver, converts it to Parquet in Gold, and performs basic analysis on the resulting dataset.

## Architecture

The project follows three layers:

- **Bronze — Raw data:** stores the original files, such as PDF, DOCX, and JPG documents.
- **Silver — Processed data:** stores the file metadata inventory in CSV format.
- **Gold — Analytics-ready data:** stores the processed inventory in Parquet format for further analysis.

```text
Original files
      |
      v
  MinIO: Bronze
      |
      | Python + Boto3
      | Extract file metadata
      v
  MinIO: Silver
  metadata_inventory.csv
      |
      | Pandas
      | CSV-to-Parquet conversion
      v
  MinIO: Gold
  catalog_YYYYMMDD.parquet
      |
      v
  Analysis with Pandas
  - Total file size
  - Number of PDF files
  - PDF file inventory
```

## Technologies

- **Python** — pipeline implementation
- **Boto3** — connection to MinIO through the S3 API
- **MinIO** — object storage for the Bronze, Silver, and Gold layers
- **Pandas** — metadata processing and analysis
- **PyArrow** — Parquet support for Pandas
- **DuckDB / SQL** — used for analytical querying and data modelling as part of the broader project, where applicable

## Pipeline workflow

### 1. Bronze to Silver: metadata extraction

The script lists the objects stored in the `bronze` bucket and extracts metadata for each file:

- File name
- File size in bytes
- Last modification time
- File extension

The metadata is collected in a Pandas DataFrame, exported to `metadata_inventory.csv`, and uploaded to the `silver` bucket.

### 2. Silver to Gold: Parquet conversion

The script reads `metadata_inventory.csv` from the `silver` bucket, loads it into Pandas, converts the DataFrame to Parquet in memory, and uploads the resulting file to the `gold` bucket.

The output filename follows this pattern: `catalog_YYYYMMDD.parquet`.

### 3. Analysis

The script performs basic analysis on the catalog:

- Calculates the total size of the files listed in the inventory.
- Counts the files with the `.pdf` extension.
- Displays the names and modification times of PDF files.

## Prerequisites

- Python 3
- A running MinIO server
- MinIO buckets named `bronze`, `silver`, and `gold`
- Access credentials for MinIO

## Installation

It is recommended to use a virtual environment.

```bash
python3 -m venv venv
source venv/bin/activate
python -m pip install --upgrade pip
pip install boto3 pandas pyarrow
```

If you use DuckDB for the SQL part of the project, install it as well:

```bash
pip install duckdb
```

## Configuration

Configure the MinIO connection in the Python script:

```python
MINIO_CONF = {
    "endpoint_url": "http://127.0.0.1:9000",
    "aws_access_key_id": "YOUR_ACCESS_KEY",
    "aws_secret_access_key": "YOUR_SECRET_KEY",
}
```

Replace the example credentials with your own MinIO credentials. The endpoint above assumes that MinIO is running locally on port `9000`.

Make sure the `bronze`, `silver`, and `gold` buckets exist before running the pipeline. The Bronze bucket must contain the files to inventory.

## Run the pipeline

Start MinIO first, then open a terminal in the directory containing `script.py`. Activate the virtual environment if necessary and run:

```bash
source venv/bin/activate
python script.py
```

The script should print progress messages, the total size of the inventoried files, and the number and details of PDF files found.

## Expected outputs

After a successful run:

- **Bronze:** original files remain stored in the bucket.
- **Silver:** `metadata_inventory.csv`
- **Gold:** `catalog_YYYYMMDD.parquet`
- **Terminal:** summary statistics and a list of PDF files, if any are present.

The Parquet filename includes the date on which the pipeline runs.

## Notes and limitations

- The script inventories objects returned by a single `list_objects_v2` request. For buckets containing more than 1,000 objects, pagination should be added.
- The CSV inventory is recreated each time the script runs.
- The script requires the MinIO server and the three buckets to be available.
- The pipeline inventories file metadata; it does not extract the contents of PDF, DOCX, or JPG files.
- Store credentials securely rather than committing real access keys to a public repository.

## Possible improvements

- Add pagination for large buckets.
- Add logging and more detailed error handling.
- Validate file metadata and handle empty buckets consistently.
- Add automated tests and data-quality checks.
- Extend the analytical layer with DuckDB SQL queries.

## Author

**Ali Mahha & Valentin Tardy**  
Academic project — Data Lakehouse & Data Engineering  
June 2026
