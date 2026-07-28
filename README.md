# Uplift Modeling for Personalized Marketing Campaigns

## Project Overview

This project demonstrates **causal machine learning for uplift modeling** using the **Criteo Uplift Dataset**. Unlike traditional predictive models that estimate the probability of conversion, uplift models estimate the **incremental impact of a treatment** (such as displaying an advertisement) on individual users.

The notebook walks through the complete workflow—from exploratory analysis and causal effect estimation to implementing multiple meta-learners for estimating the **Conditional Average Treatment Effect (CATE)** and evaluating their performance.

---

## Business Impact

Blanket ad targeting wastes spend on customers who would have converted anyway ("sure things") and on those who will never convert regardless of exposure ("lost causes" and "sleeping dogs"). Uplift modeling isolates the segment that only converts **because** they were targeted — the *persuadable* customers — and ranks the population by that incremental effect.

**Headline result:** the best model (X-Learner) achieved a Qini coefficient of **0.069**, roughly 6.9x higher than the random-targeting baseline (0.01) — meaning it identifies persuadable customers far more effectively than chance, even though the absolute value looks small at first glance.

---

## Problem Statement

Marketing campaigns are expensive, and showing advertisements to every customer is rarely optimal.

The objective is to identify users who are **most likely to change their behavior because of an advertisement**, enabling targeted campaigns that maximize return on investment.

Instead of predicting:

> *Will a customer convert?*

this project answers:

> *Will showing an advertisement increase this customer's probability of converting?*

---

## Dataset

**Dataset:** Criteo Uplift Modeling Dataset

The dataset contains:

* Treatment assignment
* Advertisement exposure
* Website visits
* Conversion outcome
* 12 customer features (`f0`–`f11`)

The treatment is randomly assigned, making the dataset suitable for causal inference.

---

## Project Workflow

### 1. Exploratory Data Analysis

* Dataset inspection
* Treatment vs control distribution
* Advertisement exposure analysis
* Visit and conversion rates
* Treatment effectiveness analysis
* Feature correlation analysis

---

### 2. Intent-to-Treat (ITT) Analysis

Estimated the causal effect of assigning advertisements by comparing treatment and control groups.

The notebook computes:

* Treatment conversion rate
* Control conversion rate
* Intent-to-Treat effect

---

### 3. Average Treatment Effect (ATE)

Estimated the average causal impact of treatment across the population.

---

### 4. Conditional Average Treatment Effect (CATE)

Estimated heterogeneous treatment effects using customer features.

This enables personalized targeting by estimating:

$CATE(x)=E[Y(1)-Y(0)\mid X=x]$

where

* $Y(1)$ = outcome if treated
* $Y(0)$ = outcome if untreated

---

## Models Implemented

The notebook implements several meta-learning approaches for uplift modeling.

### T-Learner

* Separate models for treatment and control groups
* Estimates uplift as the difference between predicted outcomes

---

### X-Learner

* Learns imputed treatment effects
* Particularly effective when treatment and control groups are imbalanced
* Uses LightGBM as the base learner

---

### R-Learner

* Rewrites the observed outcome with nuisance functions, to remove the main effect of the covariates, leaving only the treatment-effect signal plus noise.
* Answers - What value of $\tau(X)$ best explains the residualized outcome using the residualized treatment?
* solves for residual-on-residual regression.

---

## Causal Gradient Boosting (XGBoost)

Implemented a tree-based causal boosting model using **XGBoost** as the base learner to estimate heterogeneous treatment effects.

Features:

* Gradient-boosted decision trees
* Nonlinear treatment effect estimation
* Handles complex feature interactions
* High predictive performance on structured datasets

---
# Model Evaluation

Unlike traditional classification tasks, uplift models require specialized evaluation metrics.

The project evaluates each causal learner using:

* Average Treatment Effect (ATE)
* Conditional Average Treatment Effect (CATE)
* Individual Uplift Scores
* Qini Curve
* Qini Coefficient
* Area Under the Uplift Curve (AUUC)

These metrics measure how effectively each model identifies customers who benefit from treatment.

---

# Model Comparison

A comparative analysis is performed across multiple causal learners based on:

* Treatment effect estimation
* Individual uplift prediction
* AUUC
* Qini coefficient

The comparison highlights the strengths and limitations of different causal machine learning approaches for uplift modeling.

---

## Technologies Used

* Python
* Pandas
* NumPy
* Scikit-learn
* LightGBM
* Matplotlib
* Seaborn
* EconML

---

## Results & Evaluation Metrics

### Model Comparison

| Model | Qini Coefficient | AUUC | Notes |
|---|---|---|---|
| T-Learner | 0.01 | 0.009 | Baseline meta-learner; two independent models |
| **X-Learner** | 0.069 | 0.027 | LightGBM base learner; handles treatment/control imbalance |
| R-Learner | 0.067 | 0.024 | Residual-on-residual regression |
| Causal GBM (XGBoost) | 0.055 | 0.021 | Tree based CATE |

### Qini Curve

<img width="365" height="286" alt="image" src="https://github.com/user-attachments/assets/3b907357-dc8f-4ccb-a897-5499031c0075" />

<img width="365" height="278" alt="image" src="https://github.com/user-attachments/assets/9b75b543-feda-4b04-a876-26199bd1df31" />

This distinction enables more efficient marketing campaigns by targeting only customers who are expected to respond positively to an intervention, reducing unnecessary advertising costs and improving campaign ROI.
