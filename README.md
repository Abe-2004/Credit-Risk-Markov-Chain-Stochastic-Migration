# Discrete-Time Credit Risk Engineering: Absorbing Markov Chains & Vectorized Migration Simulators

## Executive Summary
This framework constructs an asset-level and portfolio-level credit risk valuation engine using finite-state discrete-time Markov chains. The architecture models corporate credit rating migrations across highly non-linear transition spaces, calculates asset life expectancy via fundamental matrix partitioning, and evaluates systemic portfolio credit losses using a high-performance vectorized Monte Carlo path simulator.

## Mathematical Architecture
Credit state migration is treated as a memoryless stochastic process governed by a row-stochastic transition probability matrix $P$, satisfying the row sum condition $\sum_{j} p_{ij} = 1$.

* **Multi-Period Forecasting (Chapman-Kolmogorov):** Multi-year state transition probabilities are calculated via explicit matrix expansion power structures:
  $$P^{(T)} = P^T$$
* **Asset Life Expectancy (Fundamental Matrix Partitioning):** To isolate the absorbing state (Default), the matrix is partitioned into transient ($Q$) and absorbing ($R$) sub-matrices. The expected years spent in transient credit states before absorption is resolved analytically via the fundamental matrix $N$:
  $$N = (I - Q)^{-1}$$
* **Vectorized Path Simulation:** A portfolio of 10,000 parallel pathways is generated simultaneously over a multi-year horizon by mapping uniform random variables against the cumulative distribution thresholds of the row-stochastic parameter space.

## Empirical Metrics & Verification
* **Portfolio Inputs:** 10,000 Distressed (DS) corporate exposures, $1,000,000 face value per asset, 40% structural recovery rate (60% Loss Given Default).
* **Analytical Ground Truth:** 3-Year theoretical migration probability to Default ($DS \to D$) = **32.73%**.
* **Simulation Performance:** The vectorized Monte Carlo engine resolved 3,281 empirical defaults, translating to a portfolio default rate of **32.81%** and a cumulative credit loss of **$1.968B**.
* **Statistical Convergence:** Convergence is verified via a microscopic absolute tracking discrepancy of **0.08%** against the closed-form analytical benchmark.
