# azure1-project
# Azure Spotify Incremental Data Pipeline 🎵

An end-to-end incremental data ingestion pipeline built on Microsoft Azure, 
loading Spotify data from Azure SQL Database to Azure Data Lake Storage Gen2 
using Change Data Capture (CDC) with automated email alerts.

---

## 🏗️ Architecture

Azure SQL DB → Azure Data Factory → ADLS Gen2 (Bronze Layer) → Logic App → Email Alert

---

## 📁 Project Structure

| Folder | Description |
|---|---|
| `pipeline/` | ADF pipeline JSON definitions |
| `dataset/` | ADF dataset configurations |
| `linkedService/` | Linked service connection configs |
| `factory/` | ADF factory settings |
| `source_script/` | SQL scripts for source tables |
| `Databricks Code/` | Databricks notebooks |
| `screenshots/` | Pipeline screenshots |
| `loop_input` | ForEach loop table configuration |

---

## 📊 Tables Ingested

| Table | CDC Column | Type |
|---|---|---|
| DimArtist | updated_at | Dimension |
| DimDate | date | Dimension |
| DimTrack | updated_at | Dimension |
| DimUser | updated_at | Dimension |
| FactStream | stream_timestamp | Fact |

---


## 📸 Pipeline Screenshots

### Main Pipeline (ForEach + Web Alert)
https://github.com/lavanya6143/azure1-project/blob/main/screenshot/foreach_if_condition.png

### ForEach Inner Activities + If Condition
![ForEach If Condition](screenshots/foreach_if_condition.png)

### Email Notification on Failure
[screenshot/email_notification.png.PNG](https://github.com/lavanya6143/azure1-project/blob/main/screenshot/email_notification.png.PNG)

---

## 🔄 Pipeline Flow

1. **ForEach Loop** — iterates over all 5 tables dynamically
2. **last_cdc (Lookup)** — reads last CDC timestamp from bronze storage
3. **current (Set Variable)** — captures current UTC timestamp
4. **azureSQLToLake (Copy Data)** — copies incremental rows to ADLS Gen2
5. **IfIncrementalData (If Condition)**
   - ✅ True: updates CDC watermark
   - ❌ False: deletes empty files

---


## ⚙️ Azure Services Used

- **Azure SQL Database** — source data
- **Azure Data Factory** — orchestration
- **Azure Data Lake Storage Gen2** — bronze layer storage
- **Azure Logic App** — email notification on failure
- **Azure Databricks** — data transformation

---
