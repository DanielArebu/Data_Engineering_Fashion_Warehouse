# Fashion Warehouse Data Engineering Project

## Bronze-Silver-Gold Architecture Implementation

This project implements a complete **Medallion Architecture** (Bronze-Silver-Gold) data lakehouse for a fashion warehouse management system using Databricks and Delta Lake.

## Project Overview

The Fashion Warehouse data pipeline processes operational data from multiple sources (Google Drive CSV files) and transforms it through three layers:

- **Bronze Layer**: Raw data ingestion and storage
- **Silver Layer**: Cleaned, validated, and conformed data
- **Gold Layer**: Business-level aggregates and analytics-ready datasets

## Architecture

```
Google Drive CSVs
       ↓
  Bronze Layer (Raw Data)
       ↓
  Silver Layer (Cleaned & Validated)
       ↓
  Gold Layer (Business Analytics)
```

##  Data Model

### Bronze Layer Tables
- `warehouses` - Warehouse locations and capacity data
- `suppliers` - Supplier information
- `products` - Product catalog
- `employees` - Employee records
- `customers` - Customer information
- `orders` - Order transactions
- `deliveries` - Delivery records
- `scanner_events` - Warehouse scanner activity
- `stock_movements` - Inventory movement tracking

### Silver Layer Tables

Cleaned and validated versions of bronze tables with:
- Data type casting and standardization
- Duplicate removal
- Foreign key relationships established
- Data quality validations

### Gold Layer Tables

#### Dimension Tables
- `dim_products` - Product master with supplier information
- `dim_customers` - Customer master with derived attributes
- `dim_warehouses` - Warehouse master
- `dim_employees` - Employee master with tenure calculations
- `dim_suppliers` - Supplier master
- `dim_date` - Date dimension (2020-2030)

#### Fact Tables
- `fact_orders` - Order transactions with profit calculations
- `fact_deliveries` - Delivery performance metrics
- `fact_stock_movements` - Inventory movement events
- `fact_scanner_events` - Scanner activity events

##  Key Features

### Data Ingestion
- Direct ingestion from Google Drive using Databricks connectors
- Automatic schema inference
- Metadata tracking with file paths

### Data Quality
- Primary key constraints
- Foreign key relationships
- Duplicate detection and removal
- Data type validation and casting

### Business Logic
- Profit margin calculations
- Delivery status tracking (Fully Received, Partial, Over Received)
- Fill rate percentages
- Employee tenure calculations
- Date dimension with business flags (weekends, month/quarter/year starts/ends)


## 🛠️Technologies Used

- **Platform**: Azure Databricks 
- **Storage**: Delta Lake
- **Languages**: SQL, Python (PySpark)
- **Source**: Google Drive CSV files (files attached to document)

## 📁 Project Structure

```
Bronze-Silver-Gold/
├── Fashion_Warehouse.ipynb    # Main ETL pipeline notebook
├── Buesiness_Logic.ipynb      # Incoming 
└── README.md                   # This file
```

##  Getting Started

### Prerequisites

1. Databricks workspace
2. Google Drive connection configured
3. Unity Catalog enabled
4. Catalog: `fashion_warehouse`

### Running the Pipeline

1. **Setup**: The notebook creates the catalog and schemas automatically
2. **Bronze Layer**: Loads CSV files from Google Drive into bronze tables
3. **Silver Layer**: Applies transformations and data quality rules
4. **Gold Layer**: Creates dimensional model for analytics

### Notebook Execution

The notebook is organized into sections:

1. **Catalog & Schema Creation**
2. **Bronze Layer Ingestion** (Cells 2-4)
3. **Silver Layer Transformation** (Cells 5-34)
4. **Gold Layer Creation** (Cells 35-64)


## Data Lineage

```
Google Drive CSV
    ↓
Bronze (Raw)
    ↓
Silver (Cleaned)
    ↓ (Joins & Transformations)
Gold (Analytics)
```

##  Data Governance

- All tables use **Delta Lake** format for ACID transactions
- Primary and foreign key constraints enforced
- Unity Catalog for centralized access control
- Schema evolution supported through Delta Lake

## Performance Optimizations

- Delta Lake format for efficient reads/writes
- Partitioning ready (can be added to fact tables by date)
- Serverless compute for auto-scaling
- Query result caching through Delta Lake

##  Testing & Validation

Each layer includes preview queries (LIMIT 5) to validate:
- Schema correctness
- Data quality
- Transformation accuracy
- Referential integrity


## 👥 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Contact

For questions or feedback, please open an issue in the repository.

## 📄 License

This project is licensed under the MIT License.

---

**Built with ❤️ using Databricks and Delta Lake**
