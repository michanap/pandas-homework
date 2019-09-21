
 #  A Whale off the Port(folio)

 In this assignment, you'll get to use what you've learned this week to evaluate the performance among various algorithmic, hedge, and mutual fund portfolios and compare them against the S&P 500.


```python
import pandas as pd
import numpy as np
import datetime as dt
from pathlib import Path
%matplotlib inline
```

# Data Cleaning

In this section, you will need to read the CSV files into DataFrames and perform any necessary data cleaning steps. After cleaning, combine all DataFrames into a single DataFrame.

Files:
1. whale_returns.csv
2. algo_returns.csv
3. sp500_history.csv

## Whale Returns

Read the Whale Portfolio daily returns and clean the data


```python
# Reading whale returns
whale_returns_csv = Path("../Resources/whale_returns.csv")
# YOUR CODE HERE
whale_returns = pd.read_csv(whale_returns_csv, index_col= 'Date', infer_datetime_format=True, parse_dates=True)
whale_returns.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SOROS FUND MANAGEMENT LLC</th>
      <th>PAULSON &amp; CO.INC.</th>
      <th>TIGER GLOBAL MANAGEMENT LLC</th>
      <th>BERKSHIRE HATHAWAY INC</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-03-02</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2015-03-03</th>
      <td>-0.001266</td>
      <td>-0.004981</td>
      <td>-0.000496</td>
      <td>-0.006569</td>
    </tr>
    <tr>
      <th>2015-03-04</th>
      <td>0.002230</td>
      <td>0.003241</td>
      <td>-0.002534</td>
      <td>0.004213</td>
    </tr>
    <tr>
      <th>2015-03-05</th>
      <td>0.004016</td>
      <td>0.004076</td>
      <td>0.002355</td>
      <td>0.006726</td>
    </tr>
    <tr>
      <th>2015-03-06</th>
      <td>-0.007905</td>
      <td>-0.003574</td>
      <td>-0.008481</td>
      <td>-0.013098</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Count nulls
# YOUR CODE HERE
whale_returns.isnull().sum()
```




    SOROS FUND MANAGEMENT LLC      1
    PAULSON & CO.INC.              1
    TIGER GLOBAL MANAGEMENT LLC    1
    BERKSHIRE HATHAWAY INC         1
    dtype: int64




```python
# Drop nulls
# YOUR CODE HERE
whale_returns.dropna(inplace=True)
whale_returns.isnull().sum()
```




    SOROS FUND MANAGEMENT LLC      0
    PAULSON & CO.INC.              0
    TIGER GLOBAL MANAGEMENT LLC    0
    BERKSHIRE HATHAWAY INC         0
    dtype: int64



## Algorithmic Daily Returns

Read the algorithmic daily returns and clean the data


```python
# Reading algorithmic returns
algo_returns_csv = Path("../Resources/algo_returns.csv")
# YOUR CODE HERE
algo_returns = pd.read_csv(algo_returns_csv, index_col= 'Date',infer_datetime_format=True, parse_dates=True)
algo_returns.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Algo 1</th>
      <th>Algo 2</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2014-05-28</th>
      <td>0.001745</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2014-05-29</th>
      <td>0.003978</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2014-05-30</th>
      <td>0.004464</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2014-06-02</th>
      <td>0.005692</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2014-06-03</th>
      <td>0.005292</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Count nulls
# YOUR CODE HERE
algo_returns.isnull().sum()
```




    Algo 1    0
    Algo 2    6
    dtype: int64




```python
# Drop nulls
# YOUR CODE HERE
algo_returns.dropna(inplace=True)
algo_returns.isnull().sum()
```




    Algo 1    0
    Algo 2    0
    dtype: int64



## S&P 500 Returns

Read the S&P500 Historic Closing Prices and create a new daily returns DataFrame from the data. 


