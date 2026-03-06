# Analysis Scripts Documentation

## Script Overview

### 1. `hypothesis_testing.ipynb`
**Purpose:** Complete statistical analysis workflow for discount impact evaluation  
**Language:** Python with statistical libraries  
**Size:** 25KB comprehensive analysis notebook  
**Execution Time:** ~2-3 minutes for complete analysis

#### Analysis Sections:
1. **Library Import & Setup:** Required packages and configuration
2. **Data Loading:** Import and initial data inspection
3. **Exploratory Data Analysis:** Descriptive statistics and visualization
4. **Statistical Testing:** Paired t-test implementation
5. **Results Interpretation:** Statistical and practical significance
6. **Visualization:** Boxplots and distribution analysis

## Detailed Workflow

### Section 1: Library Import
```python
# Core statistical libraries
import pandas as pd                    # Data manipulation
import numpy as np                     # Numerical operations
import matplotlib.pyplot as plt        # Visualization
import seaborn as sns                  # Statistical plotting
from scipy import stats                # Statistical tests
```

### Section 2: Data Loading & Inspection
```python
# Load dataset
df = pd.read_csv('sales_before_after_discount.csv')

# Data inspection
print("Dataset Shape:", df.shape)
print("\nColumn Names:", df.columns.tolist())
print("\nData Types:")
print(df.dtypes)
print("\nMissing Values:")
print(df.isnull().sum())
print("\nFirst 5 Rows:")
print(df.head())
```

### Section 3: Descriptive Statistics
```python
# Separate variables
before_sales = df['Sales_Before_Discount']
after_sales = df['Sales_After_Discount']

# Descriptive statistics
print("Before Discount Statistics:")
print(before_sales.describe())
print("\nAfter Discount Statistics:")
print(after_sales.describe())

# Calculate differences
differences = after_sales - before_sales
print("\nDifference Statistics:")
print(differences.describe())
```

### Section 4: Data Visualization
```python
# Boxplot comparison
plt.figure(figsize=(10, 6))
plt.boxplot([before_sales, after_sales], 
           labels=['Before Discount', 'After Discount'])
plt.title('Sales Comparison: Before vs After Discount')
plt.ylabel('Sales Amount')
plt.grid(True, alpha=0.3)
plt.show()

# Distribution plots
plt.figure(figsize=(12, 4))

plt.subplot(1, 2, 1)
plt.hist(before_sales, alpha=0.7, label='Before', bins=15)
plt.hist(after_sales, alpha=0.7, label='After', bins=15)
plt.title('Sales Distribution')
plt.xlabel('Sales Amount')
plt.ylabel('Frequency')
plt.legend()

plt.subplot(1, 2, 2)
plt.hist(differences, bins=15, color='green', alpha=0.7)
plt.title('Difference Distribution')
plt.xlabel('Sales Difference (After - Before)')
plt.ylabel('Frequency')

plt.tight_layout()
plt.show()
```

### Section 5: Statistical Testing
```python
# Paired t-test
t_statistic, p_value_two_tailed = stats.ttest_rel(after_sales, before_sales)

# One-tailed test (testing for increase)
p_value_one_tailed = p_value_two_tailed / 2

# Calculate test statistics
mean_diff = np.mean(differences)
std_diff = np.std(differences, ddof=1)
n = len(differences)
standard_error = std_diff / np.sqrt(n)

# Manual t-statistic verification
t_manual = mean_diff / standard_error

print(f"t-statistic (scipy): {t_statistic:.4f}")
print(f"t-statistic (manual): {t_manual:.4f}")
print(f"p-value (two-tailed): {p_value_two_tailed:.4f}")
print(f"p-value (one-tailed): {p_value_one_tailed:.4f}")
```

### Section 6: Effect Size & Confidence Intervals
```python
# Cohen's d for paired samples
cohens_d = mean_diff / std_diff

# Effect size interpretation
def interpret_effect_size(d):
    if abs(d) < 0.2:
        return "Negligible"
    elif abs(d) < 0.5:
        return "Small"
    elif abs(d) < 0.8:
        return "Medium"
    else:
        return "Large"

effect_interpretation = interpret_effect_size(cohens_d)

# 95% Confidence Interval
confidence_level = 0.95
degrees_freedom = n - 1
t_critical = stats.t.ppf((1 + confidence_level) / 2, degrees_freedom)
margin_error = t_critical * standard_error
ci_lower = mean_diff - margin_error
ci_upper = mean_diff + margin_error

print(f"Cohen's d: {cohens_d:.4f} ({effect_interpretation} effect)")
print(f"95% CI: [{ci_lower:.4f}, {ci_upper:.4f}]")
```

### Section 7: Results Interpretation
```python
# Statistical significance
alpha = 0.05
statistical_significance = p_value_one_tailed < alpha and t_statistic > 0

# Practical significance
percent_increase = (mean_diff / np.mean(before_sales)) * 100

# Final interpretation
if statistical_significance:
    conclusion = "REJECT H₀: Sales significantly increased after discount"
    business_recommendation = "Continue discount strategy with confidence"
else:
    conclusion = "FAIL TO REJECT H₀: No significant sales increase"
    business_recommendation = "Reconsider discount strategy"

print(f"\nStatistical Conclusion: {conclusion}")
print(f"Business Recommendation: {business_recommendation}")
print(f"Average Sales Increase: {percent_increase:.2f}%")
```

