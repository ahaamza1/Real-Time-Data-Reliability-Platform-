# 🚖 RideFusion: End-to-End Real-Time Ride Analytics Platform

<img width="1536" height="1024" alt="archeticture " src="https://github.com/user-attachments/assets/66813a55-651c-4918-8489-b67eff38b6f5" />


<p align="center">

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
![PySpark](https://img.shields.io/badge/PySpark-Structured%20Streaming-orange)
![Azure Databricks](https://img.shields.io/badge/Azure-Databricks-blue)
![Azure Event Hub](https://img.shields.io/badge/Azure-EventHub-0078D4)
![Delta Lake](https://img.shields.io/badge/Delta-Lake-purple)
![Power BI](https://img.shields.io/badge/PowerBI-Dashboard-yellow)
![SQL](https://img.shields.io/badge/SQL-Star%20Schema-success)
![License](https://img.shields.io/badge/License-MIT-green)

</p>

---

# 📑 Table of Contents

- Project Overview
- Business Problem
- Business Objectives
- Solution Architecture
- Technology Stack
- Data Pipeline
- Medallion Architecture
- Bronze Layer
- Silver Layer
- One Big Table (OBT)
- Gold Layer
- Star Schema
- Project Workflow
- Repository Structure
- Features
- Dashboard
- Installation
- Future Improvements
- Author

---

# 📌 Project Overview

RideFusion is a modern end-to-end Data Engineering platform designed to process both **real-time streaming events** and **historical batch data** within a unified analytics pipeline.

The project demonstrates how enterprise-grade data platforms can continuously ingest, transform, and model large-scale ride-sharing data using Microsoft's Azure ecosystem and Apache Spark technologies.

Unlike traditional ETL pipelines that process only historical datasets, RideFusion combines streaming ingestion from Azure Event Hub with historical ride records to produce near real-time business analytics.

The project follows the **Medallion Architecture** (Bronze, Silver, Gold) to ensure scalable, reliable, and maintainable data processing while providing dimensional models optimized for reporting and business intelligence.

The final analytical model is visualized through interactive Power BI dashboards built on top of a Star Schema warehouse.

---

# 🎯 Business Problem

Ride-sharing platforms continuously generate ride events from thousands of drivers and passengers every second.

These events include:

- Ride requests
- Ride confirmations
- Pickup information
- Drop-off information
- Payment transactions
- Driver ratings

At the same time, organizations maintain historical ride datasets stored in cloud storage.

Traditional analytics solutions usually separate streaming pipelines from batch pipelines, making reporting more complicated and introducing unnecessary maintenance overhead.

RideFusion solves this challenge by integrating both streaming and historical datasets into a single scalable architecture capable of supporting real-time analytics.

---

# 🎯 Business Objectives

The project aims to:

- Build a production-style Data Engineering pipeline.
- Combine streaming and batch ingestion.
- Process ride events in near real time.
- Implement the Medallion Architecture.
- Build reusable transformation pipelines.
- Automate SQL generation.
- Create a dimensional data warehouse.
- Support Slowly Changing Dimensions.
- Deliver business-ready Power BI dashboards.

---

# 🏗️ Solution Architecture

```
                  Web / Mobile Application
                           │
                    JSON Ride Events
                           │
                           ▼
                  Azure Event Hub (Kafka)
                           │
                           ▼
          Apache Spark Structured Streaming
                           │
        ┌──────────────────┴──────────────────┐
        │                                     │
Historical Ride Files                 Streaming Events
        │                                     │
        └──────────────Merge──────────────────┘
                           │
                           ▼
                  Bronze Delta Tables
                           │
                           ▼
             Silver Layer (Cleaning + OBT)
                           │
                           ▼
         Gold Layer (Fact + Dimension Tables)
                           │
                           ▼
               Interactive Power BI Dashboard
```

---

# 💻 Technology Stack

| Category | Technology |
|-----------|------------|
| Cloud Platform | Microsoft Azure |
| Streaming Platform | Azure Event Hub |
| Compute Engine | Azure Databricks |
| Processing Framework | Apache Spark |
| Streaming Engine | Structured Streaming |
| Programming Language | Python |
| Framework | PySpark |
| Data Storage | Delta Lake |
| SQL Engine | Spark SQL |
| SQL Automation | Jinja2 |
| Data Modeling | Star Schema |
| CDC | Auto CDC |
| Version Control | Git & GitHub |
| Visualization | Power BI |

---

# 🔄 Data Pipeline

RideFusion combines streaming and historical ride data inside one unified Spark pipeline.

The pipeline performs the following operations:

1. Receive live ride events from Azure Event Hub.
2. Read historical ride datasets from Azure Storage.
3. Store raw data inside Bronze Delta Tables.
4. Parse incoming JSON records.
5. Validate incoming schema.
6. Clean and standardize the data.
7. Merge streaming and historical records.
8. Enrich ride data using lookup tables.
9. Generate a One Big Table (OBT).
10. Build Star Schema dimension and fact tables.
11. Publish analytical data for Power BI.

---

# 🥉 Bronze Layer

The Bronze layer is responsible for ingesting raw data exactly as received from the source systems.

No business transformations are applied at this stage.

Its primary objective is preserving the original data while providing a reliable source for downstream processing.

## Streaming Ingestion

Streaming ride events are received from Azure Event Hub using the Kafka-compatible interface.

Incoming JSON messages are continuously consumed through Apache Spark Structured Streaming.

The raw payload is stored without modification, allowing replay and recovery if needed.

## Batch Ingestion

Historical ride data and reference lookup datasets are loaded into the Bronze layer from cloud storage.

Reference datasets include:

- Vehicle Makes
- Vehicle Types
- Ride Statuses
- Cities
- Payment Methods
- Cancellation Reasons

By combining streaming and batch ingestion, the Bronze layer becomes the single source of truth for the entire platform.

---

# 🥈 Silver Layer

The Silver layer transforms raw ride events into clean, validated, and analytics-ready datasets.

Key transformations include:

- JSON Parsing
- Schema Enforcement
- Data Type Conversion
- Duplicate Removal
- Null Handling
- Data Standardization
- Business Rule Validation

Both streaming and historical datasets are merged into a unified staging table before enrichment begins.

---

# ⚙️ One Big Table (OBT)

The Silver layer generates a One Big Table (OBT) by joining the staging ride dataset with multiple lookup tables.

The lookup datasets enrich ride records with descriptive business information instead of numeric identifiers.

The OBT includes information about:

- Vehicle Types
- Vehicle Makes
- Payment Methods
- Ride Status
- Pickup Cities
- Cancellation Reasons

This enriched dataset becomes the foundation for dimensional modeling in the Gold layer.

---

# 🚀 Dynamic SQL Generation Using Jinja2

Instead of manually writing large SQL statements with multiple JOIN operations, RideFusion uses **Jinja2** templates to dynamically generate SQL queries.

The configuration-driven approach offers several advantages:

- Reduces duplicated SQL code.
- Simplifies maintenance.
- Improves scalability.
- Makes adding new lookup tables significantly easier.
- Keeps SQL readable and modular.

This technique allows complex SQL statements to be generated automatically from reusable configuration objects rather than manually editing lengthy queries.

---

# 🥇 Gold Layer

The Gold layer represents the business-ready analytical model built on top of the cleansed Silver layer.

Instead of storing all ride information in a single denormalized table, RideFusion organizes the data into a dimensional warehouse following the **Star Schema** design.

This model significantly improves query performance, simplifies reporting, and enables interactive business intelligence dashboards.

The Gold layer is continuously updated using **Lakeflow Declarative Pipelines** and **Auto CDC**, ensuring that analytical tables remain synchronized with incoming streaming events.

---

# ⭐ Star Schema

The analytical warehouse consists of one fact table and multiple dimension tables.

## Fact Table

| Table | Description |
|--------|-------------|
| Fact_Rides | Stores ride metrics and foreign keys to dimensions |

### Measures

- Total Fare
- Base Fare
- Distance Fare
- Time Fare
- Tip Amount
- Distance Miles
- Duration Minutes
- Surge Multiplier
- Driver Rating

---

## Dimension Tables

### 👤 Dim_Passenger

Stores passenger information.

Attributes include:

- Passenger ID
- Passenger Name
- Email
- Phone Number

---

### 🚗 Dim_Driver

Stores driver information.

Attributes include:

- Driver ID
- Driver Name
- Rating
- Phone Number
- License Number

---

### 🚙 Dim_Vehicle

Stores vehicle information.

Attributes include:

- Vehicle
- Vehicle Type
- Vehicle Make
- Vehicle Model
- Vehicle Color
- License Plate

---

### 💳 Dim_Payment

Stores payment method information.

Attributes include:

- Payment Method
- Card Indicator
- Authentication Requirement

---

### 📍 Dim_Location

Stores pickup city information.

Attributes include:

- City
- Region
- State

This dimension is implemented using **Slowly Changing Dimension Type 2** to preserve historical changes.

---

### 📅 Dim_Booking

Stores booking and trip metadata.

Attributes include:

- Booking Time
- Pickup Time
- Drop-off Time
- Pickup Address
- Drop-off Address
- Pickup Coordinates
- Drop-off Coordinates
- Ride Status
- Cancellation Reason

---

# 🔄 Auto CDC

RideFusion leverages **Auto CDC** provided by Lakeflow Declarative Pipelines to automatically propagate changes from the Silver layer into Gold tables.

The project implements two Slowly Changing Dimension strategies.

## Type 1

Applied to dimensions whose previous values are not required.

Examples:

- Passenger
- Driver
- Vehicle
- Payment

Old values are overwritten whenever new information arrives.

---

## Type 2

Applied to the Location dimension.

Historical records are preserved by maintaining validity periods, allowing analysts to track changes over time without losing historical context.

---

# 📂 Repository Structure

```text
RideFusion-End-to-End-Real-Time-Ride-Analytics-Platform
│
├── README.md
├── LICENSE
├── .gitignore
├── requirements.txt
├── CONTRIBUTING.md
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
│
├── docs/
│   ├── Project_Documentation.pdf
│   ├── Architecture.md
│   └── Data_Dictionary.md
│
├── notebooks/
│
│   ├── bronze/
│   │     bronze_adls.ipynb
│   │     ingest.py
│   │
│   ├── silver/
│   │     silver.py
│   │     silver_obt.ipynb
│   │     silver_obt.sql
│   │
│   └── gold/
│         model.py
│
├── dashboard/
│     RideFusion.pbix
│
├── images/
│
└── datasets/
```

---

# ✨ Key Features

- End-to-End Data Engineering Pipeline
- Batch + Streaming Integration
- Azure Event Hub Streaming
- Apache Spark Structured Streaming
- Lakeflow Declarative Pipelines
- Delta Lake Storage
- Medallion Architecture
- Dynamic SQL Generation using Jinja2
- One Big Table (OBT)
- Star Schema Data Warehouse
- Auto CDC
- Slowly Changing Dimensions (SCD Type 1 & Type 2)
- Interactive Power BI Dashboard
- Near Real-Time Analytics

---

# 📊 Dashboard

The Power BI dashboard provides interactive business insights built on top of the Gold layer.

Key metrics include:

- Total Revenue
- Total Trips
- Average Fare
- Average Trip Duration
- Average Driver Rating
- Average Tip Amount
- Revenue by City
- Revenue by Vehicle Type
- Revenue by Payment Method
- Peak Ride Hours
- Ride Status Distribution
- Driver Performance Analysis

> Dashboard screenshots are available in the `images/` directory.

---

# 🚀 Pipeline Workflow

```text
Batch Files
        │
        │
        ▼
Azure Storage
        │
        │
        ▼
Bronze Layer
        ▲
        │
Azure Event Hub
        ▲
        │
Web Application
        │
        ▼
Silver Layer
(Data Cleaning + OBT)
        │
        ▼
Gold Layer
(Star Schema)
        │
        ▼
Power BI
```

---

# ⚙️ Installation

Clone the repository

```bash
git clone https://github.com/your-username/RideFusion-End-to-End-Real-Time-Ride-Analytics-Platform.git
```

Move to the project directory

```bash
cd RideFusion-End-to-End-Real-Time-Ride-Analytics-Platform
```

Install dependencies

```bash
pip install -r requirements.txt
```

Configure:

- Azure Databricks
- Azure Event Hub
- Azure Storage
- Spark Configuration

Execute the Bronze, Silver, and Gold pipelines.

Finally, connect Power BI to the Gold tables.

---

# 📈 Business Value

RideFusion demonstrates how modern cloud-native data platforms can unify historical and real-time data into a scalable analytics solution.

The project follows industry best practices for:

- Data Lakehouse Design
- Streaming Analytics
- Data Warehousing
- Dimensional Modeling
- Data Quality
- Cloud Data Engineering

---

# 🔮 Future Improvements

Potential enhancements include:

- Apache Airflow Orchestration
- Delta Live Expectations
- CI/CD with GitHub Actions
- Infrastructure as Code using Terraform
- Azure Key Vault Integration
- Unity Catalog
- Real-Time Monitoring
- Machine Learning Demand Forecasting
- Real-Time Alerting
- Docker Deployment

---

# 👨‍💻 Authors

**1)Abdulrahman Hamza**

**2)Ezzat mohamed**

**3)Mohamed Atif**

**4)Mohsen Khaled**

**5)Yaseen Ayman**

Computer Science Student

Data Engineering | Data Analytics | Azure | Apache Spark | Power BI

LinkedIn:

> www.linkedin.com/in/abdulrahman-hamza
> www.linkedin.com/in/m0hamed-atif
GitHub:

> github.com/your-username

---

# 📜 License

This project is licensed under the MIT License.

See the LICENSE file for more information.

---

# ⭐ Support

If you found this project useful, consider giving it a ⭐ on GitHub.

Contributions, suggestions, and feedback are always welcome.
