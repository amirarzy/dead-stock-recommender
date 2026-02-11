# Recommender System for Dead Stock Inventory Based on Product Grouping

This project builds a recommendation engine to **promote slow‑moving / dead‑stock inventory items** by recommending them together with better‑selling products from similar product groups.

The goal is to help retail / distribution companies reduce dead stock and improve inventory turnover using data‑driven recommendations.

---

## 📊 Problem & Data

- Business problem: identify **dead‑stock items** (slow‑moving products stuck in the warehouse) and recommend them alongside normal items so they can be sold.  
- Input data (structure can be adapted per company):
  - Product master data: product ID, name, category / sub‑category / brand.  
  - Inventory / sales data: stock levels, sales quantities over time.  
  - Product grouping information (e.g. category trees, manual groups, or similarity rules).

From these sources we derive:
- A list of **dead‑stock items** (low or zero sales but positive stock).  
- A list of **active items** to be used as “carrier products” in recommendations.

---

## 🧩 Feature Engineering & Product Grouping

Key steps:

- Calculated a **dead‑stock flag** based on business rules (for example: no sales in the last N months while on‑hand stock > 0).  
- Built **product groups** using category hierarchy (department → category → sub‑category) and/or similarity measures between products.  
- For each product, engineered features such as:
  - Category and sub‑category codes.  
  - Brand or supplier.  
  - Historical sales velocity / stock coverage.

Product grouping allows the recommender to suggest dead‑stock items that are **similar and relevant** to the active item being sold.

---

## 🤖 Recommendation Logic

The recommender works in two main steps:

1. **Identify candidate dead‑stock items**  
   - Filter products with `dead_stock_flag = 1`.  
   - Restrict candidates to the **same or related product groups** as the anchor (active) product.

2. **Rank and recommend**  
   - Rank candidates using a scoring function that can combine:
     - Similarity in category / sub‑category / brand.  
     - Inventory pressure (higher stock, longer age).  
     - Optional historical co‑sales signals.  
   - Return Top‑K dead‑stock items as recommendations for each active item.

The logic is implemented in the notebook `recommend-system-final-v2.ipynb` using Python and `pandas`.

---

## 📦 Outputs

Typical outputs of the pipeline:

- A **recommendation table** with columns like:  
  `anchor_product_id`, `recommended_product_id`, `score`, `group_id`, `is_dead_stock`.  
- Aggregated views by product group or supplier to help category managers see which dead‑stock items are most recommended.

These outputs can be fed into:
- POS systems (cross‑sell pop‑ups).  
- BI dashboards (Power BI / Qlik).  
- Campaign / promotion engines.

---

## 🚀 How to Run (outline)

1. Prepare input data files (product master, inventory/sales, product groups) and adjust the paths in the notebook.  
2. Install basic dependencies:
  pip install pandas numpy scikit-learn
3. Open `recommend-system-final-v2.ipynb` in Jupyter / Colab and run all cells to:
- load and clean data,  
- mark dead‑stock items,  
- build product groups,  
- generate the recommendation table. 

---

## 💡 Possible Extensions

- Add **collaborative filtering** or association‑rule mining (market‑basket analysis) on transaction data.  
- Deploy the recommender behind an API or integrate it directly with a BI dashboard.  
- Experiment with embedding‑based similarity (e.g. product2vec) for more nuanced recommendations. [web:131]