```python
# Reading S&P 500 Closing Prices
sp500_history_csv = Path("../Resources/sp500_history.csv")
# YOUR CODE HERE
sp500_history = pd.read_csv(sp500_history_csv, index_col= 'Date',infer_datetime_format=True, parse_dates=True)
sp500_history.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Close</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-04-23</th>
      <td>$2933.68</td>
    </tr>
    <tr>
      <th>2019-04-22</th>
      <td>$2907.97</td>
    </tr>
    <tr>
      <th>2019-04-18</th>
      <td>$2905.03</td>
    </tr>
    <tr>
      <th>2019-04-17</th>
      <td>$2900.45</td>
    </tr>
    <tr>
      <th>2019-04-16</th>
      <td>$2907.06</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Check Data Types
# YOUR CODE HERE
sp500_history.dtypes
```




    Close    object
    dtype: object




```python
# Fix Data Types
# YOUR CODE HERE
sp500_history["Close"] = sp500_history["Close"].str.replace("$", " ")
sp500_history["Close"] = sp500_history["Close"].astype("float")
sp500_history.dtypes
```




    Close    float64
    dtype: object




```python
# Calculate Daily Returns
# YOUR CODE HERE
daily_returns = sp500_history.pct_change()
daily_returns.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Close</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-04-23</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-04-22</th>
      <td>-0.008764</td>
    </tr>
    <tr>
      <th>2019-04-18</th>
      <td>-0.001011</td>
    </tr>
    <tr>
      <th>2019-04-17</th>
      <td>-0.001577</td>
    </tr>
    <tr>
      <th>2019-04-16</th>
      <td>0.002279</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Drop nulls
# YOUR CODE HERE
daily_returns.dropna(inplace=True)
daily_returns.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Close</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-04-22</th>
      <td>-0.008764</td>
    </tr>
    <tr>
      <th>2019-04-18</th>
      <td>-0.001011</td>
    </tr>
    <tr>
      <th>2019-04-17</th>
      <td>-0.001577</td>
    </tr>
    <tr>
      <th>2019-04-16</th>
      <td>0.002279</td>
    </tr>
    <tr>
      <th>2019-04-15</th>
      <td>-0.000509</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Rename Column
# YOUR CODE HERE
daily_returns.columns=['S&P 500']
daily_returns.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>S&amp;P 500</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-04-22</th>
      <td>-0.008764</td>
    </tr>
    <tr>
      <th>2019-04-18</th>
      <td>-0.001011</td>
    </tr>
    <tr>
      <th>2019-04-17</th>
      <td>-0.001577</td>
    </tr>
    <tr>
      <th>2019-04-16</th>
      <td>0.002279</td>
    </tr>
    <tr>
      <th>2019-04-15</th>
      <td>-0.000509</td>
    </tr>
  </tbody>
</table>
</div>



## Combine Whale, Algorithmic, and S&P 500 Returns


```python
# Concatenate all DataFrames into a single DataFrame
# YOUR CODE HERE
combined_data = pd.concat([whale_returns,algo_returns,daily_returns], axis='columns', join='inner')
combined_data.head()                   
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SOROS FUND MANAGEMENT LLC</th>
      <th>PAULSON &amp; CO.INC.</th>
      <th>TIGER GLOBAL MANAGEMENT LLC</th>
      <th>BERKSHIRE HATHAWAY INC</th>
      <th>Algo 1</th>
      <th>Algo 2</th>
      <th>S&amp;P 500</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-03-03</th>
      <td>-0.001266</td>
      <td>-0.004981</td>
      <td>-0.000496</td>
      <td>-0.006569</td>
      <td>-0.001942</td>
      <td>-0.000949</td>
      <td>0.004408</td>
    </tr>
    <tr>
      <th>2015-03-04</th>
      <td>0.002230</td>
      <td>0.003241</td>
      <td>-0.002534</td>
      <td>0.004213</td>
      <td>-0.008589</td>
      <td>0.002416</td>
      <td>-0.001195</td>
    </tr>
    <tr>
      <th>2015-03-05</th>
      <td>0.004016</td>
      <td>0.004076</td>
      <td>0.002355</td>
      <td>0.006726</td>
      <td>-0.000955</td>
      <td>0.004323</td>
      <td>0.014378</td>
    </tr>
    <tr>
      <th>2015-03-06</th>
      <td>-0.007905</td>
      <td>-0.003574</td>
      <td>-0.008481</td>
      <td>-0.013098</td>
      <td>-0.004957</td>
      <td>-0.011460</td>
      <td>-0.003929</td>
    </tr>
    <tr>
      <th>2015-03-09</th>
      <td>0.000582</td>
      <td>0.004225</td>
      <td>0.005843</td>
      <td>-0.001652</td>
      <td>-0.005447</td>
      <td>0.001303</td>
      <td>0.017254</td>
    </tr>
  </tbody>
