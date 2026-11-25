# Hypothesis Testing: Discount Impact

An independent statistics practice project comparing paired store-level sales values labelled before and after a discount. The repository contains a small CSV and a Jupyter notebook. The dataset's source, measurement units, discount details, and time periods are not documented, so this work should be treated as an exploratory exercise rather than evidence of a real retail intervention.

## Repository contents

- [Analysis notebook](scripts/hypothesis_testing.ipynb)
- [CSV data](data/sales_before_after_discount.csv)
- [Statistical methods notes](docs/statistical_methods.md)
- [Dependency notes](docs/requirment.txt.txt)
- [Additional documentation](docs/README.md.txt)

The dependency note is a plain text list, not a pip requirements file. Its filename is currently misspelled, and it does not list Jupyter.

## What the notebook contains

- Basic inspection and descriptive statistics for the 30 paired rows in the CSV
- A boxplot comparing the before and after values
- Shapiro-Wilk tests on the two raw columns
- A two-sided paired t-test using SciPy

The saved notebook output reports a paired-test statistic of approximately 3.863 and a two-sided p-value of approximately 0.00058. These are the checked-in notebook's saved outputs; the notebook has not been verified here by running it from a clean environment.

## Interpretation limits

- The CSV's provenance, units, sampling method, discount amounts, and before/after dates are not recorded.
- A before-and-after comparison without a documented control group or assignment method cannot establish that a discount caused any observed change.
- For a paired t-test, the normality assumption concerns the paired differences. The notebook instead applies Shapiro-Wilk tests to each raw column.
- The notebook's effect-size calculation divides the mean difference by the standard deviation of the before values; it is not the usual paired-sample Cohen's d.
- The README and the separate methods document previously described one-tailed testing, confidence intervals, and business impact more strongly than the notebook supports. No ROI, rollout recommendation, or verified business impact is claimed here.

## Reproduction status

The notebook currently reads a CSV from a personal Windows path (`C:/Users/meena/OneDrive/Documents/sales_before_after_discount.csv`) rather than from this repository. For a local run, first change that notebook cell to read `../data/sales_before_after_discount.csv` from the notebook's `scripts/` directory.

The repository does not currently provide a root-level `requirements.txt` or a clean-environment setup procedure. The notebook imports pandas, NumPy, Matplotlib, and SciPy; Jupyter is needed to run it. The dependency note is at `docs/requirment.txt.txt`.

## Tools

Python, pandas, NumPy, Matplotlib, SciPy, and Jupyter Notebook.

## Project status

Independent practice analysis. Dataset provenance, reproducibility, paired-difference diagnostics, and effect-size reporting need improvement before making a stronger statistical or business claim.
