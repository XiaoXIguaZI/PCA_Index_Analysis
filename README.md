Replicating GSW2005 Yield Curve
==================

## Overview
This project replicates the methodology of Gürkaynak, Sack, and Wright (2005) (GSW2005) for estimating the U.S. Treasury yield curve. The implementation follows their data filtering process, curve estimation techniques, and error minimization approach.

## Features
- **Data Filtering:** Applies GSW filtering rules to CRSP Treasury data.
- **Nelson-Siegel-Svensson (NSS) Model:** Implements the yield curve estimation model.
- **Yield Curve Visualization:** Produces fitted yield curves and forward rate structures.
- **Performance Comparison:** Compares results against Federal Reserve published yields.

## Installation
Ensure you have Python installed and set up the environment:
```bash
conda env create -f environment.yml
conda activate hw4_env
```
Or install manually:
```bash
pip install -r requirements.txt
```

## Usage
### Running Jupyter Notebooks
Navigate to the `src/` directory and run:
```bash
jupyter notebook
```
Open `02_replicate_GSW2005.ipynb` to execute the yield curve replication.

### Running the Analysis
```bash
python src/load_fred.py  # Load data
python src/pca_index.py   # Estimate yield curve
```

## Project Structure
```
├── assets                 # Project assets (e.g., logos)
├── data/manual            # Data folder for manual input files
├── docs                   # Documentation files
│   ├── api.rst            # API documentation
│   ├── conf.py            # Sphinx config file
│   ├── index.rst          # Main documentation index
│   ├── myst_markdown_demos.md
├── src                    # Source code and notebooks
│   ├── 01_CRSP_treasury_overview.ipynb
│   ├── 02_replicate_GSW2005.ipynb
│   ├── config.py
│   ├── load_fred.py       # Data loading script for FRED dataset
│   ├── misc_tools.py      # Miscellaneous helper functions
│   ├── pca_index.py       # Yield curve estimation implementation
│   ├── test_misc_tools.py # Unit tests for misc_tools
├── environment.yml        # Conda environment setup
├── requirements.txt       # Python dependencies
├── pyproject.toml         # Project configuration
```

## Theoretical Background
### **1. Nelson-Siegel-Svensson (NSS) Model**
The NSS model expresses instantaneous forward rates using a functional form:
\[
f(n) = \beta_1 + \beta_2 e^{-n/\tau_1} + \beta_3\left(\frac{n}{\tau_1}\right)e^{-n/\tau_1} + \beta_4\left(\frac{n}{\tau_2}\right)e^{-n/\tau_2}
\]
where:
- \( \beta_1 \) is the long-term asymptotic rate.
- \( \beta_2, \beta_3, \beta_4 \) control the shape of the yield curve.
- \( \tau_1, \tau_2 \) determine the decay speed of the factors.

### **2. Data Filtering (GSW Methodology)**
The replication follows GSW2005’s filtering process:
- Excludes securities with <3 months to maturity.
- Excludes on-the-run and first off-the-run issues after 1980.
- Excludes T-bills (only notes and bonds are considered).
- Excludes 20-year bonds after 1996 with a gradual decay rule.
- Excludes callable bonds.

### **3. Curve Fitting**
The model minimizes the weighted squared error between observed and estimated prices:
\[
\min_{\beta,\tau} \sum_{i=1}^N \frac{(P_i^{obs} - P_i^{model})^2}{D_i}
\]
where:
- \( P_i^{obs} \) = Observed clean price.
- \( P_i^{model} \) = Model-implied price.
- \( D_i \) = Duration of security \( i \).

### **4. Performance Evaluation**
The implementation compares fitted yields against Federal Reserve GSW yields to assess accuracy.
