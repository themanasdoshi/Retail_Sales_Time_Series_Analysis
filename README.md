
# 🛍️ Retail Sales Time Series Analysis & ARIMA Forecasting

This project performs a time-series analysis and forecasting on a retail sales dataset. 
The goal is to prepare the raw sales data, conduct exploratory data analysis (EDA) to understand trends, check for stationarity, and build an ARIMA model to forecast future sales based on historical monthly patterns.

## 📝 Workflow Summary

The analysis follows a clear three-step process:

* **Preparation & Cleaning 🧹**
* **Exploratory Data Analysis (EDA) 📊**
* **Time-Series Forecasting 🔮**

---

## 1. Preparation & Cleaning 🧹

The initial steps focus on making the raw data suitable for time-series modeling.

* **Data Ingestion:** The `sales_data_sample.csv` file is loaded into a pandas DataFrame.
* **Initial Audit:** The dataset contains **2823 entries** and **25 columns**. The `ORDERDATE` column is initially of `object` (string) type.
* **Standardization:** Column names are cleaned by converting them to **lowercase** and replacing spaces with **underscores** (e.g., `ORDERNUMBER` becomes `ordernumber`).
* **Type Conversion:** The `orderdate` column is successfully converted to the `datetime64[ns]` format.
* **Missing Data Handling:** Columns with a significant number of missing values (`addressline2`, `state`, `territory`, `postalcode`) were identified and subsequently **dropped** from the DataFrame.

---

## 2. Exploratory Data Analysis (EDA) 📊

EDA helps in understanding the data distribution, key segments, and general sales trends.

### Descriptive Statistics

The statistical summary of the numerical columns reveals:

* **Sales:** The mean sale amount is approximately **$3,553.89**, with a wide range from a minimum of **$482.13** to a maximum of **$14,082.80**.
* **Quantity Ordered:** The average quantity ordered is **35.09** units, with a standard deviation of **9.74**.
* **Order Dates:** Sales span from **January 6, 2003**, to **May 31, 2005**.

### Key Sales Trends Visualized

1.  **Total Monthly Sales Over Time:**
    * The time series plot shows a clear **seasonal pattern**.
    * Sales typically peak significantly towards the end of the year, likely in **November** (Month 11), suggesting a strong holiday season effect.
2.  **Total Sales by Product Line:**
    * **Classic Cars** are the dominant revenue generator, followed by **Vintage Cars**.
    * Product lines like **Planes** and **Trucks and Buses** are secondary contributors, while **Train** sales are the lowest.
3.  **Proportion of Sales by Deal Size:**
    * **Medium** deals account for the largest proportion of total sales, at **50.6%**.
    * **Small** deals represent **37.7%**.
    * **Large** deals contribute **11.7%** of the total revenue.

### Correlation Analysis

The correlation matrix of numerical variables highlights relationships:

* **Sales and Quantity Ordered:** A **strong positive correlation** (**0.92**) exists, which is expected, as higher quantities ordered directly result in higher total sales.
* **Price Each and Sales:** A **moderate positive correlation** (**0.79**) suggests that more expensive items contribute significantly to the total sales value.
* **MSRP and Price Each:** A **very strong positive correlation** (**0.93**) confirms that the actual price paid is tightly linked to the suggested retail price.

---

## 3. Time-Series Forecasting 🔮

This section focuses on preparing the time series for an ARIMA model and performing the forecast.

### Stationarity Check (ADF Test)

The Augmented Dickey-Fuller (ADF) test is performed on the monthly sales data to check for stationarity, a core requirement for ARIMA modeling.

| Statistic | Value |
| :--- | :--- |
| **ADF Statistic** | -3.63 |
| **p-value** | 0.0052 |
| **Critical Values (5%)** | -2.97 |

**Result:** 
The p-value (**0.0052**) is **less than the significance level of 0.05**. This allows us to **reject the null hypothesis of non-stationarity**. 
The data is determined to be **likely stationary**, meaning differencing (**d=0**) is not immediately required for the ARIMA model.

### Model Identification (ACF & PACF)

ACF and PACF plots are generated to help identify the initial parameters for the ARIMA model.

* **ACF Plot (q parameter - Moving Average):** A drop-off or sudden cut-off at Lag 1 suggests the MA order **(q)** could be **1**.
* **PACF Plot (p parameter - Autoregressive):** A cut-off after Lag 1 suggests the AR order **(p)** could be **1**.
* **Differencing (d parameter - Integrated):** Confirmed by the ADF test, the series is stationary, so **d=0**.

**Initial Model Parameters:** Based on these plots and the ADF test, the ARIMA model parameters are tentatively set to **ARIMA(1, 0, 1)**.

### Model Building and Evaluation

* **Model Training:** An **ARIMA(1, 0, 1)** model is trained on the first **80%** of the monthly sales data.
* **Forecasting & Visualization:** The model is then used to forecast sales for the remaining **20%** (the test period). The results are plotted against the actual test data.

**Visualization Observations:** The forecasted sales line generally follows the trend of the actual sales data in the test period and provides a reasonably good prediction.
