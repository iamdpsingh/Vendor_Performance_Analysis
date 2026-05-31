# Vendor Performance Analysis - Supporting Documentation

**Repository:** iamdpsingh/Vendor_Performance_Analysis  
**Language Composition:** Jupyter Notebook (99.5%), Python (0.5%)  
**Last Updated:** February 28, 2026  
**Created:** June 13, 2025  

---

## 📋 Executive Summary

This project is a comprehensive **Vendor Performance Analysis** system designed to evaluate vendor efficiency, profitability, and operational metrics. The analysis leverages data analytics techniques to provide insights into vendor performance across multiple dimensions including sales, purchases, inventory, and freight costs.

The project primarily uses Jupyter Notebooks for exploratory data analysis and interactive reporting, with Python scripts handling data ingestion and database operations.

---

## 🎯 Project Objectives

1. **Analyze Vendor Performance Metrics** - Evaluate vendors based on profitability, sales volume, and operational efficiency
2. **Data Integration** - Consolidate data from multiple sources (purchases, sales, inventory, invoices)
3. **Business Intelligence** - Generate insights for vendor management and procurement decisions
4. **Performance Tracking** - Create metrics for ongoing vendor performance monitoring

---

## 📊 Repository Structure

### Main Components

| Component | Type | Purpose |
|-----------|------|---------|
| `Vendor Perfomance Analysis.ipynb` | Notebook | Main analysis notebook with comprehensive vendor metrics |
| `Exploratory Data Analysis-(EDA).ipynb` | Notebook | Exploratory analysis and data discovery |
| `ingestion_db.py` | Script | PostgreSQL data ingestion pipeline |
| `get_vendor_summary.py` | Script | SQL-based vendor summary generation |
| `data/` | Directory | Raw CSV data files (purchases, sales, inventory, invoices) |
| `logs/` | Directory | Application and processing logs |
| `output data/` | Directory | Generated analysis outputs |

### Data Files

```
data/
├── purchases.csv              # Purchase records
├── purchase_prices.csv        # Price catalog
├── sales.csv                  # Sales transactions
├── vendor_invoice.csv         # Vendor invoicing data
├── begin_inventory.csv        # Starting inventory
└── end_inventory.csv          # Ending inventory
```

---

## 🔄 Data Processing Pipeline

### Architecture Overview

```
Raw CSV Data
    ↓
PostgreSQL Database (via ingestion_db.py)
    ↓
SQL Aggregation (get_vendor_summary.py)
    ↓
Data Cleaning & Feature Engineering
    ↓
Analysis & Visualization (Jupyter Notebooks)
    ↓
Business Insights & Reports
```

### Data Ingestion Process

The system uses **PostgreSQL** for robust data management with the following features:

- **Chunked Upload:** Large files processed in 100,000 row chunks
- **COPY Protocol:** Efficient bulk loading using PostgreSQL COPY command
- **Fallback Mechanism:** Automatic fallback to pandas if COPY fails
- **Duplicate Handling:** Tables updated only if data changes
- **Logging:** Comprehensive audit trail of all operations

### Database Tables

| Table | Source | Description |
|-------|--------|-------------|
| `purchases` | purchases.csv | Purchase transactions with vendor and pricing info |
| `purchase_prices` | purchase_prices.csv | Brand-level pricing information |
| `sales` | sales.csv | Sales transactions with revenue data |
| `vendor_invoice` | vendor_invoice.csv | Vendor-level invoicing and freight costs |
| `begin_inventory` | begin_inventory.csv | Opening inventory balances |
| `end_inventory` | end_inventory.csv | Closing inventory balances |
| `vendor_summary` | Generated | Aggregated vendor performance metrics |

---

## 📈 Key Analytics & Metrics

### Vendor Summary Metrics

The analysis generates the following key performance indicators:

#### Financial Metrics
- **Total Purchase Dollars (`TotalPurchaseDollars`)** - Total spending with vendor
- **Total Sales Dollars (`TotalSalesDollars`)** - Revenue from vendor products
- **Gross Profit (`GrossProfit`)** - Sales minus Purchase Cost
- **Profit Margin (`ProfitMargin`)** - Profitability percentage
- **Freight Cost (`FreightCost`)** - Shipping and logistics costs

