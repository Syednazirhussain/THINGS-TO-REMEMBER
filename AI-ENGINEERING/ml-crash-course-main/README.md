# Machine Learning Crash Course

A hands-on, notebook-by-notebook introduction to **classical machine learning with Python and scikit-learn**. Each folder is a self-contained lesson that builds on the previous one — from your first linear regression through model tuning, cross-validation, clustering, and a final end-to-end capstone project.

Every lesson uses a small, real (or realistic) dataset, keeps the code readable, and focuses on **one idea at a time**. Most lessons come with a companion **exercise** (a problem-statement notebook to try yourself, plus a worked solution).

> This course is created by **Codebasics Inc.**
>
> 🚀 Want to go further? Check out our [**Gen AI & Data Science Bootcamp (with Virtual Internship)**](https://codebasics.io/bootcamps/gen-ai-data-science-bootcamp-with-virtual-internship).

---

## What you'll learn

- Supervised learning: **regression** (linear) and **classification** (logistic, SVM, decision tree, random forest)
- The core workflow: **train/test split**, **data cleaning**, feature **scaling**, and reading a **classification report** / **confusion matrix**
- Why models fail: **overfitting vs. underfitting** and how to spot it
- Trusting your results: **K-Fold & Stratified cross-validation**
- Getting the most from a model: **hyperparameter tuning with GridSearchCV**
- Unsupervised learning: **K-Means clustering** and the elbow method
- Putting it all together in an **end-to-end capstone project**

---

## Course outline

| # | Topic | Notebook | Dataset(s) | Exercise |
|---|-------|----------|------------|:--------:|
| 1 | Linear Regression | `1_linear_regression/linear_regression_crash_course.ipynb` | delivery time | ✅ (salary) |
| 2 | Train / Test Split | `2_train_test_split/train_test_split_crash_course.ipynb` | delivery time | — |
| 3 | Logistic Regression (binary) | `3_logistic_regression/logistic_regression_crash_course.ipynb` | insurance | — |
| 4 | Logistic Regression (multiclass) | `4_logistic_multiclass/logistic_multiclass.ipynb` | — | ✅ (t-shirt sizes) |
| 5 | Data Cleaning | `5_data_cleaning/data_cleaning_crash_course.ipynb` | loan data | — |
| 6 | Support Vector Machine (SVM) | `6_svm/svm_heart_disease.ipynb` | heart disease | — |
| 7 | Overfitting vs. Underfitting | `7_overfitting_underfitting/overfitting_underfitting.ipynb` | synthetic (make_moons) | — |
| 8 | Decision Tree | `8_decision_tree/decision_tree.ipynb` | heart disease | ✅ (breast cancer) |
| 9 | Random Forest | `9_random_forest/random_forest.ipynb` | Titanic | ✅ (wine) |
| 10 | Cross-Validation (K-Fold / Stratified) | `10_cross_validation/cross_validation_tutorial.ipynb` | heart disease | ✅ (digits) |
| 11 | Hyperparameter Tuning (GridSearchCV) | `11_hyperparam_tuning/hyperparam_tuning_gridsearchcv.ipynb` | iris | ✅ (breast cancer) |
| 12 | K-Means Clustering | `12_k_means/kmeans_tutorial.ipynb` | income | ✅ (iris) |
| 13 | Capstone: Product Return Prediction | `13_capstone_project_product_return_prediction/product_return_prediction.ipynb` | returns dataset | — |

> A recommended path is to go in order (1 → 13). Later lessons assume the ideas from earlier ones — e.g. cross-validation (10) builds on overfitting (7), and hyperparameter tuning (11) builds on cross-validation (10).

---

## Getting started

### 1. Prerequisites
- **Python 3.9+**
- Basic Python and a little pandas help, but each notebook explains as it goes.

### 2. Install the dependencies

```bash
pip install numpy pandas matplotlib seaborn scikit-learn jupyter
```

### 3. Launch Jupyter and open a lesson

```bash
jupyter notebook
```

Then open any folder's notebook and run the cells top to bottom. Each notebook reads its dataset with a **relative path**, so run it from inside its own folder (or keep the folder structure intact).

---

## How the lessons are structured

Each **lesson notebook** follows the same friendly arc:
1. A short intro explaining the idea and a real-world framing
2. Load and inspect the data
3. Clean / preprocess (only what's needed)
4. Train the model
5. Evaluate it (accuracy, classification report, confusion matrix, or plots)
6. A **"What we learned"** recap and a **"Try next"** nudge

Each **exercise** folder contains two notebooks:
- `exercise_problem_statement.ipynb` — a scaffolded notebook with tasks and hints (code left for you to fill in)
- `exercise_solution.ipynb` — the fully worked solution with explanations

Try the problem statement first, then check your work against the solution.

---

## The capstone project

`13_capstone_project_product_return_prediction/` is an **end-to-end project** that ties the course together: predicting which e-commerce orders will be **returned** (a realistic ~30% imbalanced classification problem). It walks through the business problem, EDA, cleaning and feature engineering, training and comparing several models, and evaluating them properly — the full workflow you'd follow on a real task.

---

## Tech stack

- **[scikit-learn](https://scikit-learn.org/)** — models, preprocessing, model selection
- **pandas** / **numpy** — data handling
- **matplotlib** / **seaborn** — visualization
- **Jupyter** — interactive notebooks

Happy learning! 🚀
