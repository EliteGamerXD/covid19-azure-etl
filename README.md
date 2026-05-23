# COVID-19 Data Platform — Azure End-to-End ETL Pipeline

## Overview
An end-to-end data engineering pipeline built on Azure to ingest, transform, and serve COVID-19 data for reporting. The pipeline ingests raw data from multiple sources, processes it through transformation layers using Azure Data Factory and Databricks, and loads the output into Azure SQL Database for downstream consumption in Power BI.

---

## Architecture

![Architecture Diagram](screenshots/architecture.png)

---

## Data Sources
- **ECDC (European Centre for Disease Prevention and Control)** — COVID-19 case counts, hospital admissions, ICU occupancy, and testing data ingested via HTTP linked service
- **Eurostat Population Data** — uploaded as flat files to Azure Blob Storage and used as lookup data for per-capita calculations

---

## Pipeline Stages

### 1. Ingestion
- HTTP-based ingestion of ECDC datasets via ADF Copy Activity
- Blob Storage ingestion for population and lookup data
- Raw files landed in Azure Data Lake Storage Gen2 (raw layer)

### 2. Transformation
- **ADF Mapping Data Flows** — visual transformations including:
  - Filter, Select, Pivot, Lookup, Conditional Split
  - Derived Column, Aggregate, Join
  - Used to clean and reshape cases, hospital admissions, and testing data
- **Azure Databricks (PySpark)** — additional transformations on population 
  data using PySpark notebooks
- Processed data written to ADLS Gen2 (processed layer)

### 3. Loading
- Processed datasets loaded into **Azure SQL Database** via ADF pipelines

### 4. Reporting
- Power BI connected to Azure SQL Database for COVID-19 trend reporting
  (cases, hospital admissions, ICU occupancy by country and date)

---

## Azure Services Used
| Service | Purpose |
|---|---|
| Azure Data Factory | Pipeline orchestration, ingestion, data flows |
| Azure Data Lake Storage Gen2 | Raw and processed data storage |
| Azure Blob Storage | Source file storage |
| Azure Databricks | PySpark-based transformation |
| Azure SQL Database | Serving layer for reporting |
| Azure DevOps | Source control and CI/CD pipeline integration |
| Power BI | Reporting and visualisation |

---

## Key ADF Concepts Implemented
- Parameterised pipelines, datasets, and linked services
- Control flow activities: Get Metadata, If Condition, ForEach, Validation
- Trigger types: Schedule Trigger, Tumbling Window Trigger, Event Trigger
- Pipeline debugging and monitoring via ADF Monitor


## Notes
This project was built as part of the 
[Azure Data Factory For Data Engineers](https://www.udemy.com/course/learn-azure-data-factory-from-scratch/) 
course by Ramesh Retnasamy. The pipeline implementation, transformations, and 
architecture were built hands-on throughout the course.
