
# 🏠 Tehran Housing Market Time Series Analysis

This repository presents a detailed time series analysis of the **Tehran housing market** using data-driven methods and forecasting techniques. The core of the analysis revolves around **exploratory visualizations** and **Vector Autoregression (VAR)** modeling for **multi-variable prediction**.

---

## 📊 Data Analysis and Visualization

The dataset includes **monthly data** from Tehran’s housing market, featuring the following key indicators:

- `deal_count`: Number of house sale deals
- `sale_price_per_sqm`: Average sale price per square meter
- `rent_price`: Average rent price

The following visualizations were used to explore the structure and relationships within the dataset:

### 📈 Time Series Plots
- **Sale Price per Square Meter over Time**
- **Rent Price over Time**
- **Number of Housing Deals over Time**

Purpose: To examine overall trends, seasonality, and anomalies.

### 📉 Pairwise Relationship Plots
- **Scatter plots**:
  - `sale_price_per_sqm` vs. `rent_price`
  - `sale_price_per_sqm` vs. `deal_count`
  - `rent_price` vs. `deal_count`

- **Pair plot using `seaborn.pairplot`**:
  - Visual inspection of linear/nonlinear relationships and variable distributions.

### 🔁 Differencing Visualization
- Plots of **first-differenced time series** for each variable:
  - To ensure stationarity for VAR modeling.
  - Checked after Augmented Dickey-Fuller tests.

---

## 🧠 Why Use VAR Model?

The **Vector Autoregression (VAR)** model is ideal for this project because:

- The Tehran housing market involves **mutually interdependent variables** (e.g., rent prices influence sale prices and vice versa).
- VAR models all variables **jointly**, unlike univariate models like ARIMA which model one variable in isolation.
- It captures **dynamic interactions**: How the past values of all variables influence the current value of each.

---

## ⚙️ Detailed VAR Modeling Process

### 1. **Stationarity Testing**
- Applied the **Augmented Dickey-Fuller (ADF)** test on:
  - `deal_count`
  - `sale_price_per_sqm`
  - `rent_price`
- Since all series were **non-stationary**, we applied **first-order differencing** to each.

### 2. **Lag Order Selection**
- Used **Akaike Information Criterion (AIC)** and **Bayesian Information Criterion (BIC)** to select optimal lag order.
- Result: Optimal lag selected was **4**.

### 3. **Fitting the VAR Model**
The VAR model was trained using the **differenced** versions of the three time series. It jointly estimated how each variable depends on:

- Its own lagged values (up to lag 4)
- The lagged values of the other two variables

In matrix form, the system looks like:

\[
Y_t = c + A_1 Y_{t-1} + A_2 Y_{t-2} + A_3 Y_{t-3} + A_4 Y_{t-4} + \varepsilon_t
\]

Where:
- \( Y_t = [\Delta \text{deal\_count}, \Delta \text{sale\_price}, \Delta \text{rent\_price}] \)
- \( A_i \) are coefficient matrices learned during training.

### 4. **Granger Causality Tests**
Performed to determine if one variable’s past values **Granger-cause** another. Results showed:
- `sale_price` Granger-causes `rent_price` and vice versa.
- Moderate interaction between `deal_count` and the other variables.

---

## 🔮 Forecasting Results

The trained VAR model was used to **forecast** all three variables for the next **12 months** (1 year). Steps included:

- **Multi-step forecasting** using the `forecast` function on the VAR model
- **Forecast intervals** were computed and plotted
- **Inverse differencing** was applied to return to the original scale

### 📊 Forecast Plots
- **Forecasted Deal Count**
- **Forecasted Sale Price per Square Meter**
- **Forecasted Rent Price**

All forecasts were plotted with:
- Historical values for comparison
- 95% confidence intervals
- X-axis: Monthly timeline
- Y-axis: Forecasted value

---

## 📦 Dependencies

Install the required packages:

```bash
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn
```

---

## 📁 File Structure

```
├── eco.ipynb          # Jupyter Notebook with full analysis, visualization, and modeling
├── README.md          # Project documentation
```

---

## 📈 Key Insights

- **Sale and rent prices** in Tehran are strongly correlated.
- **Housing deal counts** are more volatile but still show seasonal patterns.
- **VAR modeling** enables multi-step forecasting by incorporating the interactions between sale prices, rent prices, and deal volume.
- Results can support **real estate investment decisions**, **policy-making**, or **urban economic research**.

---