</table>
</div>



---

# Portfolio Analysis

In this section, you will calculate and visualize performance and risk metrics for the portfolios.

## Performance

Calculate and Plot the daily returns and cumulative returns. 


```python
# Plot daily returns
# YOUR CODE HERE
combined_data.plot(figsize=(20,10), title='Daily Returns')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11da1f5f8>




![png](output_23_1.png)



```python
# Plot cumulative returns
# YOUR CODE HERE
cumulative_returns = (1 + combined_data).cumprod() -1
cumulative_returns.plot(figsize=(20,10), title='Cumulative Returns')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11cd69860>




![png](output_24_1.png)


---

## Risk

Determine the _risk_ of each portfolio:

1. Create a box plot for each portfolio. 
2. Calculate the standard deviation for all portfolios
4. Determine which portfolios are riskier than the S&P 500
5. Calculate the Annualized Standard Deviation


```python
# Box plot to visually show risk
# YOUR CODE HERE
combined_data.plot.box(figsize=(20,10), title='Portfolio Risk')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11ec21128>




![png](output_27_1.png)



```python
# Daily Standard Deviations
# Calculate the standard deviation for each portfolio. Which portfolios are riskier than the S&P 500?
# YOUR CODE HERE
daily_std = combined_data.std()
daily_std.head(7)
```




    SOROS FUND MANAGEMENT LLC      0.007896
    PAULSON & CO.INC.              0.007026
    TIGER GLOBAL MANAGEMENT LLC    0.010897
    BERKSHIRE HATHAWAY INC         0.012919
    Algo 1                         0.007623
    Algo 2                         0.008341
    S&P 500                        0.008587
    dtype: float64




```python
# Determine which portfolios are riskier than the S&P 500
# YOUR CODE HERE
# TIGER GLOBAL MANAGEMENT LLC & BERKSHIRE HATHAWAY INC risker than S&P

daily_std['S&P 500'] <  daily_std
```




    SOROS FUND MANAGEMENT LLC      False
    PAULSON & CO.INC.              False
    TIGER GLOBAL MANAGEMENT LLC     True
    BERKSHIRE HATHAWAY INC          True
    Algo 1                         False
    Algo 2                         False
    S&P 500                        False
    dtype: bool




```python
# Calculate the annualized standard deviation (252 trading days)
# YOUR CODE HERE
annualized_std = daily_std * np.sqrt(252)
annualized_std.head(7)
```




    SOROS FUND MANAGEMENT LLC      0.125348
    PAULSON & CO.INC.              0.111527
    TIGER GLOBAL MANAGEMENT LLC    0.172989
    BERKSHIRE HATHAWAY INC         0.205079
    Algo 1                         0.121006
    Algo 2                         0.132413
    S&P 500                        0.136313
    dtype: float64



---

## Rolling Statistics

Risk changes over time. Analyze the rolling statistics for Risk and Beta. 

1. Calculate and plot the rolling standard deviation for the S&PP 500 using a 21 day window
2. Calcualte the correlation between each stock to determine which portfolios may mimick the S&P 500
2. Calculate and plot a 60 day Beta for Berkshire Hathaway Inc compared to the S&&P 500


```python
# Calculate and plot the rolling standard deviation for the S&PP 500 using a 21 day window
# YOUR CODE HERE
combined_data.rolling(window=21).std().plot(figsize=(20,10), title='21 Day Rolling Standard Deviation')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11f323dd8>




![png](output_33_1.png)



