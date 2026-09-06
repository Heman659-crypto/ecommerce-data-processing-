<div align="center">

# 🛒 Processed E-Commerce Dataset — Day 9 Assignment

**A Pandas + NumPy data engineering pipeline that merges, concatenates, and enriches raw e-commerce data into one clean, analysis-ready dataset.**

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Numeric%20Computing-013243?logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557C)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

</div>

---

## 📌 Overview

Raw e-commerce data is rarely analysis-ready — customer details, product
prices, and order transactions all live in separate files. This project
takes three raw CSV files and builds a **single, clean, enriched dataset**
suitable for reporting, dashboards, or further analysis.

This repository contains the full, reproducible pipeline: the raw inputs,
the fully-executed Jupyter/Colab notebook, the final processed CSV, and the
charts generated along the way.

---

## 🧰 Techniques Demonstrated

| # | Technique | Library | Where it's used |
|---|---|---|---|
| 1 | Loading & inspecting structured data | Pandas | Sections 2–3 |
| 2 | Combining tables with **`merge()`** | Pandas | Section 4 |
| 3 | Stacking DataFrames with **`concat()`** | Pandas | Section 5 |
| 4 | DateTime parsing & feature extraction | Pandas | Section 6 |
| 5 | Row-wise feature engineering with **`apply()`** | Pandas | Section 7 |
| 6 | Vectorized feature engineering (`np.select`, `np.where`) | NumPy | Section 7 |
| 7 | Statistical summaries & outlier detection (IQR method) | NumPy | Section 8 |
| 8 | Final dataset assembly & validation | Pandas | Section 9 |
| 9 | Multi-panel visual dashboard | Matplotlib | Section 10 |
| 10 | Exporting the final dataset | Pandas | Section 11 |

---

## 🗂️ Repository Structure

```
ecommerce-data-processing/
├── data/                                     # Raw input datasets (provided)
│   ├── Day9_Orders.csv
│   ├── Day9_Customers.csv
│   └── Day9_Products.csv
├── notebooks/
│   └── Ecommerce_Data_Processing.ipynb       # Main notebook (fully executed, zero errors)
├── output/
│   └── Processed_Ecommerce_Dataset.csv       # Final processed dataset (deliverable)
├── images/                                   # Charts exported from the notebook
│   ├── summary_dashboard.png
│   └── order_size_and_top_customers.png
├── requirements.txt
├── LICENSE
└── README.md
```

---

## 📥 Input Datasets

| File | Rows | Key Columns |
|---|---|---|
| `Day9_Orders.csv` | 120 | `Order_ID`, `Order_Date`, `Customer_ID`, `Product_ID`, `Quantity`, `Payment_Method`, `Order_Status` |
| `Day9_Customers.csv` | 30 | `Customer_ID`, `Customer_Name`, `City`, `Region`, `Membership_Type` |
| `Day9_Products.csv` | 20 | `Product_ID`, `Product_Name`, `Category`, `Unit_Price`, `Brand` |

## 📤 Output Dataset

`output/Processed_Ecommerce_Dataset.csv` — **120 rows × 26 columns**.
Every order is enriched with full customer and product details, plus these
engineered columns:

| New Column | Description |
|---|---|
| `Order_Year`, `Order_Month_Name`, `Order_Day`, `Order_Weekday`, `Is_Weekend` | Extracted from `Order_Date` |
| `Total_Amount` | `Quantity × Unit_Price` (via `apply()`) |
| `Order_Size` | `Small` / `Medium` / `Large`, bucketed with `np.select` |
| `Customer_Tier_Score` | Numeric loyalty score derived from `Membership_Type` |
| `High_Value_Order` | Flags orders above the 75th percentile of spend (`np.where`) |
| `Is_Outlier` | Statistical outlier flag using the IQR method (`np.percentile`) |
| `Fulfillment_Group` | `Delivered` / `Non-Delivered` (used to demonstrate `concat()`) |

---

## 🔎 Processing Pipeline

1. **Load** the three raw CSVs with `pd.read_csv()`.
2. **Audit** each DataFrame — dtypes, missing values, duplicates, and quick NumPy statistical summaries.
3. **Merge**: `Orders ⨝ Customers` on `Customer_ID`, then `⨝ Products` on `Product_ID` → one enriched row per order.
4. **Split & Concat**: separate orders into `Delivered` vs. `Non-Delivered`, tag each subset, then stack them back together with `pd.concat()`.
5. **DateTime features**: convert `Order_Date` to `datetime64` and extract year, month name, day, weekday name, and a weekend flag.
6. **Feature engineering**: compute `Total_Amount` with `apply()`; bucket `Order_Size` with `np.select`; flag `High_Value_Order` and `Is_Outlier` with `np.where` / IQR bounds from `np.percentile`.
7. **Organize** the final DataFrame into a clean, logically grouped column order.
8. **Validate**: confirm zero missing values and zero duplicate rows in the final dataset.
9. **Visualize**: build a 6-panel Matplotlib dashboard (revenue by category, orders by weekday, payment method share, monthly revenue trend, order-size distribution, top 5 customers).
10. **Export** the result to `output/Processed_Ecommerce_Dataset.csv`.

---

## 📊 Visual Dashboard

**Summary Dashboard** — Revenue by category, orders by weekday, payment method share, and monthly revenue trend:

![Summary Dashboard](images/summary_dashboard.png)

**Order Size Distribution & Top Customers:**

![Order Size and Top Customers](images/order_size_and_top_customers.png)

---

## ▶️ How to Run

### Option A — Google Colab (recommended)
1. Open `notebooks/Ecommerce_Data_Processing.ipynb` in [Google Colab](https://colab.research.google.com/).
2. Run the setup cell — it auto-detects Colab and prompts you to upload the three CSVs (or mount Google Drive).
3. Run all cells (`Runtime → Run all`). The processed CSV will be generated and offered as a direct download.

### Option B — Local Jupyter Notebook
```bash
git clone https://github.com/Heman659-crypto/ecommerce-data-processing.git
cd ecommerce-data-processing

python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt

jupyter notebook notebooks/Ecommerce_Data_Processing.ipynb
```
Run all cells — the CSV files in `data/` are used automatically, and the
processed output is written to `output/Processed_Ecommerce_Dataset.csv`.

---

## 🛠️ Tech Stack

- **Python 3.11**
- **Pandas** — loading, merging, concatenation, DateTime handling
- **NumPy** — vectorized feature engineering, statistics, outlier detection
- **Matplotlib** — multi-panel visual dashboard
- **Jupyter Notebook / Google Colab**

---

## ✅ Quality Checklist

- [x] All three datasets loaded and validated (no missing values, no duplicates)
- [x] `merge()` used to combine Orders, Customers, and Products
- [x] `concat()` used to demonstrate combining DataFrames
- [x] `apply()` used for row-wise feature creation
- [x] DateTime operations used to extract month, day, and weekday
- [x] Final DataFrame organized into a clean, logical structure
- [x] Notebook runs top-to-bottom with **zero errors**
- [x] Final dataset exported as CSV
- [x] Notebook, raw data, and processed CSV all committed to this repository

---

## 📎 Assignment Reference

Completed as part of the **Study Material — Day 9** assignment:
*"Processed E-commerce Dataset"* — practicing `merge()`, `concat()`,
`apply()`, and DateTime operations in Pandas.

---

## 📄 License

Licensed under the [MIT License](LICENSE) — free to use for learning purposes.

---

## 🙋 Author

**Heman: Heman659-Crypto**


