PCA Index Analysis
==================


# Overview

This project provides a framework for constructing a **Principal Component Analysis (PCA) index** using economic data from the Federal Reserve Economic Data (FRED). It includes data preprocessing, PCA computation, and visualization.
Assignment 4 for Data Science Tools for Finance, by Dr. Jeremy Bejarano

## 📖 Mathematical Background

### Principal Component Analysis (PCA)
PCA is a dimensionality reduction technique that transforms correlated variables into a set of uncorrelated components (principal components). The transformation is defined as follows:

![Equation](https://latex.codecogs.com/png.latex?X%20%3D%20U%20%5CSigma%20V%5ET)

where:
- ![Equation](https://latex.codecogs.com/png.latex?X) is the standardized data matrix,
- ![Equation](https://latex.codecogs.com/png.latex?U) is the left singular matrix (eigenvectors of ![Equation](https://latex.codecogs.com/png.latex?XX%5ET)),
- ![Equation](https://latex.codecogs.com/png.latex?%5CSigma) is the diagonal matrix of singular values,
- ![Equation](https://latex.codecogs.com/png.latex?V) is the right singular matrix (eigenvectors of ![Equation](https://latex.codecogs.com/png.latex?X%5ETX)).

The first few principal components capture most of the variance in the data, making PCA useful for constructing economic indices.

## 🏗️ Project Structure

```
├── README.md
├── assets
│   └── logo.png
├── data
│   └── manual
├── docs
│   ├── api.rst
│   ├── conf.py
│   ├── index.rst
│   └── myst_markdown_demos.md
├── dodo.py
├── env.example
├── env.example_alt
├── environment.yml
├── pyproject.toml
├── requirements.txt
└── src
    ├── 01_example_notebook.ipynb
    ├── 02_pca_index_visualizations.ipynb
    ├── 03_pca_index_dashboard.ipynb
    ├── config.py
    ├── factor_analysis_demo.ipynb
    ├── load_fred.py
    ├── misc_tools.py
    ├── pca_index.py
    └── test_misc_tools.py
```

## 🔄 Workflow

### 1. Load Economic Data
- `load_fred.py` fetches macroeconomic and financial series from FRED.
- The dataset includes variables such as interest rates, GDP, stock indices, and credit spreads.

### 2. Data Preprocessing
- `misc_tools.py` contains helper functions to clean, merge, and standardize the data.
- Standardization is performed using:
  ![Equation](https://latex.codecogs.com/png.latex?x%27%20%3D%20%5Cfrac%7Bx%20-%20%5Cmu%7D%7B%5Csigma%7D)
  
  where ![Equation](https://latex.codecogs.com/png.latex?%5Cmu) is the mean and ![Equation](https://latex.codecogs.com/png.latex?%5Csigma) is the standard deviation.

### 3. Principal Component Analysis
- `pca_index.py` computes the PCA transformation on standardized economic indicators.
- Uses **scikit-learn's PCA** and **Statsmodels' multivariate PCA**.
- Outputs principal components and their explained variance.

### 4. Visualization & Interpretation
- Notebooks (`02_pca_index_visualizations.ipynb`, `03_pca_index_dashboard.ipynb`) generate plots for:
  - Eigenvalues and explained variance
  - Factor loadings for each economic variable
  - Interactive dashboards using **Plotly**

## 🚀 Running the Project

### Installation
Ensure dependencies are installed:
```sh
conda env create -f environment.yml  # Using Conda
pip install -r requirements.txt       # Using pip
```

### Running Analysis
Run the data processing and PCA index computation:
```sh
python src/load_fred.py    # Download economic data
python src/pca_index.py    # Compute PCA index
```

### Visualization
Open Jupyter Notebooks for interactive visualizations:
```sh
jupyter notebook src/02_pca_index_visualizations.ipynb
```

## 📊 Output & Insights
- **PCA Economic Index**: A composite indicator capturing the main economic trends.
- **Factor Analysis**: Identifies macroeconomic variables that drive market movements.
- **Dynamic Dashboard**: Provides an interactive interface to explore PCA results.

