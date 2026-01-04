# Options-Implied Probability Surface (2D + 3D)

## Overview
This project extracts **market-implied probability distributions** for a stock’s future price directly from option prices and visualizes them as both a **2D heatmap** and a **3D surface**. Instead of forecasting prices or fitting a parametric model, it uses a no-arbitrage result from options theory to infer where the market is implicitly assigning probability mass at different maturities.

In plain English: we reverse-engineer the options market to see **where traders are paying for outcomes**, not where someone claims the stock will go.

---

## What the script does
For each option expiry:
1. **Collects call and put quotes** from Yahoo Finance.
2. **Uses mid-market prices when available** (bid/ask midpoint), falling back to last trade only if necessary.
3. **Stitches the cleanest side of the market**:
   - Below the forward price → use **puts** (converted to calls via put-call parity)
   - Above the forward price → use **calls**
   - Smoothly blends near the forward to avoid artificial kinks.
4. **Fits a smooth curve** to option prices across strikes.
5. **Takes the second derivative with respect to strike** (Breeden–Litzenberger) to extract the **risk-neutral probability density** of the terminal stock price.
6. **Repeats across maturities**, aligns everything on a common price grid, and converts densities into interpretable probability mass.
7. **Visualizes the result** as:
   - A 2D heatmap (time × terminal price)
   - A 3D wireframe surface (time × terminal price × probability mass)
