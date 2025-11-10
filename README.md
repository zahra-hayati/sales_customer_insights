# 🛒 Green Cart Ltd. Q2 Sales & Behaviour Analysis — Strategic Review

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data_Analysis-green)
![Status](https://img.shields.io/badge/Project%20Status-Completed-brightgreen)
![License](https://img.shields.io/badge/License-MIT-blue)

---

## 📝 1. Project Brief: Business Problem & Goal

**Green Cart Ltd.** aims to optimize its operational efficiency, marketing spend, and product inventory based on Q2 performance data. The core **business problem** is the lack of a cohesive view connecting customer segments, product performance, and logistical bottlenecks (delays) to revenue.

The **goal** of this project is to merge and analyze sales, customer, and product datasets to provide **data-driven recommendations** on discount strategy, product inventory management (identifying underperformers), and delivery logistics for strategic planning.

---

## 📊 2. Dataset Overview

Three mock datasets were merged and analyzed, covering Q2 activity:

| Dataset       | Size         | Key Fields                                                            | Source / Note                        |
| :------------ | :----------- | :-------------------------------------------------------------------- | :----------------------------------- |
| **Sales**     | ~250 records | `order_id`, `product_id`, `quantity`, `unit_price`, `delivery_status` | Transactional data (lines per order) |
| **Products**  | ~150 records | `product_id`, `product_name`, `category`, `base_price`                | Product metadata                     |
| **Customers** | ~100 records | `customer_id`, `signup_date`, `loyalty_tier`, `region`                | Customer profile data                |

---

## ⚙️ 3. Methods & Analysis Depth

### Data Cleaning Rules

- **Text/Categorical Standardization:** Cleaned and standardized text fields (e.g., `delivery_status` to 'Delivered'/'Delayed', `region` to Title Case, `loyalty_tier` to 'Gold'/'Silver'/'Bronze').
- **Numeric Correction:** Handled non-numeric entries in `quantity` (e.g., 'three' replaced with 3) and imputed missing `quantity` with 1 and missing `discount_applied` with 0.
- **Imputation & Inferring:** Imputed missing `loyalty_tier` based on calculated **Total Spent** thresholds (£500 Bronze, £2000 Silver, £5000 Gold) and filled missing dates with the median date.

### Feature Engineering

Key features derived for deeper analysis:

- **`revenue`:** Calculated as $Quantity \times Price \times (1 - Discount)$.
- **`price_band`:** Categorized products into Low, Medium, and High price tiers.
- **`days_to_order`:** Days from `launch_date` to `order_date` (adoption speed).

### Analysis Techniques (Depth)

- **Segmentation:** Segmented revenue and orders by **Loyalty Tier**, **Region**, and **Price Band**.
- **Retention Proxy:** Calculated the **Q2 Early Order Rate** (customers who ordered within 14 days of sign-up).
- **Underperformance Score:** Calculated a three-factor score to identify inventory risk (low quantity, high discount, high delay rate).
- **Normalization:** Applied **MinMaxScaler** to revenue and price for comparative distribution analysis.

---

## 🚀 4. Headline Insights & Key Metrics

- **Top Category Dominance:** The **Cleaning** category generated the highest revenue at **£93,754**, significantly outperforming the next highest (Storage: £47,081).
- **Discount Ineffectiveness:** The correlation between `discount_applied` and `quantity` is near zero **(-0.007)**, indicating deep discounts are not driving purchase volume.
- **Logistical Risk in High Value:** Orders in the **Medium/High Price Bands** experience a disproportionately high percentage of **Delayed** deliveries (e.g., **~35%** for High-Price orders).
- **Underperforming Inventory:** **5** specific products were flagged as **underperformers** (score $\ge 2$) due to low quantity sold, high discounts, and high delay rates, posing a potential inventory risk.
- **Early Customer Engagement:** For Q2 signups, **65%** of customers placed an order within the first 14 days, highlighting a strong initial activation period.

---

## 💡 5. Business Impact & Recommended Actions

| Action                           | Insight Driving Action                                           | Expected Uplift / Value                                                                                                              |
| :------------------------------- | :--------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| **Redeploy Discount Budget**     | Discounts (avg. 10%) do not increase quantity sold.              | **15% reduction** in total discount spending, reallocated to value-add bundles or free premium shipping.                             |
| **Optimize High-Value Delivery** | Delays are concentrated in Medium/High price orders.             | **20% decrease** in high-value order delay rates by auditing the East region's logistics partner.                                    |
| **Review Underperforming SKUs**  | 5 products scored high on the Underperformance Score.            | **Release up to 10%** of tied-up capital by de-stocking, heavily promoting, or bundling underperforming products.                    |
| **Enhance Gold Tier Experience** | Gold tier customers drive the highest average revenue per order. | **3% increase** in Gold tier retention by offering early access to new products (e.g., from the top-performing 'Cleaning' category). |

---

## 🖼️ 6. Presentation: Key Visuals for Stakeholders

<p align="center">
  <img src="reports/figures/01_weekly_revenue_trends.png" width="30%" style="margin-right:10px;" alt="Line plot of weekly revenue trends by region"/>
  <img src="reports/figures/02_top_categories.png" width="30%" style="margin-right:10px;" alt="Bar chart of top 5 product categories by total revenue"/>
  <img src="reports/figures/03_delivery_status_price_band.png" width="30%" alt="Stacked bar chart of delivery status by product price band"/>
</p>

---

## 🛠️ 7. Quick Start / Run Steps

This project is fully reproducible using the provided environment files and running the single notebook in sequence.

1.  **Clone the repository and move into the directory:**
    ```bash
    git clone https://github.com/zahra-hayati/sales_customer_insights.git
    cd sales_customer_insights/
    ```
2.  **Create and activate the environment (Conda option):**
    ```bash
    conda env create -f environment.yml
    conda activate rapid_scale_analysis
    ```
    _(Alternatively, use `pip install -r requirements.txt` if using a `venv`)_
3.  **Run the analysis:**
    - Open and run the notebook: `jupyter notebook notebooks/01_sales_analysis.ipynb`

---

## 📁 8. Reproducibility & Project Structure

```
sales_customer_insights/
├── data/                    # Source data (sales_data.csv, product_info.csv, customer_info.csv)
├── notebooks/
│ └── 01_analysis_report.ipynb # Full cleaning, EDA, feature engineering, and plot generation
├── reports/
│ ├── figures/               # Key plots saved here (acquisition_sources.png, plan_selection_by_age.png, etc.)
│ └── analysis_report.pdf    # Stakeholder-ready PDF summary
├── .gitignore               # Ensures data/ and environment files are not tracked
├── requirements.txt         # All Python library dependencies
├── environment.yml          # Environment file for easy setup
└── README.md                # This report
```

---

### Known Limitations/Assumptions

- **Data Size:** The analysis is based on a limited mock dataset (Q2 only, ~250 orders), which limits statistical confidence in some rates.
- **Imputation Bias:** The loyalty tier was imputed based on calculated total spend, which may slightly bias results towards the segmentation thresholds defined.
- **Causation:** Correlation between discount and quantity (or lack thereof) does not prove causation; A/B testing is required to confirm the discount strategy recommendation.

---

## 👩‍💻 Contact

**Author:** Zahra Hayati  
**Project:** Green Cart Ltd. Q2 Sales & Behaviour Analysis — Strategic Review
**Email:** zahrahyt.7@gmail.com  
**LinkedIn:** [linkedin.com/in/zahra-hayati-data-science](https://www.linkedin.com/in/zahra-hayati-data-science)  
**GitHub:** [github.com/zahra-hayati](https://github.com/zahra-hayati)
