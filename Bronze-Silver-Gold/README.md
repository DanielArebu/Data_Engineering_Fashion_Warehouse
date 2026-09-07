# Fashion Warehouse Data Engineering Project

## Bronze-Silver-Gold Architecture Implementation

This project implements a complete **Medallion Architecture** (Bronze-Silver-Gold) data lakehouse for a fashion warehouse management system using Databricks and Delta Lake.

## 📋 Project Overview

The Fashion Warehouse data pipeline processes operational data from multiple sources (Google Drive CSV files) and transforms it through three layers:

- **Bronze Layer**: Raw data ingestion and storage
- **Silver Layer**: Cleaned, validated, and conformed data
- **Gold Layer**: Business-level aggregates and analytics-ready datasets

## 🏗️ Architecture

```
Google Drive CSVs
       ↓
  Bronze Layer (Raw Data)
       ↓
  Silver Layer (Cleaned & Validated)
       ↓
  Gold Layer (Business Analytics)
```

## 📊 Data Model

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

## 🔑 Key Features

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

### Analytics Features
- **Profit Analysis**: Order value, cost, profit amount, and margin percentage
- **Delivery Performance**: Delay tracking, fill rates, delivery status
- **Inventory Management**: Stock movement tracking with employee attribution
- **Time Intelligence**: Complete date dimension for time-series analysis

## 🛠️ Technologies Used

- **Platform**: Databricks
- **Storage**: Delta Lake
- **Compute**: Serverless (CPU)
- **Languages**: SQL, Python (PySpark)
- **Source**: Google Drive CSV files

## 📁 Project Structure

```
Bronze-Silver-Gold/
├── Fashion_Warehouse.ipynb    # Main ETL pipeline notebook
└── README.md                   # This file
```

## 🚀 Getting Started

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

Each CREATE/INSERT operation is followed by a SELECT LIMIT 5 preview for verification.

## 📈 Business Metrics Available

### Sales & Profitability
- Order value and profit margins
- Product performance by SKU
- Customer lifetime value

### Supply Chain
- Delivery accuracy (fully/partially/over received)
- Supplier performance (lead times, fill rates)
- Delivery delays tracking

### Warehouse Operations
- Stock movement patterns
- Scanner event analysis
- Warehouse capacity utilization
- Employee productivity

## 🔍 Sample Queries

### Top Products by Profit Margin
```sql
SELECT 
    p.product,
    AVG(o.profit_margin_pct) as avg_margin
FROM fashion_warehouse.gold.fact_orders o
JOIN fashion_warehouse.gold.dim_products p ON o.sku = p.sku
GROUP BY p.product
ORDER BY avg_margin DESC
LIMIT 10;
```

### Delivery Performance by Supplier
```sql
SELECT 
    d.supplier_name,
    COUNT(*) as total_deliveries,
    AVG(d.fill_rate_pct) as avg_fill_rate,
    SUM(CASE WHEN d.is_delayed THEN 1 ELSE 0 END) as delayed_count
FROM fashion_warehouse.gold.fact_deliveries d
GROUP BY d.supplier_name
ORDER BY avg_fill_rate DESC;
```

## 📝 Data Lineage

```
Google Drive CSV
    ↓
Bronze (Raw)
    ↓
Silver (Cleaned)
    ↓ (Joins & Transformations)
Gold (Analytics)
```

## 🔐 Data Governance

- All tables use **Delta Lake** format for ACID transactions
- Primary and foreign key constraints enforced
- Unity Catalog for centralized access control
- Schema evolution supported through Delta Lake

## 📊 Performance Optimizations

- Delta Lake format for efficient reads/writes
- Partitioning ready (can be added to fact tables by date)
- Serverless compute for auto-scaling
- Query result caching through Delta Lake

## 🧪 Testing & Validation

Each layer includes preview queries (LIMIT 5) to validate:
- Schema correctness
- Data quality
- Transformation accuracy
- Referential integrity

## 🚧 Future Enhancements

- [ ] Add incremental data loading (MERGE statements)
- [ ] Implement SCD Type 2 for dimension tracking
- [ ] Add data quality checks with Great Expectations
- [ ] Create monitoring dashboard
- [ ] Implement automated testing
- [ ] Add CI/CD pipeline
- [ ] Partition large fact tables by date
- [ ] Add data lineage visualization

## 👥 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Contact

For questions or feedback, please open an issue in the repository.

## 📄 License

This project is licensed under the MIT License.

---

**Built with ❤️ using Databricks and Delta Lake**