#### Operational Metrics
- **Purchase Quantity (`TotalPurchaseQuantity`)** - Units purchased
- **Sales Quantity (`TotalSalesQuantity`)** - Units sold
- **Stock Turnover (`StockTurnover`)** - Sales quantity / Purchase quantity
- **Sales to Purchase Ratio (`SalesToPurchaseRatio`)** - Revenue vs. Cost efficiency

#### Product Information
- **Vendor Number & Name** - Vendor identification
- **Brand** - Product brand
- **Description** - Product description
- **Purchase Price** - Cost per unit
- **Actual Price** - Market reference price
- **Volume** - Package volume/size

---

## 🐍 Key Python Functions

### `get_vendor_summary.py`

#### `create_vendor_summary(conn)`
Generates comprehensive vendor summary by:
- Joining purchases, sales, and invoice data
- Aggregating sales quantities and dollars
- Calculating freight costs
- Creating unified vendor view

```
Query Components:
├── FreightSummary: Aggregates freight costs by vendor
├── PurchaseSummary: Consolidates purchase data with pricing
└── SalesSummary: Aggregates sales metrics by vendor and brand
```

#### `clean_data(df)`
Data preprocessing operations:
- Type conversion (Volume → float)
- Missing value handling (fill with 0)
- String cleaning (strip whitespace)
- **Feature Engineering:**
  - Gross Profit = Sales - Purchases
  - Profit Margin = (Gross Profit / Sales) × 100%
  - Stock Turnover = Sales Quantity / Purchase Quantity
  - Sales to Purchase Ratio = Sales / Purchase Dollars

### `ingestion_db.py`

#### `upload_raw_csvs(tables)`
Manages data loading to PostgreSQL:
- Validates file existence
- Checks for existing tables
- Uses COPY for efficiency
- Falls back to chunked pandas upload if needed

#### `ingest_db(df, tables, engine)`
Ingests processed DataFrames into PostgreSQL

#### Performance Characteristics
- **Chunk Size:** 100,000 rows per batch
- **Connection:** SQLAlchemy with psycopg2
- **Logging:** Comprehensive operation logging

---

## 🔧 Setup & Configuration

### Prerequisites

```python
# Required packages
pandas
sqlalchemy
psycopg2
jupyter notebook
```

### Environment Configuration

Create `.env` file with PostgreSQL credentials:

```
DB_USER=your_db_user
DB_PASSWORD=your_password
DB_HOST=localhost
DB_PORT=5432
DB_NAME=vendor_analysis_db
```

### Execution Flow

1. **Data Ingestion**
   ```bash
   python ingestion_db.py  # Load CSVs to PostgreSQL
   ```

2. **Generate Vendor Summary**
   ```bash
   python get_vendor_summary.py  # Create aggregated metrics
   ```

3. **Analysis & Visualization**
   - Open Jupyter Notebooks
   - Run exploratory data analysis
   - Generate visualizations and insights

---

## 📊 Analysis Notebooks

### `Vendor Perfomance Analysis.ipynb`
**Primary Analysis Notebook**
- Comprehensive vendor performance evaluation
- Multi-dimensional analysis across metrics
- Visualizations and dashboards
- Executive summaries

### `Exploratory Data Analysis-(EDA).ipynb`
**Data Exploration & Discovery**
- Data quality assessment
- Distribution analysis
- Correlation studies
- Data profiling and anomaly detection

### `ingestion_db.ipynb`
**Data Loading Pipeline**
- Interactive data ingestion monitoring
- Connection validation
- Data validation checks
- Error handling and recovery

---

## 📝 Data Quality & Validation

### Data Cleaning Operations

1. **Type Conversion:** Volume field converted to float
2. **Missing Values:** Null values replaced with 0
3. **String Normalization:** Whitespace stripped from categorical fields
4. **Outlier Detection:** Visual inspection in notebooks

### Validation Checks

- File existence validation before ingestion
- Database connection verification
- Table schema consistency
- Row count validation

---

## 🚀 Performance Optimization

