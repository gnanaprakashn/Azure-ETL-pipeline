
# 🚀 Azure ETL Pipeline (SSMS → ADF → Blob Storage → Databricks → SSMS)

This project demonstrates a complete end-to-end **Azure ETL pipeline** where denormalized data stored in **SQL Server (SSMS)** is normalized using **Azure Data Factory (ADF)** and **Azure Databricks**, transformed into a **star schema**, and loaded back into SQL Server.

This is a real-world enterprise ETL pattern used by data engineering teams.

---

## 🏗️ Architecture


<img src="Azure ETL pipeline.png" width="850"/>

**Flow:**  
SSMS → ADF Copy Activity → Blob Storage (Raw) → Databricks Transform → Blob Storage (Processed) → ADF Copy Activity → SSMS (Star Schema)

---

## 📁 Repository Structure

```

Azure-ETL-Pipeline/
│
├── Sample - Superstore_Orders.csv     # Raw denormalized dataset
├── databricks code.py.txt             # PySpark transformation script
├── procedure.txt                      # Step-by-step execution instructions
├── Azure ETL pipeline.png             # Architecture diagram
└── README.md                          # Project documentation

```

---

## 🔥 Project Overview

The goal of this project is to:

1. Extract **denormalized** data from SQL Server (SSMS)
2. Load the data into **Azure Blob Storage** using ADF
3. Transform the data using **Databricks PySpark**  
   - Clean  
   - Normalize  
   - Create **dimension** and **fact** tables (Star Schema)
4. Load the cleaned and modeled data back into SSMS for analytics

This automates ETL using Azure’s modern data engineering tools.

---

## 🧩 Why This Pipeline?

The source table in SSMS was **heavily denormalized**, containing repeated fields, multiple unrelated attributes, and no proper relational structure.

✔ We normalized the data  
✔ Split it into Dimensional & Fact tables  
✔ Applied business logic  
✔ Created a clean Star Schema  
✔ Automated the entire workflow using ADF

This is exactly how Data Engineers transform source OLTP systems into analytic-ready OLAP models.

---

## ⚙️ Step-by-Step Pipeline Breakdown

---

### **1️⃣ Source Data – SSMS (Denormalized)**

The raw source table contains combined attributes like:
- Customer details  
- Product details  
- Sales metrics  
- City, state, region  
- Order info  

This table must be normalized for BI usage.

---

### **2️⃣ Azure Data Factory – Linked Services**

Created Linked Services for:
- **Azure SQL Database (SSMS)**
- **Azure Blob Storage**
- **Azure Databricks**

These secure the connection between Azure services.

---

### **3️⃣ ADF – Copy Activity (SSMS → Blob Storage)**

Extracted data from SSMS and stored it in Blob:

```

/raw/superstore_orders.csv

```

ADF ensures reliable extraction with monitoring & retries.

---

### **4️⃣ Databricks – Transformation (Normalization + Star Schema)**

PySpark code performs:

✔ Cleaning (trim, null handling, data types)  
✔ Removing duplicates  
✔ Creating **Dimension tables**:
- `dim_customer`
- `dim_product`
- `dim_location`

✔ Creating **Fact table**:
- `fact_sales`

✔ Writing transformed tables to:

```

/processed/dim_customer/
/processed/dim_product/
/processed/dim_location/
/processed/fact_sales/

````

> Code used is inside `databricks code.py.txt`

---

### **5️⃣ ADF – Load Transformed Data Back to SSMS**

Another Copy Activity loads the processed Parquet files back to:
- `dbo.dim_customer`
- `dbo.dim_product`
- `dbo.dim_location`
- `dbo.fact_sales`

This completes the ETL cycle:
Blob → SSMS

---

## 📦 Sample Databricks Code (Summary)

```python
df_raw = spark.read.csv(raw_path, header=True, inferSchema=True)

dim_customer = df_raw.select("Customer Name","Customer Segment") \
                     .dropDuplicates() \
                     .withColumn("customer_id", monotonically_increasing_id())

fact_sales = df_raw.join(dim_customer, "Customer Name") \
                   .select("customer_id","Order Date","Sales")
````

Full code is inside `databricks code.py.txt`.

---

## 📊 Final Output – Star Schema

### ✔ Dimension Tables

* dim_customer
* dim_product
* dim_location

### ✔ Fact Table

* fact_sales

This schema is BI-ready for:

* Power BI
* Tableau
* Azure Synapse
* Snowflake migration

---

## 🧪 Testing & Validation

✔ Confirm raw data is extracted to Blob
✔ Validate Databricks transformations
✔ Confirm processed Parquet files in Blob
✔ Test SSMS tables after loading
✔ End-to-end pipeline run in ADF

---

## 🎯 Skills Demonstrated

* Azure Data Factory (Linked Services, Datasets, Pipelines)
* Azure Blob Storage (raw & processed layers)
* Databricks PySpark transformations
* Star Schema modeling (dim & fact tables)
* ADF copy activity (extraction + loading)
* SSMS SQL Server integration
* Enterprise-grade ETL orchestration

This is a complete Azure Data Engineering workflow.

---

## 📄 Resume Bullet Points

* Built an end-to-end ETL pipeline using **Azure Data Factory, Blob Storage, Databricks, and SQL Server** to transform denormalized data into a fully normalized star schema.
* Developed PySpark notebooks to clean, normalize, and model the data into **dimension and fact tables**.
* Orchestrated automated data movement between Azure SQL → Blob → Databricks → SQL using ADF pipelines with Linked Services and Copy Activities.

---

## 👤 Author

**Gnana Prakash N**
Data Engineer
GitHub: [gnanaprakashn](https://github.com/gnanaprakashn)

---

## 📜 License

MIT © 2025


