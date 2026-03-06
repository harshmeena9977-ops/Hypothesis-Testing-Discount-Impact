# Statistical Methods Documentation

## Hypothesis Testing Framework

### 1. Research Design
**Study Type:** Quasi-experimental, pre-post test design  
**Sample:** Retail stores with before/after discount measurements  
**Dependent Variable:** Sales performance metrics  
**Independent Variable:** Discount implementation  

### 2. Statistical Test Selection

#### Paired t-test Rationale
```python
# Why paired t-test?
# 1. Same stores measured before and after (dependent samples)
# 2. Continuous outcome variable (sales data)
# 3. Normal distribution assumption
# 4. Small to moderate sample size
```

#### Test Assumptions
1. **Independence:** Stores are independent of each other
2. **Normality:** Differences between pairs are normally distributed
3. **Scale:** Sales data is continuous
4. **Random Sampling:** Stores represent population

### 3. Hypothesis Formulation

#### Statistical Hypotheses
- **H₀ (Null Hypothesis):** μ_before = μ_after  
  *No significant difference in sales before and after discount*
  
- **H₁ (Alternative Hypothesis):** μ_after > μ_before  
  *Sales significantly increased after discount implementation*

#### Significance Level
- **α = 0.05** (95% confidence level)
- **One-tailed test** (testing for increase, not just difference)
- **Critical Value:** t_critical based on degrees of freedom

### 4. Test Implementation

#### Python Code
```python
import numpy as np
from scipy import stats
import pandas as pd

# Load data
df = pd.read_csv('sales_before_after_discount.csv')
before_sales = df['Sales_Before_Discount']
after_sales = df['Sales_After_Discount']

# Paired t-test
t_statistic, p_value_two_tailed = stats.ttest_rel(after_sales, before_sales)

# One-tailed test (testing for increase)
p_value_one_tailed = p_value_two_tailed / 2

# Decision rule
alpha = 0.05
if p_value_one_tailed < alpha and t_statistic > 0:
    result = "Reject H₀: Significant increase"
else:
    result = "Fail to reject H₀: No significant increase"
```

#### Manual Calculation
```python
# Step-by-step calculation
differences = after_sales - before_sales
mean_diff = np.mean(differences)
std_diff = np.std(differences, ddof=1)
n = len(differences)
standard_error = std_diff / np.sqrt(n)

# t-statistic
t_manual = mean_diff / standard_error

# Degrees of freedom
df = n - 1

# Critical value (one-tailed)
t_critical = stats.t.ppf(1 - alpha, df)
```

### 5. Effect Size Analysis

#### Cohen's d Calculation
```python
# Effect size for paired samples
cohens_d = mean_diff / std_diff

# Effect size interpretation
def interpret_cohens_d(d):
    if abs(d) < 0.2:
        return "Negligible effect"
    elif abs(d) < 0.5:
        return "Small effect"
    elif abs(d) < 0.8:
        return "Medium effect"
    else:
        return "Large effect"

effect_interpretation = interpret_cohens_d(cohens_d)
```

#### Practical Significance
```python
# Percentage increase
percent_increase = (mean_diff / np.mean(before_sales)) * 100

# Business impact assessment
if percent_increase > 5:
    practical_significance = "High business impact"
elif percent_increase > 2:
    practical_significance = "Moderate business impact"
else:
    practical_significance = "Low business impact"
```

### 6. Confidence Intervals

#### 95% Confidence Interval
```python
# Confidence interval for mean difference
confidence_level = 0.95
degrees_freedom = n - 1
t_critical_ci = stats.t.ppf((1 + confidence_level) / 2, degrees_freedom)

margin_error = t_critical_ci * standard_error
ci_lower = mean_diff - margin_error
ci_upper = mean_diff + margin_error

print(f"95% CI: [{ci_lower:.2f}, {ci_upper:.2f}]")
```

