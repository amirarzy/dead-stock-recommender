# Recommender System for Dead Stock Inventory Based on Product Grouping

This project builds a recommendation engine to **promote slow‑moving / dead‑stock inventory items** by recommending them together with better‑selling products from similar product groups.

The goal is to help retail / distribution companies reduce dead stock and improve inventory turnover using data‑driven recommendations.

---

## 📊 Problem & Data

- Business problem: identify **dead‑stock items** (slow‑moving products stuck in the warehouse) and recommend them alongside normal items so they can be sold.  
- Input data (structure can be adapted per company):
  - Product master data: product ID, name, category / sub‑category / brand.
  - Inventory / sales data: stock levels, sales quantities over time.
  - Product grouping information (e.g. category trees, manual groups, or embeddings).

From these sources we derive:
- A list of **dead‑stock items** (low or zero sales but positive stock).  
- A list of **active items** to be used as “carrier products” in recommendations.

---

## 🧩 Feature Engineering & Product Grouping

Key steps:

- Calculated **dead‑stock flag** based on business rules (e.g. no sales in last N months but on‑hand stock > 0).  
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
   - Filter products with dead‑stock flag = 1.  
   - Restrict candidates to the **same or related product groups** as the anchor product.

2. **Rank and recommend**  
   - Rank candidates using a scoring function that can combine:
     - Similarity in category / sub‑category / brand.  
     - Inventory pressure (higher stock, longer age).  
     - Optional historical co‑sales signals.  
   - Return Top‑K dead‑stock items as recommendations for each active item.

The logic is implemented in the notebook