```python
# Correlation
# YOUR CODE HERE
correlation = combined_data.corr()
correlation
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SOROS FUND MANAGEMENT LLC</th>
      <th>PAULSON &amp; CO.INC.</th>
      <th>TIGER GLOBAL MANAGEMENT LLC</th>
      <th>BERKSHIRE HATHAWAY INC</th>
      <th>Algo 1</th>
      <th>Algo 2</th>
      <th>S&amp;P 500</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>SOROS FUND MANAGEMENT LLC</th>
      <td>1.000000</td>
      <td>0.699823</td>
      <td>0.561040</td>
      <td>0.754157</td>
      <td>0.320901</td>
      <td>0.826730</td>
      <td>0.000574</td>
    </tr>
    <tr>
      <th>PAULSON &amp; CO.INC.</th>
      <td>0.699823</td>
      <td>1.000000</td>
      <td>0.434308</td>
      <td>0.545451</td>
      <td>0.268631</td>
      <td>0.678085</td>
      <td>0.013549</td>
    </tr>
    <tr>
      <th>TIGER GLOBAL MANAGEMENT LLC</th>
      <td>0.561040</td>
      <td>0.434308</td>
      <td>1.000000</td>
      <td>0.424125</td>
      <td>0.164114</td>
      <td>0.507160</td>
      <td>-0.001505</td>
    </tr>
    <tr>
      <th>BERKSHIRE HATHAWAY INC</th>
      <td>0.754157</td>
      <td>0.545451</td>
      <td>0.424125</td>
      <td>1.000000</td>
      <td>0.291678</td>
      <td>0.687756</td>
      <td>-0.013856</td>
    </tr>
    <tr>
      <th>Algo 1</th>
      <td>0.320901</td>
      <td>0.268631</td>
      <td>0.164114</td>
      <td>0.291678</td>
      <td>1.000000</td>
      <td>0.287852</td>
      <td>-0.033963</td>
    </tr>
    <tr>
      <th>Algo 2</th>
      <td>0.826730</td>
      <td>0.678085</td>
      <td>0.507160</td>
      <td>0.687756</td>
      <td>0.287852</td>
      <td>1.000000</td>
      <td>-0.002192</td>
    </tr>
    <tr>
      <th>S&amp;P 500</th>
      <td>0.000574</td>
      <td>0.013549</td>
      <td>-0.001505</td>
      <td>-0.013856</td>
      <td>-0.033963</td>
      <td>-0.002192</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate Beta for a single portfolio compared to the total market (S&P 500)
# YOUR CODE HERE
covariance = combined_data['BERKSHIRE HATHAWAY INC'].cov(combined_data['S&P 500'])
covariance

variance = combined_data['S&P 500'].var()
variance

Algo1_beta = covariance / variance
Algo1_beta

rolling_covariance = combined_data['BERKSHIRE HATHAWAY INC'].rolling(window=90).cov(combined_data['S&P 500'])
rolling_variance = combined_data['S&P 500'].rolling(window=90).var()
rolling_beta = rolling_covariance / rolling_variance
rolling_beta.plot(figsize=(20, 10), title='Rolling 90-Day Beta of BERKSHIRE HATHAWAY INC')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11f64c908>




![png](output_35_1.png)


### Challenge: Exponentially Weighted Average 

An alternative way to calculate a rollwing window is to take the exponentially weighted moving average. This is like a moving window average, but it assigns greater importance to more recent observations. Try calculating the `ewm` with a 21 day half-life.


```python
# (OPTIONAL) YOUR CODE HERE
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11e8560f0>




![png](output_37_1.png)


---

