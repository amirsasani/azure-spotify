# Azure Spotify Data Engineering Project

This repository contains an **end-to-end Azure Data Engineering project** built as part of my hands-on learning in cloud data engineering.

The project demonstrates how to ingest data from a real-world API (Spotify), process it using **Azure Databricks (PySpark)**, orchestrate workflows with **Azure Data Factory**, and store curated data for analytics.

🎥 Tutorial reference: https://youtu.be/vJM0wDrTRxI

---

## 📌 Project Overview

The objective of this project is to design and implement a **scalable ETL/ELT data pipeline on Azure** following modern data engineering best practices.

The pipeline:
- Extracts data from the **Spotify API**
- Stores raw data in **Azure Data Lake Storage**
- Transforms data using **PySpark in Azure Databricks**
- Loads curated datasets into **Azure SQL Database** for analytics

This project follows a **layered data architecture** (Bronze / Silver / Gold).

---

## 🧠 Technologies & Tools

- **Azure Data Factory** – Data orchestration & scheduling  
- **Azure Databricks** – Distributed data processing with PySpark  
- **Azure Data Lake Storage Gen2** – Raw & processed data storage  
- **Azure SQL Database** – Serving layer for analytics  
- **Spotify Web API** – Real-world data source  
- **Delta Lake** – Reliable and scalable data transformations  
- **Python / PySpark / SQL**  
- **Git & GitHub** – Version control  

---

## 🏗️ Architecture

**Data Flow:**

1. **Extract**  
   - Spotify data is fetched via REST API

2. **Load (Bronze Layer)**  
   - Raw JSON data is stored in Azure Data Lake

3. **Transform (Silver Layer)**  
   - Data is cleaned, normalized, and enriched using PySpark in Databricks

4. **Serve (Gold Layer)**  
   - Final tables are written to Azure SQL Database for querying and analytics

---

## 📂 Repository Structure

```text
azure-spotify/
├── dataset/                  # Dataset definitions for ADF
│   ├── azure_sql.json           # Azure SQL dataset configuration
│   ├── json_dynamic.json        # Dynamic JSON dataset
│   └── parquet_dynamic.json     # Dynamic Parquet dataset
├── factory/                  # Azure Data Factory resources
│   └── df-azureproject-amir.json
├── linkedService/            # Linked services configurations
│   ├── azure_sql.json           # Azure SQL connection
│   └── datalake.json            # Data Lake connection
├── pipeline/                 # ADF pipelines
│   ├── incremental_ingestion.json  # CDC-based incremental pipeline
│   └── incremental_loop.json       # Loop for multiple tables
├── databricks/               # Databricks notebooks (PySpark)
│   └── spotify_dab.dbc          # Transformation notebooks
├── publish_config.json       # ADF publish configuration
└── README.md
```

---

## 🎯 Key Features

### 1. **Incremental Data Loading (CDC)**
- Implementation of **Change Data Capture (CDC)** for incremental data loading
- Only new or updated records are transferred from Azure SQL to Data Lake
- Uses timestamp columns to identify changes
- Reduces execution time and data transfer costs

### 2. **Dynamic Pipelines**
- Parameterized pipelines that can work with different tables and schemas
- Uses metadata-driven approach
- Supports parallel execution for multiple tables using Loop Pipeline

### 3. **Layered Architecture (Medallion)**
- **Bronze Layer**: Raw data storage in Parquet format
- **Silver Layer**: Data cleansing, normalization, and enrichment
- **Gold Layer**: Final tables ready for analytics in Azure SQL

### 4. **Data Transformation with PySpark**
- Big data processing using Apache Spark in Databricks
- Complex transformations and aggregations
- Performance optimization using Delta Lake

---

## 🚀 Implementation Steps

### Step 1: Set Up Azure Resources
```bash
# Create Resource Group
az group create --name rg-spotify-project --location eastus

# Create Storage Account (Data Lake)
az storage account create \
  --name saazurespotify \
  --resource-group rg-spotify-project \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2 \
  --hierarchical-namespace true

# Create Azure SQL Database
az sql server create \
  --name sql-spotify-server \
  --resource-group rg-spotify-project \
  --location eastus \
  --admin-user sqladmin \
  --admin-password <strong-password>

az sql db create \
  --resource-group rg-spotify-project \
  --server sql-spotify-server \
  --name spotify-db \
  --service-objective S0
```

