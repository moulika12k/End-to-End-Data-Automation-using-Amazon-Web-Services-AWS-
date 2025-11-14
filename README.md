# AWS ETL Pipeline for Log and CSV Data

This project showcases an automated, end-to-end ETL (Extract, Transform, Load) pipeline built using AWS services. The pipeline processes two types of data — **application log files** and **customer tickets (CSV)** — transforms them into analytics-ready Parquet files, stores them in S3, loads them into **Amazon Redshift**, and finally connects the data to **Power BI** for reporting.

---

## 🚀 Project Overview

The goal of this project was to build a fully automated pipeline that:

* Ingests raw log and CSV files from S3
* Cleans, transforms, and converts them to Parquet
* Loads processed data into Amazon Redshift
* Enables fast, real-time reporting in Power BI

This project was built as part of a guided learning experience and helped me understand AWS data engineering concepts in a real-world flow.

---

## 🧱 Architecture Summary

### **Data Sources**

* MySQL database exported as CSV
* Application log files stored in S3

### **AWS Services Used**

* **AWS Lambda** – Automation & serverless transformations for log data
* **AWS Glue** – Visual ETL workflow for ticket (CSV) data
* **AWS S3** – Central storage for raw, processed, and final Parquet data
* **AWS Glue Crawler** – Schema detection for Athena and Redshift
* **Amazon Athena** – SQL queries on S3 for validation & exploratory analysis
* **Amazon Redshift Serverless** – Data warehouse for analytics
* **CloudWatch** – Monitoring Lambda executions

---

## 🔄 Pipeline Flow

### **1. Raw Data Ingestion**

* Log files uploaded to `s3://bucket/raw/logs/`
* Ticket CSV files uploaded to `s3://bucket/raw/tickets/`

### **2. Log File Pipeline (AWS Lambda)**

* Lambda triggers automatically when a new log file arrives
* Cleans data (remove nulls, rename fields, type conversions)
* Converts to Parquet
* Stores results in `s3://bucket/processed/logs/`

### **3. Ticket CSV Pipeline (AWS Glue)**

* Glue job created using the visual editor
* Steps performed:

  * Change schema and data types
  * Drop nulls and empty strings
  * Rename fields (e.g., `issuecat` → `issue_category`)
  * Filter rows (`num_interactions >= 0`)
  * Apply SQL CASE transformations
  * Reorder/select final output columns
* Outputs stored in `s3://bucket/processed/tickets/`

### **4. Schema Generation (Glue Crawler)**

* Crawlers run on processed folders
* Creates/updates tables inside Glue Data Catalog

### **5. Querying with Athena**

* Used for validating processed data
* Performed grouping, aggregation, and exploratory SQL analysis

### **6. Loading into Redshift**

* Created Redshift database and tables
* Used COPY command to load processed Parquet files from S3

### **7. Reporting with Power BI**

* Power BI connected to Amazon Redshift
* Data loaded **already transformed**, enabling faster refresh

---

## 📊 Key Outcomes

* Built **two AWS data pipelines** (Lambda + Glue)
* Automated transformations end-to-end
* Converted raw files to optimized **Parquet format**
* Centralized analytics using Amazon Redshift
* Connected Redshift to Power BI for fast reporting

---

## 🛠️ Tools & Technologies

* Python (basic scripting inside Lambda / Glue-generated code)
* AWS Lambda
* AWS Glue & Glue Visual ETL
* AWS Glue Crawler
* Amazon S3
* Amazon Athena
* Amazon Redshift Serverless
* Power BI

---

## 📁 Project Structure

```
├── raw_data/
│   ├── logs/
│   └── tickets/
├── processed_data/
│   ├── logs_parquet/
│   └── tickets_parquet/
├── lambda_functions/
│   └── process_logs.py
└── glue_jobs/
    └── process_tickets_script.py
```

---

## 💬 Notes

* This was a guided project where I focused heavily on understanding the pipeline flow.
* The Python used inside Lambda/Glue was advanced for my level, but I fully understand the **architecture, purpose, and end-to-end data flow**.
* I can clearly explain how data moves from MySQL → S3 → Lambda/Glue → Redshift → Power BI.

---

## 📌 Future Improvements

* Add CI/CD using AWS CodePipeline
* Add error handling and DLQs for Lambda
* Add orchestration using AWS Step Functions

---

## 🙌 Acknowledgements

Special thanks to CodeBasics for the guided project structure and explanations.

---

A visual architecture diagram, is added for your reference.
