# 📦 Olist Brazilian E-Commerce: Comprehensive End-to-End Business Analytics & Customer Segmentation

## Project Overview & Business Strategy
This project provides an end-to-end exploratory and diagnostic data analysis of the Olist e-commerce ecosystem in Brazil, evaluating over 100,000 orders placed between 2016 and 2018. Unlike subscription businesses with continuous re-engagement loops, retail marketplaces like Olist contend with high customer acquisition costs coupled with low organic reorder frequencies. 

Optimizing profitability requires dissecting operational supply-chain bottlenecks, hourly purchasing patterns, geographic concentration, category economics, and segmented customer lifetime value.

---

## Technical Pipeline & Preprocessing

* **Data Cleaning & Consolidation:** Cleaned and merged transactional schemas covering orders, order items, customer demographics, seller information, and product reviews.
* **Timestamp Normalization:** Formatted operational timestamps to establish precise fulfillment intervals: purchase approval, seller-to-carrier handoff (warehouse dispatch), carrier transit duration, and customer receipt.
* **Statistical Outlier Detection (IQR Method):** 
  * Quantified extreme anomalies using the Interquartile Range ($IQR = Q_3 - Q_1$).
  * Flagged upper and lower outliers beyond the bounds $[Q_1 - 1.5 \times IQR, Q_3 + 1.5 \times IQR]$ across heavy-tailed numerical features (such as item prices, freight values, and product weights).
* **Outlier Mitigation & Variance Stabilization:** Rather than trimming or dropping high-value outliers (which would distort legitimate premium revenue data), implemented **Log Transformations** (`np.log1p`) across heavily skewed financial features to stabilize variance, compress skewness, and normalize distributions for modeling.
* **Feature Engineering:** Engineered operational SLA metrics (warehouse fulfillment latency, carrier transit lag, delivery delta against estimated deadline) alongside RFM transaction matrices.

---

## In-Depth Analysis & Visual Insights

### 1. Delivery Latency & The Warehouse Bottleneck Root Cause
Logistics efficiency is the single greatest driver of customer review scores on Olist.

* **Customer Rating Penalty:** Orders delivered on or before the estimated date maintain high satisfaction levels (**4.3** and **4.2** rating points). However, missing the deadline leads to an immediate rating collapse to an average score of **2.6**.

![Delivery Latency vs Satisfaction](newplot%20(12).png)

* **Root Cause: Warehouse Dispatch vs. Carrier Transit:** Decomposing fulfillment cycle times revealed that the primary bottleneck often stems from **seller dispatch and warehouse processing delays** (the gap between order approval and shipping carrier pickup) rather than the logistics carrier in transit. Delays in warehouse handoff consume the customer delivery buffer before transit even starts.

![Warehouse Delay Bottleneck](newplot%20(13).png)

---

### 2. Diurnal Demand Dynamics & The "Night Drop" Phenomenon
Analyzing order volume on an hourly cadence reveals strong diurnal consumer behavior:

* **Daytime Peak Activity:** Purchasing momentum builds steadily throughout business hours, peaking in the afternoon and early evening.
* **The "Night Drop":** Orders experience a sharp downward drop late at night and into the early morning hours, identifying clear operational windows where automated marketing and warehouse batch-processing should be scheduled.

![Hourly Orders and Night Drop](newplot%20(8).png)

---

### 3. Geographic Market Concentration
Both order volume and revenue generation show extreme geographical clustering in southeastern Brazil.

* **Volume Dominance:** São Paulo (SP) generated over 40,000 orders, followed by Rio de Janeiro (RJ) and Minas Gerais (MG).
* **Revenue Disparity:** São Paulo alone generated **$5.07M**, outpacing Rio de Janeiro (**$1.76M**) and Minas Gerais (**$1.55M**). Smaller states contribute under $500k each.

![Total Orders by State](newplot%20(13).png)

![Top 10 States by Revenue](newplot%20(15).png)

* **City-Level Volume Distribution:** Within leading states, orders concentrate heavily in state capitals (e.g., São Paulo city, Rio de Janeiro city, and Belo Horizonte).

![City Orders Distribution across Top States](newplot%20(14).png)

---

### 4. Product Category Dynamics & High-Ticket Orders
Top grossing merchandise categories combine essential repeat consumer staples with high-margin discretionary purchases.

