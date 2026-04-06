# 🛒 Store Data Cleaning

A Python/Pandas data cleaning and enrichment pipeline for retail store sales data.

## 📋 Overview

This notebook processes raw store sales data (`store.csv`) through a series of cleaning, transformation, and aggregation steps, producing a clean, analysis-ready dataset (`store_new.csv`).

## 📁 Files

| File | Description |
|------|-------------|
| `store.csv` | Raw input data |
| `Store_Data_Cleaning.ipynb` | Main Jupyter notebook |
| `store_new.csv` | Cleaned output data |

## 🔧 Requirements

```bash
pip install pandas
```

## 🚀 Usage

```bash
jupyter notebook Store_Data_Cleaning.ipynb
```

Make sure `store.csv` is in the same directory as the notebook before running.

## 🔄 Pipeline Steps

### 1. Data Import
Loads `store.csv` into a Pandas DataFrame.

### 2. Anomaly Detection (IQR Method)
Detects outliers in the `Sales` column using the Interquartile Range (IQR) method — flags values above `Q3 + 1.5 × IQR`. Reports the count of anomalies and the top 3 most affected categories.

### 3. Data Cleaning
- Drops rows with missing `Postal Code`
- Removes duplicate rows

### 4. Date Formatting
Parses `Order Date` and `Ship Date` columns into proper datetime objects (`DD/MM/YYYY` format).

### 5. Temporal Feature Extraction
Derives new columns from `Order Date`:

| Column | Description |
|--------|-------------|
| `years` | Year of order |
| `quarter` | Quarter (1–4) |
| `months` | Month number |

### 6. Financial Calculations
| Column | Formula |
|--------|---------|
| `cost` | `Sales × 0.60` |
| `profit` | `Sales − cost` |
| `ratio_profit` | `(profit / Sales) × 100` |

### 7. Delivery Time (Logistics)
Computes `delivery_days` as the difference between `Ship Date` and `Order Date`.

### 8. Customer Segmentation
Classifies each order by sales value:

| Segment | Condition |
|---------|-----------|
| `high value` | Sales > $500 |
| `medium value` | $250 < Sales ≤ $500 |
| `low value` | Sales ≤ $250 |

### 9. KPI Aggregations
- **Revenue by Region** — total sales per region, sorted descending
- **Revenue by Product** — total sales per `Product ID` / `Product Name`, sorted descending

### 10. Export
Saves the enriched DataFrame to `store_new.csv` (without the index).

## 📊 Output Columns (store_new.csv)

All original columns, plus:
`years`, `quarter`, `months`, `cost`, `profit`, `ratio_profit`, `delivery_days`, `categorise`
