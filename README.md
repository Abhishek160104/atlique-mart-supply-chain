# 🚚 Atlique Mart — End-to-End Supply Chain Analytics

An end-to-end supply chain analytics pipeline built using modern AI-powered tools.  
Analyzes order fulfillment performance for **Atlique Mart**, a Gujarat-based FMCG company, across **24,195 order lines** from March–May 2025.

---

## 📊 KPI Results

| KPI | Value | Benchmark |
|-----|-------|-----------|
| Total Order Lines | 24,195 | — |
| Total Orders | 13,467 | — |
| Line Fill Rate | 65.93% | 95–98% |
| Volume Fill Rate | 96.60% | 96–99% |
| On Time Delivery % | 71.21% | 92–97% |
| In Full Delivery % | 65.93% | 95–98% |
| **OTIF %** | **47.84%** | **90–95%** |

> ⚠️ Key Insight: Volume Fill Rate (96.6%) is healthy but OTIF (47.84%) is critically low — indicating a **logistics/delivery timing problem**, not an inventory problem.

---

## 🏗️ Architecture

```
Gmail Inbox (CSV files)
        │
        ▼
   N8N Workflow
   ├── Extract from File (fact_aggregate)  ──► Supabase PostgreSQL
   └── Extract from File (fact_order_line) ──► Supabase PostgreSQL
                                                      │
                                                      ▼
                                              Quadratic AI
                                         (Data cleaning + KPI analysis)
                                                      │
                                                      ▼
                                           KPI Report (Excel)
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **N8N** | Agentic workflow automation — monitors Gmail, extracts CSV, loads to DB |
| **Supabase** | Cloud PostgreSQL database — stores all 6 dimension & fact tables |
| **Quadratic AI** | AI-powered spreadsheet — data cleaning, merging, KPI calculation |
| **Python + Pandas** | Data transformation, validation & Excel report generation |
| **openpyxl** | Formatted KPI Excel report with 4 sheets |

---

## 📁 Project Structure

```
atlique-mart-supply-chain/
│
├── data/
│   ├── dim_customers.csv         # Customer master (35 customers)
│   ├── dim_products.csv          # Product catalog (18 products)
│   ├── dim_date.csv              # Date dimension (Mar–May 2025)
│   ├── fact_order_line.csv       # Order line details (24,195 rows)
│   ├── fact_aggregate.csv        # Daily aggregated metrics
│   ├── fact_targets_orders.csv   # Customer KPI targets
│   └── exchange_rate.csv         # USD to INR rates (Mar 1–May 17 2025)
│
├── scripts/
│   ├── data_pipeline.py          # Load → Clean → Merge → Calculate → Export
│   └── kpi_report.py             # Generate formatted Excel KPI report
│
├── output/
│   ├── fact_orders_summary.csv   # Final merged dataset (24,195 rows)
│   └── supply_chain_kpis.xlsx    # KPI report (4 sheets)
│
├── n8n/
│   └── workflow_screenshot.png   # N8N pipeline screenshot
│
└── README.md
```

---

## 🗄️ Database Schema (Supabase / PostgreSQL)

```sql
-- 6 tables loaded automatically via N8N
dim_customers      -- customer_id, customer_name, city, currency
dim_products       -- product_id, product_name, category, price_INR, price_USD
dim_date           -- date, month_name, week_no, fiscal_year
fact_order_line    -- order_id, order_date, customer_id, product_id, order_qty,
                   -- agreed_delivery_date, actual_delivery_date, delivery_qty,
                   -- In Full, On Time, On Time In Full
fact_aggregate     -- order_date, customer_id, product_id, total_ordered_qty,
                   -- total_delivered_qty, total_order_lines, infull_lines,
                   -- ontime_lines, otif_lines
fact_targets       -- customer_id, product_id, target_line_fill_rate, target_otif
```

---

## ⚙️ How to Run

### 1. Clone the repo
```bash
git clone https://github.com/YOUR_USERNAME/atlique-mart-supply-chain.git
cd atlique-mart-supply-chain
```

### 2. Install dependencies
```bash
pip install pandas openpyxl
```

### 3. Run the data pipeline
```bash
python scripts/data_pipeline.py
```

### 4. Generate KPI report
```bash
python scripts/kpi_report.py
```

Output files will be saved in the `output/` folder.

---

## 📈 KPI Definitions

| KPI | Formula |
|-----|---------|
| **Line Fill Rate** | In-Full Lines / Total Lines × 100 |
| **Volume Fill Rate** | Total Delivered Qty / Total Ordered Qty × 100 |
| **On Time Delivery %** | On-Time Lines / Total Lines × 100 |
| **In Full Delivery %** | In-Full Lines / Total Lines × 100 |
| **OTIF %** | (On-Time AND In-Full Lines) / Total Lines × 100 |

---

## 💡 Key Learnings

1. **AI tools can hallucinate** — always validate SQL JOINs and KPI logic manually
2. **OTIF is the hardest KPI** — requires both on-time AND full quantity simultaneously
3. **N8N automation** saved hours of manual CSV upload work
4. **Domain knowledge is the foundation** — AI accelerates, but you must know what correct looks like

---

## 🔗 Tools & Resources

- [N8N](https://n8n.io/) — Open source workflow automation
- [Supabase](https://supabase.com/) — Open source Firebase alternative with PostgreSQL
- [Quadratic AI](https://www.quadratichq.com/) — AI-powered spreadsheet
- [Supply Chain KPI benchmarks](https://www.gartner.com/en/supply-chain)

---

## 👤 Author

**[Your Name]**  
[LinkedIn](www.linkedin.com/in/abhishekanalyst-g) | [GitHub](https://github.com/Abhishek160104)
