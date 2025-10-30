# AdventureWorks Sales Analysis (PostgreSQL + Jupyter)

Reproducible analysis of customer and sales behavior by product subcategory and sales territory using AdventureWorks on PostgreSQL with a Jupyter Notebook.

## Requirements
- Python 3.9+ (recommended 3.10/3.11)
- PostgreSQL 13+ with `psql` and (optional) `pg_restore`
- Browser for Jupyter

## Environment setup
1) Create and activate a virtual environment (Windows PowerShell):
```
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

2) Install dependencies:
```
pip install -r requirements.txt
```

3) Environment variables
Create a `.env` file at the project root with:
```
PG_USER=postgres
PG_PASSWORD=your_password
PG_HOST=localhost
PG_PORT=5432
PG_DB=AdventureWorks
```
Adjust to your setup.

## Load AdventureWorks data (choose one)

### Option A: install.sql + CSVs (Lorin Thwaits script)
1) Download the "Adventure Works 2014 OLTP Script" (as referenced in `install.sql`).
2) Extract the CSVs and copy them into the same folder as `install.sql`.
3) (If indicated by the script) Adapt CSVs for PostgreSQL:
```
ruby update_csvs.rb
```
4) Create the database and run the script:
```
psql -c "CREATE DATABASE \"AdventureWorks\";"
psql -d AdventureWorks -f install.sql
```
5) Verify table row counts in the notebook.

Notes:
- The script assumes AdventureWorks schemas (Sales, Production, etc.).
- If the DB already exists, you can skip the CREATE DATABASE step.
- Ensure CSVs are in the same directory as `install.sql`.

3) Verify row counts in the notebook.

## Run the analysis
1) Launch Jupyter:
```
jupyter notebook
```
2) Open the notebook:
- `adventureworks_ventas_analisis.ipynb`

3) Execute cells in order:
- Load `.env` and connect
- Verify and count rows
- Build detailed sales table (`sales_df`) computing `line_total = orderqty * unitprice * (1 - unitpricediscount)`
- Aggregations by territory/subcategory, product, orders, and customers
- Central tendency and dispersion KPIs
- Business KPIs (AOV, Top 5 subcategories/products, price variability)
- Visualizations (histogram, boxplot, bar charts, heatmap)
- Insights and recommendations

## Important columns and conventions
- Queries use lowercase schema/table/column names to avoid PostgreSQL case-sensitivity issues.
- `line_total` is computed in SQL; it often does not exist as a physical column.
- Key dataframes in the notebook: `sales_df`, `orders`, `customers`.

## Visualizations generated
- Histogram: total spend per customer.
- Boxplot: `line_total` by subcategory.
- Bar charts: Top 5 subcategories by revenue and Top 5 products by revenue/quantity.
- Heatmap: revenue by territory and subcategory.

## KPIs computed
- Mean, median, mode, variance, standard deviation, IQR of spend per order/customer.
- AOV per order and per customer.
- Top 5 subcategories by revenue.
- Top 5 products by quantity and revenue.
- Product with highest `unitprice` variability.

## Project structure
- `.env` PostgreSQL credentials.
- `requirements.txt` Python dependencies.
- `install.sql` AdventureWorks creation/loading script (via CSVs).
- `adventureworks_ventas_analisis.ipynb` main analysis notebook.