#### Interpretation
- **CI excludes zero:** Statistically significant result
- **CI width:** Precision of estimate (narrower = more precise)
- **Business relevance:** Range of plausible effects

### 7. Power Analysis

#### Statistical Power
```python
# Post-hoc power analysis
from statsmodels.stats.power import ttest_power

# Effect size
effect_size = cohens_d

# Power calculation
power = ttest_power(effect_size, nobs=n, alpha=alpha, alternative='larger')

print(f"Statistical Power: {power:.2f}")
```

#### Sample Size Planning
```python
# Required sample size for desired power
desired_power = 0.80
required_n = ttest_power.solve_power(effect_size, power=desired_power, 
                                   alpha=alpha, alternative='larger')
print(f"Required sample size: {ceil(required_n)}")
```

### 8. Diagnostic Tests

#### Normality Assessment
```python
# Shapiro-Wilk test for normality of differences
shapiro_stat, shapiro_p = stats.shapiro(differences)

if shapiro_p > alpha:
    normality_assumption = "Normal distribution confirmed"
else:
    normality_assumption = "Normality violated - consider non-parametric test"
```

#### Alternative Non-parametric Test
```python
# Wilcoxon signed-rank test (if normality violated)
wilcoxon_stat, wilcoxon_p = stats.wilcoxon(after_sales, before_sales)

# Interpretation
if wilcoxon_p < alpha:
    non_parametric_result = "Significant difference (Wilcoxon)"
else:
    non_parametric_result = "No significant difference (Wilcoxon)"
```

### 9. Robustness Checks

#### Outlier Analysis
```python
# Identify outliers using IQR method
Q1 = np.percentile(differences, 25)
Q3 = np.percentile(differences, 75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

outliers = differences[(differences < lower_bound) | (differences > upper_bound)]

# Analysis with and without outliers
if len(outliers) > 0:
    # Remove outliers and re-run test
    clean_differences = differences[(differences >= lower_bound) & 
                                  (differences <= upper_bound)]
    # Re-calculate test statistics
```

#### Sensitivity Analysis
```python
# Bootstrap confidence intervals
def bootstrap_mean_diff(data, n_bootstrap=1000):
    bootstrap_means = []
    for _ in range(n_bootstrap):
        sample = np.random.choice(data, size=len(data), replace=True)
        bootstrap_means.append(np.mean(sample))
    return np.percentile(bootstrap_means, [2.5, 97.5])

bootstrap_ci = bootstrap_mean_diff(differences)
```

### 10. Reporting Standards

#### APA Style Results
```
A paired-samples t-test was conducted to compare sales before and after 
discount implementation across n stores. Results indicated a statistically 
significant increase in sales after discount implementation (M = [after_mean], 
SD = [after_sd]) compared to before discount (M = [before_mean], SD = [before_sd]), 
t([df]) = [t_value], p = [p_value], d = [cohens_d]. The 95% confidence interval 
for the mean difference was [ci_lower, ci_upper].
```

#### Business Report Summary
```
Executive Summary:
- Statistical Evidence: p < 0.05 confirms discount effectiveness
- Practical Impact: X% average sales increase per store
- Business Recommendation: Continue and expand discount strategy
- Risk Assessment: Low statistical risk, high confidence in results
```

## Quality Assurance

### Validation Checklist
- [ ] Data integrity verified (no missing values, correct formats)
- [ ] Assumptions tested and documented
- [ ] Effect size calculated and interpreted
- [ ] Confidence intervals reported
- [ ] Power analysis conducted
- [ ] Robustness checks performed
- [ ] Results reproducible
- [ ] Documentation complete

### Common Pitfalls
1. **Ignoring assumptions:** Always test normality for t-tests
2. **Multiple testing:** Adjust alpha for multiple comparisons
3. **Confusing significance with importance:** Report effect sizes
4. **One-tailed vs. two-tailed:** Justify test direction
5. **Sample size:** Ensure adequate power for meaningful results
