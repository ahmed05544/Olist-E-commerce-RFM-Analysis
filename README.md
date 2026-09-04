<div align="center">

# 📦 Olist Brazilian E-Commerce: Diagnostic Analytics, Supply-Chain Root-Cause & RFM Customer Segmentation
### An End-to-End Strategic Data Architecture & Behavioral Economics Portfolio Project

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg?logo=python&logoColor=white)](#)
[![Pandas](https://img.shields.io/badge/Pandas-2.0+-150458.svg?logo=pandas&logoColor=white)](#)
[![Plotly](https://img.shields.io/badge/Plotly-Interactive%20Data%20Viz-3F4F75.svg?logo=plotly&logoColor=white)](#)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](#)

</div>

---

## 📌 Executive Summary & Key Performance Indicators (KPIs)
Between 2016 and 2018, Olist operated as a decentralized Brazilian marketplace, connecting regional merchants to nationwide consumers. An exhaustive diagnostic analysis of **~100,000 completed orders** reveals key growth bottlenecks:

| Business Metric | Observed Value | Strategic Implication |
| :--- | :--- | :--- |
| **Total Gross Merchandise Value (GMV)** | **~$15.8M** | Heavy geographic concentration in Southeastern Brazil. |
| **SLA Delay Penalty** | **4.3 ★ ➔ 2.6 ★ (-39.5%)** | Logistics failure is the single greatest driver of churn and brand damage. |
| **Primary Delivery Bottleneck** | **Warehouse / Seller Dispatch Lag** | Delays occur *before* carrier transit handoff, depleting buffer times. |
| **Top 2 RFM Churn Risk Cohorts** | **31.4% of Total Revenue** | *"At Risk"* & *"Lost/Hibernating"* hold over $4.9M in historical platform sales. |
| **Diurnal Conversion Cliff** | **Night Drop (23:00 - 05:00)** | Severe conversion drop-off requiring automated batching and off-peak load shifting. |

---

## 🏗️ Technical Architecture & Diagnostic Pipeline
Raw Transactional Schemas (Orders, Items, Sellers, Reviews, Geolocation)
│
▼
Data Validation & Cleansing
(Merged 9 Relational Entities, Null Imputation, Type Casting)
│
▼
Statistical Outlier Architecture (IQR Diagnostic)
[Lower: Q1 - 1.5IQR]  <─── Range ───>  [Upper: Q3 + 1.5IQR]
│
┌─────────────────────────┴─────────────────────────┐
▼                                                   ▼
Raw Truncation (Rejected)                           Log1p Stabilization (Adopted)
• Deletes valid whale purchases                     • Stabilizes heavy-tailed skewness
• Distorts aggregate GMV                            • Preserves parametric integrity
│
▼
Diagnostic Engineering & Temporal Feature Extraction
(Warehouse SLA Lag, Carrier Transit, Delivery Delta, RFM Matrices)
### Statistical Decisions & Mathematical Trade-Offs
1. **IQR Anomaly Flagging:** Calculated dispersion thresholds via $IQR = Q_3 - Q_1$ across heavy-tailed operational variables (Freight Value, Unit Price, Product Weight).
2. **Variance Stabilization via $\log(1 + x)$:** Instead of dropping high-ticket buyers—which would artificially suppress legitimate whale transaction signals—applied natural logarithmic transformation:
   $$y = \ln(1 + x)$$
   This stabilized right-skewed variances while retaining complete customer lifetime value records.

---

## 🔬 Deep-Dive Operational & Behavioral Insights

### 1. Delivery Latency & The Warehouse SLA Bottleneck
Logistics efficiency dictates platform sentiment. While on-time and early deliveries consistently earn stellar marks (**4.2 – 4.3 Stars**), crossing the estimated delivery threshold triggers a ratings collapse to **2.6 Stars**.

![Delivery Latency vs Satisfaction](newplot%20(12).png)

* **Root Cause Decomposition (Warehouse vs. Carrier):** Breaking down cycle times revealed that carriers are frequently blamed for transit delays caused by upstream merchant bottlenecks. Prolonged **Seller Fulfillment Latency** (time from purchase approval to carrier scan) depletes the delivery buffer before the parcel leaves the distribution facility.

![Warehouse Delay Bottleneck](newplot%20(11).png)

---

### 2. Diurnal Demand Patterns & The "Night Drop"
Hourly transaction telemetry reveals sharp behavioral fluctuations:
* **Daytime Purchasing Wave:** Volume expands progressively from 09:00, sustaining peak transaction density between 14:00 and 20:00.
* **The "Night Drop":** Order velocity contracts sharply after 23:00, bottoming out between 02:00 and 05:00.

![Hourly Orders and Night Drop](newplot%20(8).png)

* *Operational Implication:* High-compute batch ingestion, warehouse restocking, and algorithmic seller balance payouts should be scheduled within this off-peak window to eliminate database lock contention.

---

### 3. Macro-Geographic Revenue Distribution
Platform demand exhibits extreme regional clustering:

* **State Dominance:** **São Paulo (SP)** serves as the primary revenue engine, contributing **$5.07M** across **40k+ orders**, dwarfing Rio de Janeiro (**RJ: $1.76M**) and Minas Gerais (**MG: $1.55M**).
* **Urban Concentration:** Order density within secondary states remains almost entirely isolated to state capitals (Curitiba, Porto Alegre, Belo Horizonte).

<div align="center">
<table>
<tr>
<td><img src="newplot%20(13).png" alt="Orders by State" width="450"/></td>
<td><img src="newplot%20(15).png" alt="Revenue by State" width="450"/></td>
</tr>
</table>
</div>

![City Orders Distribution across Top States](newplot%20(14).png)

---

### 4. Merchandise Category Economics & Concentration Risk
Revenue performance is stratified between essential consumer goods and high-margin discretionary items:

* **Volume & Revenue Leaders:** **Health & Beauty** ($1.23M), **Watches & Gifts** ($1.17M), and **Bed, Bath & Table** ($1.02M) form the $1M+ tier.
* **Premium Basket Dominance:** Baskets exceeding high-ticket thresholds are dominated by **Watches & Gifts** ($596k) and **Health & Beauty** ($571k).

<div align="center">
<table>
<tr>
<td><img src="newplot%20(16).png" alt="Top Categories" width="450"/></td>
<td><img src="newplot%20(18).png" alt="Premium Orders" width="450"/></td>
</tr>
</table>
</div>

* **Vendor Dependency:** The marketplace exhibits substantial seller concentration risk; the top two merchants independently generated **$227k** and **$218k**, followed by a steep power-law decay across remaining vendors.

![Top 10 Sellers by Revenue](newplot%20(19).png)

---

### 5. Macro Revenue Trajectory & Seasonality
Revenue trends demonstrate platform maturation from late 2016 through mid-2018:
* **Rapid Scale Phase:** Expanded from <$200k/month in early 2017 to an all-time platform record approaching **$1.0M** during Black Friday 2017.
* **Stabilized Run-Rate:** Rolling average baseline established a consistent operating cadence exceeding **$800k/month** throughout 2018.

![Monthly Revenue vs Rolling Revenue](newplot%20(17).png)

---

## 🎯 Strategic RFM Customer Segmentation & Revenue Matrix

Customers were scored using a quantile-based **Recency, Frequency, and Monetary (RFM)** model calibrated for low-repeat marketplace dynamics.

<div align="center">
<table>
<tr>
<td><img src="newplot%20(20).png" alt="Customer Segment Headcount" width="450"/></td>
<td><img src="newplot%20(21).png" alt="Revenue Contribution by Segment" width="450"/></td>
</tr>
</table>
</div>

### Segment Matrix & Actionable Playbook

| Segment | Headcount | % Revenue | Churn Risk | Strategic Intervention Playbook |
| :--- | :---: | :---: | :---: | :--- |
| **Loyal Customers** | ~15.0k | **16.5%** | Low | VIP loyalty tiers, early access to premium launches, personalized cross-category bundles. |
| **Potential Loyalists** | ~15.0k | **16.0%** | Low-Med | Incentivize 2nd purchase via limited-time coupons; recommend complementary categories. |
| **Lost / Hibernating** | 14.9k | **15.9%** | Critical | **Automated Win-Back Triggers:** Aggressive discount re-engagement campaigns via push/email. |
| **At Risk** | 14.9k | **15.5%** | High | Personalized customer care outreach, feedback collection surveys, targeted price drops. |
| **Champions** | 7.56k | **8.56%** | Minimal | Advocacy referral programs, exclusive subscription perks, direct feedback councils. |
| **Cannot Lose Them** | 7.27k | **8.54%** | Critical | White-glove concierge support, executive re-activation offers, high-margin incentives. |
| **New Customers** | 3.73k | **3.82%** | Medium | Frictionless onboarding sequences, post-purchase tracking updates, educational content. |

---

## 💡 Strategic Roadmap & Business Recommendations

1. **Warehouse Fulfillment SLA Contract (Vendor Penalties):** 
   * *Problem:* Warehouse dispatch lag accounts for the majority of depleted customer delivery buffers.
   * *Execution:* Implement strict 24-48 hour fulfillment SLAs for merchants. Automated penalty points on marketplace search ranking for late-dispatching vendors.
2. **Automated Win-Back Lifecycle Engine:**
   * *Problem:* Inactive cohorts (*Lost / Hibernating* + *At Risk*) represent **31.4% of historical platform GMV**.
   * *Execution:* Deploy automated event-driven marketing triggers at day 45, 90, and 180 of inactivity to recapture high-value churned buyers.
3. **Decentralized Logistics (Micro-Hubs):**
   * *Problem:* Over 60% of volume originates from SP, RJ, and MG, straining single-point delivery lines.
   * *Execution:* Establish micro-fulfillment hubs across the São Paulo metropolitan area to achieve guaranteed same-day/next-day dispatch.
4. **Time-Calibrated Promotional Push:**
   * *Problem:* Order velocity craters during the night window (23:00 - 05:00).
   * *Execution:* Schedule dynamic push notifications between 12:00 and 19:00 when purchase intent and desktop/mobile conversions are optimal.

---

## 🛠️ Technology Stack & Environment Setup

* **Language:** Python 3.9+
* **Data Processing:** `pandas`, `numpy` (Vectorized transformations, quantile partitioning)
* **Visualization Suite:** `plotly.graph_objects`, `plotly.express`, `seaborn`, `matplotlib`
* **Methodologies:** Interquartile Range (IQR) Anomaly Detection, $\log(1+x)$ Variance Stabilization, RFM Behavioral Segmentation

