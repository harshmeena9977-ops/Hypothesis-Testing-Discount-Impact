# 🧪 Hypothesis Testing: Discount Strategy Impact Analysis

## 📊 Problem Statement
A retail chain needed to validate whether their discount strategy was actually driving sales increases or if observed improvements were merely random fluctuations. Management required statistical evidence to justify continuing discount investments and optimize promotional spending across store locations.

## 🗄️ Dataset Overview
**Source:** Retail chain sales performance data  
**Size:** 581 bytes (store-level comparison data)  
**Study Design:** Paired samples (before/after discount comparison)  
**Time Period:** Pre and post-discount implementation periods  

### Key Features:
- **Store_ID:** Unique identifier for each retail location
- **Sales_Before_Discount:** Baseline sales performance metrics
- **Sales_After_Discount:** Post-promotion sales performance
- **Paired Design:** Same stores measured before and after intervention

## 🔬 Approach & Methodology

### 1. Statistical Framework
- **Hypothesis Testing:** Paired t-test for dependent samples
- **Significance Level:** α = 0.05 (95% confidence level)
- **Test Type:** One-tailed test (testing for sales increase)
- **Assumptions:** Normal distribution, dependent samples

### 2. Exploratory Data Analysis
- **Descriptive Statistics:** Mean, median, standard deviation analysis
- **Distribution Analysis:** Normality assessment for test validity
- **Visual Inspection:** Boxplots and histograms for data understanding

### 3. Statistical Testing
- **Paired t-test:** Compare mean differences between periods
- **Effect Size Calculation:** Cohen's d for practical significance
- **Confidence Intervals:** 95% CI for mean difference estimation

## 🛠️ Tools & Technologies

| Category | Tools | Purpose |
|----------|-------|---------|
| **Statistical Analysis** | Python, SciPy, Statsmodels | Hypothesis testing and statistical calculations |
| **Data Manipulation** | Pandas, NumPy | Data cleaning and transformation |
| **Visualization** | Matplotlib, Seaborn | Statistical plots and distributions |
| **Documentation** | Jupyter Notebook | Reproducible analysis workflow |

## 💡 Key Insights & Business Impact

### Statistical Results
- **Null Hypothesis (H₀):** μ_before = μ_after (no difference in sales)
- **Alternative Hypothesis (H₁):** μ_after > μ_before (sales increased)
- **Test Statistic:** t = calculated from paired differences
- **p-value:** < 0.05 (statistically significant result)

### Business Implications
- **Strategy Validation:** Discount strategy proven statistically effective
- **Investment Confidence:** Management can continue promotional investments
- **ROI Justification:** Statistical evidence supports marketing spend
- **Scalability:** Results support rolling out successful strategies chain-wide

### Practical Significance
- **Effect Size:** Cohen's d indicates practical business impact
- **Mean Difference:** Quantifiable sales increase per store
- **Confidence Level:** 95% confidence in observed improvements
- **Business Decision:** Data-driven discount policy implementation

## 📈 Statistical Analysis Results

### Hypothesis Testing Framework
```python
# Statistical test implementation
from scipy import stats

# Paired t-test for dependent samples
t_statistic, p_value = stats.ttest_rel(after_sales, before_sales)

# One-tailed test for sales increase
if p_value/2 < 0.05 and t_statistic > 0:
    print("Reject H₀: Sales significantly increased after discount")
else:
    print("Fail to reject H₀: No significant sales increase")
```

### Effect Size Analysis
```python
# Cohen's d calculation
mean_diff = np.mean(after_sales - before_sales)
std_diff = np.std(after_sales - before_sales)
cohens_d = mean_diff / std_diff

# Interpretation:
# d = 0.2: Small effect
# d = 0.5: Medium effect  
# d = 0.8: Large effect
```

### Confidence Interval
```python
# 95% Confidence Interval for mean difference
confidence_level = 0.95
degrees_freedom = len(after_sales) - 1
mean_diff = np.mean(after_sales - before_sales)
std_error = stats.sem(after_sales - before_sales)

ci = stats.t.interval(confidence_level, degrees_freedom, 
                     mean_diff, std_error)
```

## 🚀 How to Run This Project

### Prerequisites
```bash
# Install required Python packages
pip install pandas numpy scipy matplotlib seaborn jupyter
```

### Setup Instructions
```bash
# 1. Clone repository
git clone https://github.com/harshmeena9977-ops/Hypothesis-Testing-Discount-Impact.git
cd Hypothesis-Testing-Discount-Impact

# 2. Launch Jupyter Notebook
jupyter notebook hypothesis_testing.ipynb

# 3. Execute analysis cells sequentially
# Run all cells in order for complete analysis
```

### Analysis Workflow
```python
# Step 1: Import libraries and load data
import pandas as pd
from scipy import stats
df = pd.read_csv('sales_before_after_discount.csv')

# Step 2: Exploratory data analysis
print(df.describe())
df.boxplot(column=['Sales_Before_Discount', 'Sales_After_Discount'])

# Step 3: Statistical testing
before = df['Sales_Before_Discount']
after = df['Sales_After_Discount']
t_stat, p_value = stats.ttest_rel(after, before)

# Step 4: Results interpretation
alpha = 0.05
if p_value/2 < alpha and t_stat > 0:
    print(f"Statistically significant increase (p={p_value/2:.4f})")
```

## 📋 Project Structure
```
├── README.md                    # This file
├── hypothesis_testing.ipynb     # Main analysis notebook
├── sales_before_after_discount.csv  # Dataset
├── requirment.txt.txt          # Python dependencies
└── docs/                       # Additional documentation
    ├── statistical_methods.md  # Detailed methodology
    └── business_insights.md    # Executive summary
```

## 🏆 Resume Achievement

**Senior Data Analyst** | Statistical Discount Strategy Analysis  
*Designed and executed hypothesis testing framework validating discount strategy effectiveness, providing statistical evidence (p<0.05) that enabled data-driven promotional investment decisions across retail chain operations*

## 📞 Contact & Repository
- **GitHub:** [harshmeena9977-ops/Hypothesis-Testing-Discount-Impact](https://github.com/harshmeena9977-ops/Hypothesis-Testing-Discount-Impact)
- **Analysis Notebook:** Available in hypothesis_testing.ipynb
- **Dataset:** Store-level sales comparison data

---

*Last Updated: March 2026 | Project Version: 2.0 (Recruiter-Ready)*
