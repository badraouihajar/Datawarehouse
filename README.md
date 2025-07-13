# Data Warehouse Design for Technical Service Sales Analysis

This project focuses on designing and implementing a data warehouse for a fictional company that provides technical services to individuals. The approach is based on a **star schema** and leverages **Apache Hop** for ETL orchestration and historical data tracking.

## ⭐ Star Schema Overview

The warehouse is structured around a central fact table and several dimension tables:

### 📊 Fact Table: `fact_sales`

Contains measurable information about service sales:
- `sk_ventes`: surrogate key (primary key)
- `date_insertion`: date of sale
- `prix`: amount of the service
- `code_prestation`: foreign key to the service dimension
- `num_client`: foreign key to the customer dimension
- `duree`: duration of intervention
- `adresse`, `heure_début`, `heure_fin`

### 🧭 Dimension Tables

- **Date Dimension (`dates`)**  
  Attributes: year, quarter, month, day, weekday, hour  
  → Enables time-based analysis (e.g. "Which months generate the most revenue?").

- **Geography Dimension (`geographie`)**  
  Attributes: region, department, INSEE code, etc.  
  → Enables spatial analysis (e.g. "Which department has the highest sales?").

- **Customer Dimension (`clients_entrepot`)**  
  Attributes: name, address, city, `debut_validite`, `fin_validite`  
  → Tracks customer data over time with slowly changing dimensions (SCD Type 2).

- **Service Dimension (`prestations_entrepot`)**  
  Attributes: service code, name, category (e.g. Repair, Installation)  
  → Enables service performance analysis (e.g. "Which service category is most profitable?").

## 🔑 Business Keys vs Surrogate Keys

| Key Type        | Description                                                                 |
|-----------------|-----------------------------------------------------------------------------|
| Business Keys   | Natural identifiers from the source system (e.g. `num_client`, `code_prestation`) |
| Surrogate Keys  | Artificially generated unique keys for consistency and tracking changes     |

Examples:
- `sk_prestations`: replaces `code_prestation` in the DW
- `sk_ventes`: uniquely identifies each sale in the fact table
- `num_client`, `id_geo`, `datetime`: used as SKs since they already meet uniqueness requirements

## ⚙️ ETL Implementation with Apache Hop

- Pipelines to extract data from relational sources (CSV, SQL)
- Dimension loading with surrogate key generation
- SCD management with validity periods (`debut_validite`, `fin_validite`)
- Fact table population with joins to dimension SKs
- Error handling and logging

## 🧠 Business Questions Answered

- What are the most profitable service categories?
- How does sales performance vary by region or department?
- Which days of the week generate the most revenue?
- What is the evolution of customer activity over time?

## 👩‍💻 Author

Hajar Badraoui  
[GitHub Profile](https://github.com/Hajar-badr)

*Project developed in 2025 as part of a training in data warehousing and business intelligence.*
