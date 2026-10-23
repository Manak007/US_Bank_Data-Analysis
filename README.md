<div align="center">

# 🏦 U.S. Bank Financial Performance & Risk Analysis

**Exploratory data analysis of profitability, size and capital risk across 4,388 FDIC-insured U.S. banks**

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?logo=numpy&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=white)
![Data: FDIC](https://img.shields.io/badge/Data-FDIC%20Q3%202025-6D213C)

</div>

> **Credit:** This repository is a fork of [**marivrnina/us-bank-financial-performance-analysis**](https://github.com/marivrnina/us-bank-financial-performance-analysis), originally created by **Mariia Voronina**. The analysis, notebook and findings are her work. This copy is maintained by **Manavi Singh**.

---

## 📌 At a Glance

| | |
|---|---|
| **Banks analysed** | 4,388 U.S. institutions |
| **Reporting period** | Q3 2025 (30 Sep 2025) |
| **Variables** | 161 raw → 153 after cleaning |
| **Median ROA / ROE / NIM** | 1.10% / 10.96% / 3.67% |
| **Unprofitable banks** | 227 (5.2%) |
| **Key data-quality flag** | 1,720 banks (39.2%) report a 0% risk-based capital ratio |

---

## 📖 Table of Contents

1. [Business Questions](#-business-questions)
2. [Dataset](#-dataset)
3. [Tech Stack](#-tech-stack)
4. [Repository Structure](#-repository-structure)
5. [Methodology](#-methodology)
6. [Key Findings](#-key-findings)
7. [Conclusion](#-conclusion)
8. [Limitations & Next Steps](#-limitations--next-steps)
9. [How to Run](#-how-to-run)
10. [Credits](#-credits)

---

## 🎯 Business Questions

- How profitable are U.S. banks today, and how widely does profitability vary?
- Are larger banks more profitable than smaller banks?
- Does a higher net interest margin translate into higher overall returns?
- How do lending intensity and capital adequacy relate to profitability?
- Which data-quality issues could distort conclusions drawn from regulatory data?

---

## 🗂 Dataset

| Attribute | Detail |
|---|---|
| **Source** | [Federal Deposit Insurance Corporation (FDIC) – Bank Data Downloads](https://www.fdic.gov/bank-data-guide/data-downloads) |
| **Dataset** | FDIC Quarterly Financial Data (institution level) |
| **Reporting period** | Q3 2025 (September 30, 2025) |
| **Institutions** | 4,388 |
| **Original variables** | 161 |
| **Coverage** | United States |

The data includes institution-level assets, deposits, income, profitability ratios and regulatory capital indicators. See [`data/README.md`](data/README.md) for download instructions and a field dictionary.

---

## 🛠 Tech Stack

| Tool | Purpose |
|---|---|
| **Python** | Data processing and analysis |
| **Pandas** | Cleaning, aggregation, grouped financial comparisons |
| **NumPy** | Numerical operations and conditional logic |
| **Matplotlib & Seaborn** | Statistical visualisation |
| **Jupyter Notebook** | Interactive, documented analysis |

---

## 📁 Repository Structure

```
us-bank-financial-performance-analysis/
├── notebooks/
│   └── bank_data_eda.ipynb      # Full analysis notebook
├── data/
│   └── README.md                # How to download the FDIC dataset + field dictionary
├── images/                      # Charts exported from the notebook
├── requirements.txt             # Python dependencies
├── .gitignore
└── README.md
```

---

## 🔬 Methodology

### 1. Data Exploration & Cleaning
- Profiled shape, data types and missing values (88,885 missing cells; 0 duplicate rows).
- Removed **8 fully empty columns** (161 → 153 variables).
- Flagged extreme profitability observations (|ROA| > 10% or |ROE| > 100%) for review rather than silently dropping them.
- Investigated **zero risk-based capital ratios** and ran a sensitivity analysis excluding them.

### 2. Profitability Analysis

| Metric | Field | Description |
|---|---|---|
| Return on Assets (ROA) | `ROA` | Profitability relative to total assets |
| Return on Equity (ROE) | `ROE` | Profitability relative to shareholders' equity |
| Net Interest Margin (NIM) | `NIMY` | Net interest income relative to average earning assets |

Distributions were plotted with the 1st–99th percentile range to keep outliers from hiding the shape of the data.

### 3. Bank Size Segmentation
Banks were grouped by total assets into **< $1B**, **$1B–$10B**, **$10B–$100B** and **> $100B**. Groups were compared on **medians** to limit the influence of extreme values.

### 4. Financial Risk Analysis
- **Loan-to-Deposit Ratio (`LNLSDEPR`)**: lending intensity and potential funding pressure.
- **Risk-Based Capital Ratio (`RBCRWAJ`)**: regulatory capital relative to risk-weighted assets.

### 5. Correlation Analysis
**Spearman rank correlation** was used because it is robust to outliers and does not assume linear relationships.

---

## 📊 Key Findings

### 1. Most banks are profitable, but the spread is wide

| Metric | Result |
|---|---:|
| Median ROA | **1.10%** |
| Median ROE | **10.96%** |
| Median NIM | **3.67%** |
| Banks with negative ROA | **227 (5.2%)** |

![Profitability distributions](images/01_profitability_distributions.png)

![Profitable vs unprofitable banks](images/02_profitability_status.png)

### 2. Bigger isn't clearly more profitable

| Bank size | Banks | Median ROA | Median ROE | Median NIM | Median Loan/Deposit |
|---|---:|---:|---:|---:|---:|
| < $1B | 3,331 | 1.10% | 10.94% | **3.75%** | 77.16% |
| $1B–$10B | 900 | 1.10% | 11.18% | 3.55% | 86.32% |
| $10B–$100B | 127 | 1.06% | 9.54% | 3.36% | 83.10% |
| > $100B | 30 | **1.14%** | **11.72%** | 3.04% | 70.27% |

The largest banks post the highest median ROA (1.14%) despite the **lowest** NIM (3.04%). Smaller banks earn more on lending (3.75% NIM) but end with similar returns. This points to non-interest income, cost efficiency and business-model differences rather than lending spread alone.

![Profitability by bank size](images/08_profitability_by_size.png)

![Bank size vs ROA](images/04_size_vs_roa.png)

### 3. A large data-quality issue in capital ratios

- **1,720 banks (39.2%)** report a risk-based capital ratio of exactly **0%**.
- The issue is concentrated in small banks: **45.9%** of banks under $1B vs **0%** of banks over $100B.
- Excluding zeros moves the median capital ratio from **12.93% → 15.53%**, showing how much the treatment of these values changes the conclusions.

![Risk indicator distributions](images/05_risk_distributions.png)

### 4. Correlations

| Pair | Spearman ρ | Interpretation |
|---|---:|---|
| ROA ↔ ROE | **0.84** | Strong positive |
| ROA ↔ NIM | **0.54** | Moderate positive |
| Capital ratio ↔ Loan/Deposit* | **−0.45** | Moderate negative |
| Profitability ↔ Risk indicators | ≈ 0 | Weak / none |

<sub>*Excluding zero reported capital ratios.</sub>

![Correlation heatmap](images/07_correlation_heatmap.png)

![Loan-to-deposit vs ROA](images/06_ltd_vs_roa.png)

> These relationships are descriptive and do not establish causation.

---

## ✅ Conclusion

- **Size alone does not drive profitability.** The largest U.S. banks are only marginally ahead on ROA, despite materially lower net interest margins.
- **One metric is not enough.** NIM and ROA tell different stories, so bank performance should be judged on several indicators together.
- **Data treatment matters.** Outliers and reporting conventions, such as zero capital ratios, can shift headline statistics substantially.

---

## ⚠️ Limitations & Next Steps

**Limitations**
- Single reporting period, so no long-term trends.
- Extreme ratios may reflect unusual events, special-purpose charters or reporting issues.
- Zero risk-based capital ratios need verification against FDIC reporting definitions.
- Risk is assessed with selected indicators, not a full risk model.
- Correlation does not imply causation.

**Possible extensions**
- Add multiple quarters to analyse trends over time.
- Confirm the meaning of zero capital ratios with FDIC definitions.
- Include efficiency ratio and non-interest income to explain the NIM vs ROA gap.
- Segment by charter type, state or specialisation (e.g. credit-card banks).

---

## ▶️ How to Run

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/us-bank-financial-performance-analysis.git
cd us-bank-financial-performance-analysis

# 2. Install dependencies
pip install -r requirements.txt

# 3. Download the FDIC dataset into data/ (see data/README.md)

# 4. Launch the notebook
jupyter notebook notebooks/bank_data_eda.ipynb
```

---

##  Credits
- **Maintained by:** Manavi Singh
- **Data:** [Federal Deposit Insurance Corporation (FDIC)](https://www.fdic.gov/bank-data-guide/data-downloads)
