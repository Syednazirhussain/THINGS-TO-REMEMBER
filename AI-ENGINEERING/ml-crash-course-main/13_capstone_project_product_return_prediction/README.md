# Capstone Project: Product Return Prediction

An **end-to-end machine learning project** that predicts which e-commerce orders are likely to be **returned** — a realistic, imbalanced (~30% return rate) binary classification problem.

> This project is created by **Codebasics Inc.**
>
> 🚀 Want to go further? Check out our [**Gen AI & Data Science Bootcamp (with Virtual Internship)**](https://codebasics.io/bootcamps/gen-ai-data-science-bootcamp-with-virtual-internship).

---

## Why this matters

Returns are one of the largest operational costs for an e-commerce marketplace — every returned order incurs reverse logistics, inspection, repackaging, and refund costs. Predicting returns up front lets a business flag high-risk orders and act early.

## What's inside

- **`product_return_prediction.ipynb`** — the full walkthrough: business framing → EDA → cleaning & feature engineering → training and comparing multiple models → evaluation.
- **`dataset/returns_dataset.csv`** — the raw dataset (~12,000 orders, with `returned` as the target).
- **`dataset/cleaned_returns.csv`** — the cleaned/preprocessed version produced during the project.

> **Dataset note:** `returns_dataset.csv` is a programmatically generated demonstration dataset (with a realistic return-rate imbalance and genuine signal in the features). It exists so the full workflow — outlier handling, encoding, scaling, model comparison — can be practiced end to end.

## How to run

From this folder, install the dependencies and launch Jupyter:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn jupyter
jupyter notebook product_return_prediction.ipynb
```

Run the cells top to bottom. The notebook reads the dataset with a **relative path** (`dataset/...`), so run it from inside this folder.

## What it demonstrates

This capstone ties together the skills from the rest of the [ML Crash Course](../README.md):
- Data cleaning, encoding, and feature scaling
- Handling **class imbalance**
- Training and comparing several classifiers
- Evaluating with the **classification report** and **confusion matrix**, not just accuracy