* **Top Revenue Drivers:** **Health & Beauty** leads aggregate sales at **$1.23M**, followed closely by **Watches & Gifts** (**$1.17M**) and **Bed, Bath & Table** (**$1.02M**).
* **Premium Order Drivers:** Examining average high-ticket transactions highlights that **Watches & Gifts** ($596k total premium order volume) and **Health & Beauty** ($571k) capture the majority of high-basket checkouts.

![Top 10 Categories by Revenue](newplot%20(16).png)

![Premium Orders by Category](newplot%20(18).png)

---

### 5. Seller Performance & Concentration Risk
Revenue generation across independent merchants shows high vendor dependence:

* **Top Merchant Inflows:** The leading marketplace seller generated **$227k**, with the second-highest vendor reaching **$218k**.
* **Merchant Tiering:** A sharp revenue drop separates the top 5 merchants from the rest of the cohort, emphasizing the importance of dedicated vendor management for top-tier sellers.

![Top 10 Sellers by Revenue](newplot%20(19).png)

---

### 6. Revenue Trajectory & Seasonality
Analyzing gross sales trajectory from late 2016 through mid-2018 shows exponential early-stage scaling followed by consistent transaction volume.

* **Growth Phase:** Sales expanded rapidly from under $200k monthly run-rate in Q1 2017 to peak monthly revenue approaching **$1.0M** in late 2017 (Black Friday period).
* **Baseline Run-Rate:** The rolling moving average stabilized revenue above **$800k** per month throughout the first half of 2018.

![Monthly Revenue vs Rolling Revenue](newplot%20(17).png)

---

### 7. Comprehensive RFM Customer Segmentation
Customers were evaluated across three foundational metrics:
* **Recency ($R$):** Days since the customer's last purchase relative to the dataset benchmark date.
* **Frequency ($F$):** Total transaction count completed by the customer.
* **Monetary ($M$):** Aggregate net capital spent by the customer.

#### Segment Distribution Analysis
Evaluating segment sizes illustrates the predominantly single-purchase nature of retail marketplace buyers:

* **Potential Loyalists & Loyal Customers:** Comprise ~15.0k customers each.
* **At-Risk & Inactive Cohorts:** **Lost / Hibernating** accounts for 14.9k buyers, and **At Risk** contains 14.9k buyers.
* **Core Active & New Segments:** **Champions** total 7.56k customers, while **New Customers** and **Needs Attention** each stand at approximately 3.7k buyers.

![Customer Segments Distribution](newplot%20(20).png)

#### Revenue Contribution Across Cohorts
Examining the monetary share of each segment yields actionable business insights:

* **Loyal Customers & Potential Loyalists:** Represent the top revenue contributors, driving **16.5%** and **16.0%** of aggregate platform sales respectively.
* **The Reactivation Opportunity:** The **Lost / Hibernating** cohort historically generated **15.9%** of cumulative platform revenue, with the **At Risk** segment accounting for **15.5%**.
* **High-Yield Cohorts:** **Champions** contributed **8.56%**, while **Cannot Lose Them** buyers contributed **8.54%** despite smaller headcounts.

![Revenue Percentage Per Customer Segment](newplot%20(21).png)

---

## Actionable Business Recommendations

* **Warehouse Fulfillment SLAs:** Enforce strict seller fulfillment lead times (dispatch SLA < 24-48 hours) to prevent orders entering transit with depleted buffer time.
* **Time-Targeted Marketing:** Align promotional flash sales and push notifications before the evening **Night Drop** window to maximize daytime conversion rates.
* **Automated Win-Back Triggers:** Launch targeted, automated email re-engagement promotions specifically calibrated for the **At Risk** and **Lost / Hibernating** groups, who represent over 31% of historical cash flows.
* **Southeastern Logistics Hubs:** Invest in micro-fulfillment centers in SP, RJ, and MG to reduce fulfillment overhead and capitalize on high order densities.
* **Cross-Selling Top Categories:** Implement checkout bundling between leading volume products (Bed, Bath & Table) and premium items (Watches & Gifts) to lift Average Order Value (AOV).

---

## Technologies Used
* **Data Manipulation & Analysis:** Python (`pandas`, `numpy`)
* **Visualization Suite:** Plotly Express & Graph Objects, Seaborn, Matplotlib
* **Framework:** RFM Scoring, Outlier Diagnostics via IQR, Log Transforms (`np.log1p`)Objects, Seaborn, Matplotlib
* **Framework:** RFM Scoring, Outlier Diagnostics via IQR, Log Transforms (`np.log1p`)