## Sharpe Ratios
In reality, investment managers and thier institutional investors look at the ratio of return-to-risk, and not just returns alone. (After all, if you could invest in one of two portfolios, each offered the same 10% return, yet one offered lower risk, you'd take that one, right?)

Calculate and plot the annualized Sharpe ratios for all portfolios to determine which portfolio has the best performance


```python
# Annualzied Sharpe Ratios
# YOUR CODE HERE
sharpe_ratios = (combined_data.mean() * 252) / (combined_data.std() * np.sqrt(252))
sharpe_ratios
```




    SOROS FUND MANAGEMENT LLC      0.342894
    PAULSON & CO.INC.             -0.491422
    TIGER GLOBAL MANAGEMENT LLC   -0.130186
    BERKSHIRE HATHAWAY INC         0.606743
    Algo 1                         1.369589
    Algo 2                         0.484334
    S&P 500                       -0.518582
    dtype: float64



 plot() these sharpe ratios using a barplot.
 On the basis of this performance metric, do our algo strategies outperform both 'the market' and the whales?


```python
# Visualize the sharpe ratios as a bar plot
# YOUR CODE HERE
sharpe_ratios.plot(kind='bar',title='Sharpe Ratios')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11fe91278>




![png](output_42_1.png)


---

# Portfolio Returns

In this section, you will build your own portfolio of stocks, calculate the returns, and compare the results to the Whale Portfolios and the S&P 500. 

1. Choose 3-5 custom stocks with at last 1 year's worth of historic prices and create a DataFrame of the closing prices and dates for each stock.
2. Calculate the weighted returns for the portfolio assuming an equal number of shares for each stock
3. Join your portfolio returns to the DataFrame that contains all of the portfolio returns
4. Re-run the performance and risk analysis with your portfolio to see how it compares to the others
5. Include correlation analysis to determine which stocks (if any) are correlated

## Choose 3-5 custom stocks with at last 1 year's worth of historic prices and create a DataFrame of the closing prices and dates for each stock.


```python
# Read the first stock
# YOUR CODE HERE
goog_historical_csv = Path("../Resources/goog_historical.csv")
goog_historical = pd.read_csv(goog_historical_csv, index_col= 'Trade DATE', infer_datetime_format=True, parse_dates=True)
goog_historical.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Symbol</th>
      <th>NOCP</th>
    </tr>
    <tr>
      <th>Trade DATE</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-05-09</th>
      <td>GOOG</td>
      <td>1162.38</td>
    </tr>
    <tr>
      <th>2019-05-08</th>
      <td>GOOG</td>
      <td>1166.27</td>
    </tr>
    <tr>
      <th>2019-05-07</th>
      <td>GOOG</td>
      <td>1174.10</td>
    </tr>
    <tr>
      <th>2019-05-06</th>
      <td>GOOG</td>
      <td>1189.39</td>
    </tr>
    <tr>
      <th>2019-05-03</th>
      <td>GOOG</td>
      <td>1185.40</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Read the second stock
# YOUR CODE HERE
aapl_historical_csv = Path("../Resources/aapl_historical.csv")
aapl_historical = pd.read_csv(aapl_historical_csv, index_col= 'Trade DATE', infer_datetime_format=True, parse_dates=True)
aapl_historical.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Symbol</th>
      <th>NOCP</th>
    </tr>
    <tr>
      <th>Trade DATE</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-05-09</th>
      <td>AAPL</td>
      <td>200.72</td>
    </tr>
    <tr>
      <th>2019-05-08</th>
      <td>AAPL</td>
      <td>202.90</td>
    </tr>
    <tr>
      <th>2019-05-07</th>
      <td>AAPL</td>
      <td>202.86</td>
    </tr>
    <tr>
      <th>2019-05-06</th>
      <td>AAPL</td>
      <td>208.48</td>
    </tr>
    <tr>
      <th>2019-05-03</th>
      <td>AAPL</td>
      <td>211.75</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Read the third stock
# YOUR CODE HERE
cost_historical_csv = Path("../Resources/cost_historical.csv")
cost_historical = pd.read_csv(cost_historical_csv, index_col= 'Trade DATE', infer_datetime_format=True, parse_dates=True)
cost_historical.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Symbol</th>
      <th>NOCP</th>
    </tr>
    <tr>
      <th>Trade DATE</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-05-09</th>
      <td>COST</td>
      <td>243.47</td>
    </tr>
    <tr>
      <th>2019-05-08</th>
      <td>COST</td>
      <td>241.34</td>
    </tr>
    <tr>
      <th>2019-05-07</th>
      <td>COST</td>
      <td>240.18</td>
    </tr>
    <tr>
      <th>2019-05-06</th>
      <td>COST</td>
      <td>244.23</td>
    </tr>
    <tr>
      <th>2019-05-03</th>
      <td>COST</td>
      <td>244.62</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Concatenate all stocks into a single DataFrame
# YOUR CODE HERE
combined_stocks = pd.concat([goog_historical, aapl_historical , cost_historical], axis="rows", join="inner")
combined_stocks.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Symbol</th>
      <th>NOCP</th>
    </tr>
    <tr>
      <th>Trade DATE</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-05-09</th>
      <td>GOOG</td>
      <td>1162.38</td>
    </tr>
    <tr>
      <th>2019-05-08</th>
      <td>GOOG</td>
      <td>1166.27</td>
    </tr>
    <tr>
      <th>2019-05-07</th>
      <td>GOOG</td>
      <td>1174.10</td>
    </tr>
    <tr>
      <th>2019-05-06</th>
      <td>GOOG</td>
      <td>1189.39</td>
    </tr>
    <tr>
      <th>2019-05-03</th>
      <td>GOOG</td>
      <td>1185.40</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Reset the index
# YOUR CODE HERE
combined_stocks.reset_index(inplace=True)
combined_stocks.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Trade DATE</th>
      <th>Symbol</th>
      <th>NOCP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-05-09</td>
      <td>GOOG</td>
      <td>1162.38</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-05-08</td>
      <td>GOOG</td>
      <td>1166.27</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-05-07</td>
      <td>GOOG</td>
      <td>1174.10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-05-06</td>
      <td>GOOG</td>
      <td>1189.39</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-05-03</td>
      <td>GOOG</td>
      <td>1185.40</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Pivot the Data so that the stock tickers are the columns, the dates are the index, and the 
# values are the closing prices
# YOUR CODE HERE
combined_stocks = combined_stocks.pivot_table(values="NOCP", index="Trade DATE", columns="Symbol")
combined_stocks.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Symbol</th>
      <th>AAPL</th>
      <th>COST</th>
      <th>GOOG</th>
    </tr>
    <tr>
      <th>Trade DATE</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2018-05-11</th>
      <td>188.59</td>
      <td>195.76</td>
      <td>1098.26</td>
    </tr>
    <tr>
      <th>2018-05-14</th>
      <td>188.15</td>
      <td>195.88</td>
      <td>1100.20</td>
    </tr>
    <tr>
      <th>2018-05-15</th>
      <td>186.44</td>
      <td>195.48</td>
      <td>1079.23</td>
    </tr>
    <tr>
      <th>2018-05-16</th>
      <td>188.18</td>
      <td>198.71</td>
      <td>1081.77</td>
    </tr>
    <tr>
      <th>2018-05-17</th>
      <td>186.99</td>
      <td>199.60</td>
      <td>1078.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Drop Nulls
# YOUR CODE HERE
combined_returns = combined_stocks.pct_change().dropna()
combined_returns.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Symbol</th>
      <th>AAPL</th>
      <th>COST</th>
      <th>GOOG</th>
    </tr>
    <tr>
      <th>Trade DATE</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2018-05-14</th>
      <td>-0.002333</td>
      <td>0.000613</td>
      <td>0.001766</td>
    </tr>
    <tr>
      <th>2018-05-15</th>
      <td>-0.009088</td>
      <td>-0.002042</td>
      <td>-0.019060</td>
    </tr>
    <tr>
      <th>2018-05-16</th>
      <td>0.009333</td>
      <td>0.016523</td>
      <td>0.002354</td>
    </tr>
    <tr>
      <th>2018-05-17</th>
      <td>-0.006324</td>
      <td>0.004479</td>
      <td>-0.002940</td>
    </tr>
    <tr>
      <th>2018-05-18</th>
      <td>-0.003637</td>
      <td>-0.003206</td>
      <td>-0.011339</td>
    </tr>
  </tbody>
</table>
</div>



## Calculate the weighted returns for the portfolio assuming an equal number of shares for each stock


```python
# Calculate weighted portfolio returns
weights = [1/3, 1/3, 1/3]
# YOUR CODE HERE
portfolio_returns = combined_returns.dot(weights)
portfolio_returns.head()
```




    Trade DATE
    2018-05-14    0.000015
    2018-05-15   -0.010064
    2018-05-16    0.009403
    2018-05-17   -0.001595
    2018-05-18   -0.006061
    dtype: float64



## Join your portfolio returns to the DataFrame that contains all of the portfolio returns


```python
# Only compare dates where the new, custom portfolio has dates
# YOUR CODE HERE

new_portfolio = pd.concat([combined_data, portfolio_returns], axis='columns', join='inner')
new_portfolio.columns=['SOROS FUND MANAGEMENT LLC', 'PAULSON & CO.INC.', 'TIGER GLOBAL MANAGEMENT LLC', 'BERKSHIRE HATHAWAY INC','Algo 1','Algo 2', 'S&P 500', 'custom']
new_portfolio.sort_index(ascending=False).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SOROS FUND MANAGEMENT LLC</th>
      <th>PAULSON &amp; CO.INC.</th>
      <th>TIGER GLOBAL MANAGEMENT LLC</th>
      <th>BERKSHIRE HATHAWAY INC</th>
      <th>Algo 1</th>
      <th>Algo 2</th>
      <th>S&amp;P 500</th>
      <th>custom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-04-22</th>
      <td>-0.002586</td>
      <td>-0.007333</td>
      <td>-0.003640</td>
      <td>-0.001088</td>
      <td>0.000677</td>
      <td>-0.001936</td>
      <td>-0.008764</td>
      <td>0.001217</td>
    </tr>
    <tr>
      <th>2019-04-18</th>
      <td>0.001448</td>
      <td>0.001222</td>
      <td>0.000582</td>
      <td>0.001916</td>
      <td>-0.000588</td>
      <td>-0.001229</td>
      <td>-0.001011</td>
      <td>0.001545</td>
    </tr>
    <tr>
      <th>2019-04-17</th>
      <td>-0.002897</td>
      <td>-0.006467</td>
      <td>-0.004409</td>
      <td>0.003222</td>
      <td>-0.010301</td>
      <td>-0.005228</td>
      <td>-0.001577</td>
      <td>0.009292</td>
    </tr>
    <tr>
      <th>2019-04-16</th>
      <td>0.002699</td>
      <td>0.000388</td>
      <td>-0.000831</td>
      <td>0.000837</td>
      <td>-0.006945</td>
      <td>0.002899</td>
      <td>0.002279</td>
      <td>0.000340</td>
    </tr>
    <tr>
      <th>2019-04-15</th>
      <td>-0.001422</td>
      <td>-0.001156</td>
      <td>0.000398</td>
      <td>-0.010492</td>
      <td>-0.004331</td>
      <td>-0.004572</td>
      <td>-0.000509</td>
      <td>0.007522</td>
    </tr>
  </tbody>
</table>
</div>



## Re-run the performance and risk analysis with your portfolio to see how it compares to the others


```python
# Risk
# YOUR CODE HERE
new_daily_std = new_portfolio.std()
new_annualized_std = new_daily_std * np.sqrt(252)
new_annualized_std
```




    SOROS FUND MANAGEMENT LLC      0.146812
    PAULSON & CO.INC.              0.116928
    TIGER GLOBAL MANAGEMENT LLC    0.232898
    BERKSHIRE HATHAWAY INC         0.247305
    Algo 1                         0.133927
    Algo 2                         0.139499
    S&P 500                        0.152469
    custom                         0.211627
    dtype: float64




```python
# Rolling
# YOUR CODE HERE
new_portfolio.rolling(window=21).std().plot(figsize=(20,10), title='21 Day Rolling Standard Deviation')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11db0c940>




![png](output_59_1.png)



```python
# Beta
# YOUR CODE HERE
covariance_2 = new_portfolio['custom'].cov(new_portfolio['S&P 500'])
covariance_2

variance_2 = new_portfolio['S&P 500'].var()
variance_2

custom_beta = covariance / variance
custom_beta

rolling_covariance_2 = new_portfolio['custom'].rolling(window=30).cov(new_portfolio['S&P 500'])
rolling_variance_2 = new_portfolio['S&P 500'].rolling(window=30).var()
rolling_beta_2 = rolling_covariance / rolling_variance
rolling_beta_2.plot(figsize=(20, 10), title='Rolling 30-Day Beta of custom')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1221be5f8>




![png](output_60_1.png)



```python
# Annualzied Sharpe Ratios
# YOUR CODE HERE
sharpe_ratios2 = (new_portfolio.mean() * 252) / (new_portfolio.std() * np.sqrt(252))
sharpe_ratios2
```




    SOROS FUND MANAGEMENT LLC      0.380007
    PAULSON & CO.INC.              0.227577
    TIGER GLOBAL MANAGEMENT LLC   -1.066635
    BERKSHIRE HATHAWAY INC         0.103006
    Algo 1                         2.001260
    Algo 2                         0.007334
    S&P 500                       -0.427676
    custom                         0.876152
    dtype: float64




```python
# Visualize the sharpe ratios as a bar plot
# YOUR CODE HERE
sharpe_ratios2.plot.bar(title="Sharpe Ratios2")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1221a7e48>




![png](output_62_1.png)


## Include correlation analysis to determine which stocks (if any) are correlated


```python
# YOUR CODE HERE
correlation2 = new_portfolio.corr()
correlation2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SOROS FUND MANAGEMENT LLC</th>
      <th>PAULSON &amp; CO.INC.</th>
      <th>TIGER GLOBAL MANAGEMENT LLC</th>
      <th>BERKSHIRE HATHAWAY INC</th>
      <th>Algo 1</th>
      <th>Algo 2</th>
      <th>S&amp;P 500</th>
      <th>custom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>SOROS FUND MANAGEMENT LLC</th>
      <td>1.000000</td>
      <td>0.791802</td>
      <td>0.477844</td>
      <td>0.816197</td>
      <td>0.336909</td>
      <td>0.862583</td>
      <td>-0.028256</td>
      <td>0.732548</td>
    </tr>
    <tr>
      <th>PAULSON &amp; CO.INC.</th>
      <td>0.791802</td>
      <td>1.000000</td>
      <td>0.484869</td>
      <td>0.650390</td>
      <td>0.360727</td>
      <td>0.783865</td>
      <td>-0.059862</td>
      <td>0.643828</td>
    </tr>
    <tr>
      <th>TIGER GLOBAL MANAGEMENT LLC</th>
      <td>0.477844</td>
      <td>0.484869</td>
      <td>1.000000</td>
      <td>0.324306</td>
      <td>0.113671</td>
      <td>0.408402</td>
      <td>0.005881</td>
      <td>0.390961</td>
    </tr>
    <tr>
      <th>BERKSHIRE HATHAWAY INC</th>
      <td>0.816197</td>
      <td>0.650390</td>
      <td>0.324306</td>
      <td>1.000000</td>
      <td>0.325985</td>
      <td>0.782054</td>
      <td>-0.038832</td>
      <td>0.800558</td>
    </tr>
    <tr>
      <th>Algo 1</th>
      <td>0.336909</td>
      <td>0.360727</td>
      <td>0.113671</td>
      <td>0.325985</td>
      <td>1.000000</td>
      <td>0.364457</td>
      <td>-0.054478</td>
      <td>0.260331</td>
    </tr>
    <tr>
      <th>Algo 2</th>
      <td>0.862583</td>
      <td>0.783865</td>
      <td>0.408402</td>
      <td>0.782054</td>
      <td>0.364457</td>
      <td>1.000000</td>
      <td>-0.042540</td>
      <td>0.739020</td>
    </tr>
    <tr>
      <th>S&amp;P 500</th>
      <td>-0.028256</td>
      <td>-0.059862</td>
      <td>0.005881</td>
      <td>-0.038832</td>
      <td>-0.054478</td>
      <td>-0.042540</td>
      <td>1.000000</td>
      <td>0.005603</td>
    </tr>
    <tr>
      <th>custom</th>
      <td>0.732548</td>
      <td>0.643828</td>
      <td>0.390961</td>
      <td>0.800558</td>
      <td>0.260331</td>
      <td>0.739020</td>
      <td>0.005603</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
