# 🛍️ Customer Segmentation Analysis
### RFM-Based Clustering & Power BI Dashboard | UK Online Retail Dataset

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat&logo=python&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=flat&logo=powerbi&logoColor=black)
![scikit-learn](https://img.shields.io/badge/scikit--learn-K--Means-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat)

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Problem Statement](#-problem-statement)
- [Dataset](#-dataset)
- [Methodology](#-methodology)
- [Customer Segments](#-customer-segments)
- [Dashboard](#-dashboard)
- [Key Findings](#-key-findings)
- [Project Structure](#-project-structure)
- [Installation & Setup](#-installation--setup)
- [Results](#-results)
- [Technologies Used](#-technologies-used)
- [Contributing](#-contributing)

---

## 🔍 Overview

This project applies the **RFM (Recency, Frequency, Monetary)** framework to a real-world UK online retail dataset to identify distinct customer segments using **K-Means clustering**. The results are visualised in an interactive **Power BI dashboard** designed for business stakeholders.

The analysis covers **8,000+ customers** across transactions from **December 2010 to December 2011**, generating actionable segment-level insights and strategic recommendations.

| Metric | Value |
|---|---|
| Total Customers | 8,000 |
| Total Revenue | £7.29 Million |
| Avg. Customer Frequency | 89.08 orders |
| Avg. Customer Monetary Value | £1,860 |
| Avg. Customer Recency | 91.75 days |
| Number of Segments | 3 |

---

## ❓ Problem Statement

> *Identify major customer segments from a transactional dataset containing all transactions occurring between 01/12/2010 and 09/12/2011 for a UK-based non-store online retail company that mainly sells unique all-occasion gifts.*

Without a systematic segmentation framework, critical business signals remain hidden in raw transactional data:
- **91.62%** of customers are one-time buyers
- The **At Risk** segment generates the highest revenue (£4.9M) but is at high churn risk
- The **Loyal** segment contributes only 5.4% of total revenue despite being the most valuable

---

## 📦 Dataset

**Source:** [UCI Machine Learning Repository — Online Retail Dataset](https://archive.ics.uci.edu/ml/datasets/online+retail)

**Direct Link:** [Google Drive](https://drive.google.com/file/d/1nCwyLb5mTuouiigvi6sbNBT91W3MLUtF/view?usp=sharing)

| Field | Type | Description |
|---|---|---|
| `InvoiceNo` | String | Unique invoice ID; 'C' prefix = cancellation |
| `StockCode` | String | Unique product code |
| `Description` | String | Product name |
| `Quantity` | Integer | Units per transaction |
| `InvoiceDate` | DateTime | Transaction date and time |
| `UnitPrice` | Float | Price per unit (GBP £) |
| `CustomerID` | Float | Unique customer identifier |
| `Country` | String | Customer's country |

**Scope:** Analysis is filtered to `uk_data_only` (~495,000 UK transactions).

---

## 🔬 Methodology

### 1. Data Preprocessing
- Removed records with missing `CustomerID` (~135,080 rows)
- Filtered out cancelled invoices (prefix `C`)
- Removed negative quantities and zero/negative unit prices
- De-duplicated records
- Created `TotalPrice = Quantity × UnitPrice`

### 2. RFM Feature Engineering
```python
reference_date = datetime(2011, 12, 31)

rfm = df.groupby('CustomerID').agg(
    Recency=('InvoiceDate', lambda x: (reference_date - x.max()).days),
    Frequency=('InvoiceNo', 'nunique'),
    Monetary=('TotalPrice', 'sum')
).reset_index()
```

### 3. Normalisation
Features were **log-transformed** and **standardised (z-score)** to remove scale bias before clustering.

### 4. K-Means Clustering
- Optimal K selected using the **Elbow Method** and **Silhouette Score**
- K = **3** identified as optimal (Silhouette Score: 0.42)
- Initialisation: **K-Means++** with 10 runs, 300 max iterations

### 5. Visualisation
- Interactive Power BI dashboard with cross-filtering slicers
- DAX measures for KPI cards and calculated metrics

---

## 👥 Customer Segments

### Cluster 0 — 🔴 At-Risk / Lapsed (35.6%)
| Metric | Value |
|---|---|
| Avg. Recency | ~165 days |
| Avg. Frequency | ~15 transactions |
| Avg. Monetary | ~$286 |
| Customer Count | 2,847 |

Former occasional shoppers who have not returned recently. **Priority: Re-engagement campaigns.**

---

### Cluster 1 — 🟢 Champions / Loyal (15.0%)
| Metric | Value |
|---|---|
| Avg. Recency | ~11 days |
| Avg. Frequency | ~259 transactions |
| Avg. Monetary | ~$5,933 |
| Customer Count | 1,203 |

Highest value segment — frequent, recent, and high-spending. **Priority: Retention & advocacy.**

---

### Cluster 2 — 🟡 Potential Loyalists (49.4%)
| Metric | Value |
|---|---|
| Avg. Recency | ~68 days |
| Avg. Frequency | ~69 transactions |
| Avg. Monetary | ~$1,200 |
| Customer Count | 3,950 |

Balanced RFM scores with strong growth potential. **Priority: Convert to loyal tier.**

---

## 📊 Dashboard

The Power BI dashboard (`customer_Segment.pbix`) includes:

| Visual | Description |
|---|---|
| **KPI Cards** | Avg Frequency, Avg Monetary, Avg Recency, Total Revenue |
| **Bar Chart** | Revenue by Segment Name |
| **Scatter Plot** | Customer clusters in RFM feature space |
| **Pie Chart** | Repeat buyer vs one-time buyer split |
| **Donut Chart** | Cluster size distribution |
| **Line Chart** | Revenue by month, broken down by segment |
| **Bar Chart** | Customer count & Drop-Off % by segment |

**Slicers:** Month, Year (2010 / 2011)

> 💡 **Tip:** To fix the month sort order in Power BI, use **Column Tools → Sort by Column → MonthNumber**

---

## 🔑 Key Findings

1. **Revenue Concentration Risk** — 67% of revenue comes from At Risk customers facing churn
2. **Retention Crisis** — 91.62% one-time buyer rate signals a fundamental retention failure
3. **Underperforming Loyal Segment** — Only £0.4M (5.4%) revenue from the most valuable customers
4. **Biggest Opportunity** — Potential Loyalists (42% of base) are closest to conversion
5. **Seasonal Peaks** — Revenue spikes in Q4 (Oct–Nov), indicating high seasonal dependency

### Strategic Recommendations

| Segment | Strategy | Key Tactics |
|---|---|---|
| At Risk | Win-back | Personalised discount emails, urgency messaging, feedback surveys |
| Potential Loyal | Convert | Loyalty programmes, tiered rewards, bundle promotions |
| Loyal | Retain & Grow | VIP benefits, early access, referral incentives |
| One-time Buyers | 2nd purchase | Post-purchase follow-up, 10% second-order discount |

---

## 📁 Project Structure

```
customer-segmentation/
│
├── data/
│   ├── raw/                    # Original dataset (not uploaded — see link above)
│   └── processed/              # Cleaned & RFM-engineered data
│
├── notebooks/
│   ├── 01_data_preprocessing.ipynb
│   ├── 02_rfm_engineering.ipynb
│   ├── 03_kmeans_clustering.ipynb
│   └── 04_visualisation.ipynb
│
├── dashboard/
│   └── customer_Segment.pbix   # Power BI dashboard file
│
├── reports/
│   └── Customer_Segmentation_Report.docx
│
├── images/
│   └── dashboard_screenshot.png
│
├── requirements.txt
└── README.md
```

---

## ⚙️ Installation & Setup

### Prerequisites
- Python 3.8+
- Microsoft Power BI Desktop ([Download](https://powerbi.microsoft.com/en-us/downloads/))

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/customer-segmentation.git
cd customer-segmentation
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Download the Dataset
Download the dataset from the [Google Drive link](https://drive.google.com/file/d/1nCwyLb5mTuouiigvi6sbNBT91W3MLUtF/view?usp=sharing) and place it in `data/raw/`.

### 4. Run the Notebooks
```bash
jupyter notebook notebooks/
```
Run notebooks in order: `01 → 02 → 03 → 04`

### 5. Open the Dashboard
Open `dashboard/customer_Segment.pbix` in Power BI Desktop.

---

## 📦 Requirements

```
pandas>=1.3.0
numpy>=1.21.0
scikit-learn>=0.24.0
matplotlib>=3.4.0
seaborn>=0.11.0
jupyter>=1.0.0
openpyxl>=3.0.0
```

Save as `requirements.txt`.

---

## 📈 Results

| Segment | Customers | % of Base | Avg Revenue | Recency | Frequency |
|---|---|---|---|---|---|
| At Risk | 1,440 | 36.81% | £4.9M total | High (~165d) | Low (~15) |
| Potential Loyal | 1,650 | 42.17% | £2.0M total | Medium (~68d) | Medium (~69) |
| Loyal | 820 | 21.02% | £0.4M total | Low (~11d) | High (~259) |

**Model Validation:**
- Silhouette Score: **0.42**
- Optimal K confirmed via Elbow Method at K=3

---

## 🛠️ Technologies Used

| Tool | Purpose |
|---|---|
| **Python** | Data preprocessing, feature engineering, clustering |
| **Pandas / NumPy** | Data manipulation and numerical operations |
| **Scikit-learn** | K-Means clustering, normalisation, validation metrics |
| **Matplotlib / Seaborn** | Exploratory visualisation |
| **Power BI Desktop** | Interactive business intelligence dashboard |
| **DAX** | Calculated measures and KPIs in Power BI |
| **Power Query** | Data transformation within Power BI |
| **Jupyter Notebook** | Analysis environment |

---

## 🤝 Contributing

Contributions, issues and feature requests are welcome.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add some feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Ar Kumar**
- 📧 Email: your.email@example.com
- 🔗 LinkedIn: [your-linkedin](https://linkedin.com/in/your-profile)
- 🐙 GitHub: [@your-username](https://github.com/your-username)

---

> *This project was completed as part of an academic submission for Business Analytics / Data Science, June 2026.*
