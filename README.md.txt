# Hypothesis Testing on Sales Data to Evaluate Discount Strategy

## Project Overview

**Domain:** Business Analytics / Retail Analytics  
**Goal:** Validate whether a discount strategy statistically increases sales.

## Dataset

**File:** data/"C:\Users\meena\OneDrive\Documents\sales_before_after_discount.csv"
**Columns:**

- Store_ID
- Sales_Before_Discount
- Sales_After_Discount

## Business Problem
Management wants to verify if discounts actually improve sales or if the increase happened by chance.

## Hypotheses
- H₀ (Null): Mean sales before discount = mean sales after discount
- H₁ (Alternative): Mean sales after discount > mean sales before discount

**Significance Level:** 0.05

## Methodology

- Paired t-test (dependent samples)
- Exploratory Data Analysis (EDA) with descriptive statistics
- Boxplot visualization
- Effect size calculation

## Results

- p-value < 0.05 → Reject H₀
- Sales increased significantly after discount

## Business Insight

Discount strategy is statistically and practically effective. Management can continue targeted discounts with confidence.

## How to Run the Code

1. Clone this repository  
   ```bash
   git clone https://github.com/harshmeena9977-ops/Hypothesis-Testing-Discount-Impact.git