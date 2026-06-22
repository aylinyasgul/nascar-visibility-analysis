# NASCAR Sponsorship Visibility Analysis

## Project Overview
Analysis of NASCAR sponsorship visibility metrics for NY Racing.

## Environment Setup
1. Install Anaconda or Python 3.11+
2. Create environment: `conda create -n nascar-visibility python=3.11`
3. Activate: `conda activate nascar-visibility`
4. Install packages: `pip install pandas numpy matplotlib seaborn jupyter beautifulsoup4 requests praw google-api-python-client`

## Folder Structure
- `data/raw/` - Original data files
- `data/processed/` - Cleaned data
- `code/` - Analysis scripts and notebooks
- `output/` - Generated charts and reports

## Data Sources
- Race results: Kaggle NASCAR dataset
- Social media: Reddit (PRAW), YouTube Data API
- News: Manual tracking 