### Step 2: Configure Azure Data Factory
1. Create Data Factory instance
2. Create Linked Services:
   - **Azure SQL**: Connection to SQL database
   - **Data Lake Storage**: Connection to ADLS Gen2
3. Define Datasets:
   - `azure_sql.json`: For reading from SQL
   - `parquet_dynamic.json`: For writing Parquet to Data Lake
   - `json_dynamic.json`: For metadata management

### Step 3: Create Incremental Load Pipelines
**incremental_ingestion.json**: Main pipeline that:
- Reads the last CDC value from metadata
- Extracts new data from Azure SQL
- Stores data as Parquet in Bronze layer
- Updates metadata for next execution

**incremental_loop.json**: Loop pipeline that:
- Reads list of tables from a source
- Invokes the main pipeline for each table

### Step 4: Transformation in Databricks
1. Create Databricks workspace
2. Upload notebooks from `databricks/spotify_dab.dbc`
3. Execute PySpark transformations:
   - Read Parquet data from Bronze layer
   - Data cleansing and validation
   - Feature engineering
   - Write to Silver/Gold layers

### Step 5: Load to Azure SQL
- Use Databricks or ADF to write final data to Azure SQL
- Create optimized tables for analytical queries

---

## 📊 Data Pipeline Flow

```
Spotify API / Azure SQL
         ↓
   [Azure Data Factory]
         ↓
Bronze Layer (ADLS Gen2)
    - Raw Parquet files
         ↓
   [Azure Databricks]
    - PySpark transformations
         ↓
Silver Layer (ADLS Gen2)
    - Cleaned & enriched data
         ↓
   [Azure Databricks]
    - Aggregations & business logic
         ↓
Gold Layer (Azure SQL)
    - Analytics-ready tables
         ↓
   [Power BI / Analytics]
```

---

## 💡 Key Learnings

Through this project, I gained hands-on experience with:

✅ **Azure Data Factory**: 
- Building pipelines with different activities (Copy, Lookup, Set Variable)
- Using Parameters and Variables
- Expressions and Dynamic Content
- Scheduling and Triggers

✅ **Azure Data Lake Storage Gen2**:
- Layered structure (Bronze/Silver/Gold)
- Managing different file formats (Parquet, JSON)
- Containers and folder hierarchy

✅ **Azure Databricks**:
- Setting up Spark clusters
- Writing PySpark code for ETL
- Delta Lake and ACID transactions
- Performance optimization

✅ **Incremental Load Pattern**:
- Implementing CDC with timestamp columns
- Watermark management
- Metadata-driven pipelines

✅ **Azure SQL Database**:
- Schema design for analytical workloads
- Integration with ADF and Databricks
- Query optimization

---

## 🎓 Skills Developed

- **Cloud Data Engineering** on Microsoft Azure
- **ETL/ELT** pipeline design and implementation
- **PySpark** for distributed data processing
- **SQL** for data querying and transformations
- **Data Lake** architecture and best practices
- **Pipeline Orchestration** with Azure Data Factory
- **Incremental Loading** strategies (CDC)
- **Git/GitHub** for version control

---

## 🔗 Resources

- 📺 **Tutorial Video**: [Azure Data Engineering Project](https://youtu.be/vJM0wDrTRxI)
- 📖 **Azure Documentation**: [Azure Data Factory](https://learn.microsoft.com/azure/data-factory/)
- 📖 **Databricks Documentation**: [Azure Databricks](https://learn.microsoft.com/azure/databricks/)
- 🎵 **Spotify API**: [Web API Reference](https://developer.spotify.com/documentation/web-api/)

---

## 🤝 Contributing

This project was created for educational purposes. If you have suggestions or improvements, I'd love to hear them!

---

## 📝 License

This project is released under the MIT License.

---

## 👨‍💻 Author

**Amir Sasani**  
- GitHub: [@amirsasani](https://github.com/amirsasani)
- Repository: [azure-spotify](https://github.com/amirsasani/azure-spotify)

---

## 🙏 Acknowledgments

Special thanks to the creators of the tutorial video that inspired this project and helped me learn Data Engineering concepts in Azure.
