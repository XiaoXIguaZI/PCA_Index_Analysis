PCA Index Analysis
==================

# About this project

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

# Quick Start

To quickest way to run code in this repo is to use the following steps. First, note that you must have TexLive installed on your computer and available in your path.
You can do this by downloading and installing it from here ([windows](https://tug.org/texlive/windows.html#install) and [mac](https://tug.org/mactex/mactex-download.html) installers).
Having installed LaTeX, open a terminal and navigate to the root directory of the project and create a conda environment using the following command:
```
conda create -n blank python=3.12
conda activate blank
```
and then install the dependencies with pip
```
pip install -r requirements.txt
```
You can then navigate to the `src` directory and then run 
```
doit
```

## Other commands

You can run the unit test, including doctests, with the following command:
```
pytest --doctest-modules
```
You can build the documentation with:
```
rm ./src/.pytest_cache/README.md 
jupyter-book build -W ./
```
Use `del` instead of rm on Windows


# General Directory Structure

 - The `assets` folder is used for things like hand-drawn figures or other pictures that were not generated from code. These things cannot be easily recreated if they are deleted.

 - The `output` folder, on the other hand, contains tables and figures that are generated from code. The entire folder should be able to be deleted, because the code can be run again, which would again generate all of the contents.

 - I'm using the `doit` Python module as a task runner. It works like `make` and the associated `Makefile`s. To rerun the code, install `doit` (https://pydoit.org/) and execute the command `doit` from the `src` directory. Note that doit is very flexible and can be used to run code commands from the command prompt, thus making it suitable for projects that use scripts written in multiple different programming languages.

 - I'm using the `.env` file as a container for absolute paths that are private to each collaborator in the project. You can also use it for private credentials, if needed. It should not be tracked in Git.

# Data and Output Storage

I'll often use a separate folder for storing data. I usually write code that will pull the data and save it to a directory in the data folder called "pulled"  to let the reader know that anything in the "pulled" folder could hypothetically be deleted and recreated by rerunning the PyDoit command (the pulls are in the dodo.py file).

I'll usually store manually created data in the "assets" folder if the data is small enough. Because of the risk of manually data getting changed or lost, I prefer to keep it under version control if I can.

Output is stored in the "output" directory. This includes tables, charts, and rendered notebooks. When the output is small enough, I'll keep this under version control. I like this because I can keep track of how tables change as my analysis progresses, for example.

Of course, the data directory and output directory can be kept elsewhere on the machine. To make this easy, I always include the ability to customize these locations by defining the path to these directories in environment variables, which I intend to be defined in the `.env` file, though they can also simply be defined on the command line or elsewhere. The `config.py` is reponsible for loading these environment variables and doing some like preprocessing on them. The `config.py` file is the entry point for all other scripts to these definitions. That is, all code that references these variables and others are loading by importing `config`.


# Dependencies and Virtual Environments

## Working with `pip` requirements

`conda` allows for a lot of flexibility, but can often be slow. `pip`, however, is fast for what it does.  You can install the requirements for this project using the `requirements.txt` file specified here. Do this with the following command:
```
pip install -r requirements.txt
```

The requirements file can be created like this:
```
pip list --format=freeze
```

## Working with `conda` environments

The dependencies used in this environment (along with many other environments commonly used in data science) are stored in the conda environment called `blank` which is saved in the file called `environment.yml`. To create the environment from the file (as a prerequisite to loading the environment), use the following command:

```
conda env create -f environment.yml
```

Now, to load the environment, use

```
conda activate blank
```

Note that an environment file can be created with the following command:

```
conda env export > environment.yml
```

However, it's often preferable to create an environment file manually, as was done with the file in this project.

Also, these dependencies are also saved in `requirements.txt` for those that would rather use pip. Also, GitHub actions work better with pip, so it's nice to also have the dependencies listed here. This file is created with the following command:

```
pip freeze > requirements.txt
```

### Alternative Quickstart using Conda
Another way to  run code in this repo is to use the following steps.
First, open a terminal and navigate to the root directory of the project and create a conda environment using the following command:
```
conda env create -f environment.yml
```
Now, load the environment with
```
conda activate blank
```
Now, navigate to the directory called `src`
and run
```
doit
```
That should be it!



**Other helpful `conda` commands**

- Create conda environment from file: `conda env create -f environment.yml`
- Activate environment for this project: `conda activate blank`
- Remove conda environment: `conda remove --name blank --all`
- Create blank conda environment: `conda create --name myenv --no-default-packages`
- Create blank conda environment with different version of Python: `conda create --name myenv --no-default-packages python` Note that the addition of "python" will install the most up-to-date version of Python. Without this, it may use the system version of Python, which will likely have some packages installed already.

## `mamba` and `conda` performance issues

Since `conda` has so many performance issues, it's recommended to use `mamba` instead. I recommend installing the `miniforge` distribution. See here: https://github.com/conda-forge/miniforge









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

