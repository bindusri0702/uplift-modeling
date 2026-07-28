# Uplift Modeling for Personalized Marketing Campaigns

## Project Overview

This project demonstrates **causal machine learning for uplift modeling** using the **Criteo Uplift Dataset**. Unlike traditional predictive models that estimate the probability of conversion, uplift models estimate the **incremental impact of a treatment** (such as displaying an advertisement) on individual users.

The notebook walks through the complete workflow—from exploratory analysis and causal effect estimation to implementing multiple meta-learners for estimating the **Conditional Average Treatment Effect (CATE)** and evaluating their performance.

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

## Evaluation Metrics

The project evaluates causal effects using:

* Intent-to-Treat (ITT)
* Average Treatment Effect (ATE)
* Conditional Average Treatment Effect (CATE)

The notebook also demonstrates uplift estimation for individual users.

---

This distinction enables more efficient marketing campaigns by targeting only customers who are expected to respond positively to an intervention, reducing unnecessary advertising costs and improving campaign ROI.