## Advanced Analysis Options

### Normality Testing
```python
# Shapiro-Wilk test for normality
shapiro_stat, shapiro_p = stats.shapiro(differences)
print(f"Shapiro-Wilk test: W={shapiro_stat:.4f}, p={shapiro_p:.4f}")

if shapiro_p > alpha:
    print("Normality assumption satisfied")
else:
    print("Normality violated - consider non-parametric test")
    # Wilcoxon signed-rank test
    wilcoxon_stat, wilcoxon_p = stats.wilcoxon(after_sales, before_sales)
    print(f"Wilcoxon test: p={wilcoxon_p:.4f}")
```

### Power Analysis
```python
# Post-hoc power calculation
from statsmodels.stats.power import ttest_power

effect_size = cohens_d
power = ttest_power(effect_size, nobs=n, alpha=alpha, alternative='larger')
print(f"Statistical Power: {power:.4f}")

if power < 0.8:
    print("Low power - consider larger sample size")
else:
    print("Adequate power for reliable conclusions")
```

### Bootstrap Analysis
```python
# Bootstrap confidence intervals
def bootstrap_mean_diff(data, n_bootstrap=1000):
    bootstrap_means = []
    for _ in range(n_bootstrap):
        sample = np.random.choice(data, size=len(data), replace=True)
        bootstrap_means.append(np.mean(sample))
    return np.percentile(bootstrap_means, [2.5, 97.5])

bootstrap_ci = bootstrap_mean_diff(differences)
print(f"Bootstrap 95% CI: [{bootstrap_ci[0]:.4f}, {bootstrap_ci[1]:.4f}]")
```

## Usage Instructions

### Running the Complete Analysis
```bash
# 1. Install required packages
pip install pandas numpy scipy matplotlib seaborn jupyter

# 2. Navigate to project directory
cd Hypothesis-Testing-Discount-Impact

# 3. Launch Jupyter Notebook
jupyter notebook scripts/hypothesis_testing.ipynb

# 4. Execute cells sequentially
# Run all cells in order for complete analysis
```

### Custom Analysis Template
```python
# Template for custom hypothesis testing
def custom_paired_test(data_before, data_after, alpha=0.05):
    """
    Custom paired t-test function
    """
    # Basic statistics
    n = len(data_before)
    differences = data_after - data_before
    mean_diff = np.mean(differences)
    std_diff = np.std(differences, ddof=1)
    
    # Statistical test
    t_stat, p_val = stats.ttest_rel(data_after, data_before)
    p_one_tailed = p_val / 2
    
    # Effect size
    cohens_d = mean_diff / std_diff
    
    # Results
    significant = p_one_tailed < alpha and t_stat > 0
    
    return {
        't_statistic': t_stat,
        'p_value': p_one_tailed,
        'effect_size': cohens_d,
        'mean_difference': mean_diff,
        'significant': significant
    }
```

## Troubleshooting

### Common Issues
1. **File Not Found:** Check file path and ensure CSV is in data/ folder
2. **Import Errors:** Verify all packages are installed correctly
3. **Memory Issues:** For large datasets, process in chunks
4. **Statistical Errors:** Check data types and missing values

### Debug Tips
```python
# Data validation checks
def validate_data(df):
    checks = {
        'missing_values': df.isnull().sum().sum(),
        'negative_values': (df < 0).sum().sum(),
        'sample_size': len(df),
        'data_types': df.dtypes.to_dict()
    }
    return checks

# Run validation
validation_results = validate_data(df)
print("Data Validation Results:")
for key, value in validation_results.items():
    print(f"{key}: {value}")
```

## Performance Optimization

### Large Dataset Handling
```python
# Memory-efficient processing for large datasets
chunk_size = 1000
results = []

for chunk in pd.read_csv('large_sales_data.csv', chunksize=chunk_size):
    # Process each chunk
    chunk_result = process_chunk(chunk)
    results.append(chunk_result)

# Combine results
final_result = combine_results(results)
```

### Parallel Processing
```python
# Parallel bootstrap for faster computation
from multiprocessing import Pool

def bootstrap_worker(args):
    data, n_samples = args
    sample = np.random.choice(data, size=n_samples, replace=True)
    return np.mean(sample)

# Parallel bootstrap
with Pool() as pool:
    bootstrap_args = [(differences, len(differences))] * 1000
    bootstrap_results = pool.map(bootstrap_worker, bootstrap_args)
```

## Extension Ideas

### Multiple Comparison Tests
```python
# Bonferroni correction for multiple tests
def bonferroni_correction(p_values, alpha=0.05):
    corrected_alpha = alpha / len(p_values)
    significant = [p < corrected_alpha for p in p_values]
    return corrected_alpha, significant
```

### Bayesian Analysis
```python
# Bayesian t-test alternative
import pymc3 as pm

with pm.Model() as model:
    # Priors
    mu_diff = pm.Normal('mu_diff', mu=0, sd=10)
    sigma_diff = pm.HalfCauchy('sigma_diff', beta=1)
    
    # Likelihood
    likelihood = pm.Normal('likelihood', mu=mu_diff, sd=sigma_diff, 
                          observed=differences)
    
    # Sampling
    trace = pm.sample(2000, tune=1000)
```