### Database Optimization
- **Batch Processing:** Chunked uploads prevent memory overflow
- **Bulk Loading:** COPY protocol for efficient PostgreSQL writes
- **Connection Pooling:** SQLAlchemy connection management
- **Indexed Queries:** Vendor number and brand indexed for fast aggregation

### Data Processing
- Vectorized pandas operations for speed
- SQL aggregation at database level
- Selective column loading where applicable

---

## 📋 Logging & Monitoring

### Log Files

| Log File | Purpose |
|----------|---------|
| `logs/ingestion_db.log` | Data loading operations |
| `logs/get_vendor_summary.log` | Summary generation and SQL operations |

### Log Format
```
%(asctime)s - %(levelname)s - %(message)s
Example: 2026-05-31 10:30:45,123 - INFO - Uploading purchases.csv...
```

---

## ⚠️ Known Limitations & Future Work

### Current Limitations
1. **Power BI Dashboard** - Not yet implemented due to resource constraints (mentioned in README)
2. **Real-time Processing** - Batch processing only
3. **Data Refresh** - Manual trigger required

### Future Enhancements
1. **Real-time Dashboard** - Web-based interactive dashboards
2. **Automated Reporting** - Scheduled summary reports
3. **Predictive Analytics** - Vendor performance forecasting
4. **Advanced Visualizations** - Power BI or Tableau integration
5. **API Layer** - REST API for external integrations

---

## 🔗 Data Relationships

### Entity Relationships

```
Vendor (vendor_invoice)
    ├── → Purchases (purchases)
    │   └── → Purchase_Prices (purchase_prices)
    │       └── → Brand
    │
    ├── → Sales (sales)
    │   └── → Brand
    │
    ├── → Begin_Inventory (begin_inventory)
    │   └── → Brand
    │
    └── → End_Inventory (end_inventory)
        └── → Brand
```

### Join Keys
- **Vendor:** `VendorNumber`, `VendorNo`
- **Product:** `Brand`
- **Price:** `PurchasePrice`, `ActualPrice`

---

## 📊 Analysis Use Cases

### 1. Vendor Profitability Analysis
- Identify high-margin vs. low-margin vendors
- Track profit trends over time
- Compare vendor performance

### 2. Inventory Management
- Stock turnover analysis
- Inventory aging
- Demand forecasting

### 3. Procurement Optimization
- Cost analysis by vendor
- Volume discount opportunities
- Supplier consolidation analysis

### 4. Sales Performance
- Product-level performance by vendor
- Sales trend identification
- Revenue attribution

---

## 🔐 Security & Access Control

### Database Security
- PostgreSQL authentication via .env credentials
- Environment variables (not hardcoded credentials)
- Connection pooling for resource management

### File Management
- Git LFS for large files (CSV, PKL, H5, ZIP, PT)
- .gitignore for sensitive files and logs
- Separation of data and code

---

## 📞 Support & Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| Database connection fails | Verify .env credentials and PostgreSQL service |
| Missing data files | Ensure data/ directory contains all required CSVs |
| Chunk upload failures | Check PostgreSQL server resources and available memory |
| Null values in output | Run clean_data() function and verify data sources |

### Debugging
- Check log files in `logs/` directory
- Verify database connection with `psql-connection-check.py`
- Validate file formats before ingestion

---

## 📝 Git Repository Setup

### Large File Tracking (Git LFS)

```bash
# Initialize LFS
git lfs install
git lfs track "*.csv" "*.pkl" "*.h5" "*.zip" "*.pt"

# Commit and push
git add .gitattributes
git commit -m "Setup Git LFS"
git push origin main
```

### Regular Updates

```bash
git checkout main
git pull origin main --rebase
git add .
git commit -m "Updated project files"
git push origin main
```

---

## 📄 Document Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-05-31 | Initial documentation |

---

## 📖 References & Resources

- **PostgreSQL Documentation:** https://www.postgresql.org/docs/
- **SQLAlchemy ORM:** https://docs.sqlalchemy.org/
- **Pandas Documentation:** https://pandas.pydata.org/
- **Jupyter Notebook:** https://jupyter.org/

---

**Document Generated:** May 31, 2026  
**Repository Owner:** iamdpsingh  
**Repository URL:** https://github.com/iamdpsingh/Vendor_Performance_Analysis