# Monthly Stock Price Aggregation & Technical Indicator Computation
## 1. Overview:
This project implements a data engineering pipeline that converts daily stock market data into monthly aggregated datasets and computes key technical indicators used in financial analysis.
The solution processes 2 years of daily data for 10 stock tickers, applies correct OHLC aggregation logic, calculates SMA and EMA indicators, and exports the results into partitioned CSV files (one file per ticker).


## 2. Input Data:
### Dataset link: 
- https://github.com/sandeep-tt/tt-intern-dataset
### Input File:
Filename: stock_data.csv  
Format: CSV  
Frequency: Daily  
Duration: 2 years  

### Schema:
| Column Name | Data Type | Description |
|------------|-----------|-------------|
| date       | Date      | Trading date |
| open       | Float     | Opening price |
| high       | Float     | Highest price of the day |
| low        | Float     | Lowest price of the day |
| close      | Float     | Closing price |
| adjclose   | Float     | Adjusted close price |
| volume     | Integer   | Traded volume |
| ticker     | String    | Stock symbol |

### Tickers Covered:
  ```text
  AAPL, AMD, AMZN, AVGO, CSCO, MSFT, NFLX, PEP, TMUS, TSLA
  ```

## 3. Processing Requirements:
### Monthly Aggregation Rules (OHLC):
  Daily data is resampled to monthly frequency using the following logic:

| Field  | Aggregation Rule                |
| ------ | ------------------------------- |
| Open   | First trading day of the month  |
| Close  | Last trading day of the month   |
| High   | Maximum daily high in the month |
| Low    | Minimum daily low in the month  |
| Volume | Sum of daily volumes            |

## 4. Technical Indicators:
All indicators are computed using monthly closing prices.  
### Simple Moving Average (SMA):
* Formula:
  
$$\text{SMA}_N = \frac{\sum_{i=1}^{N} \text{Close}_i}{N}$$

* where:  
    N = Number of Periods

* Computed for:  
SMA-10  
SMA-20

### Exponential Moving Average (EMA):
* Multiplier (Smoothing Factor):
    
$$\alpha = \frac{2}{N + 1}$$

* EMA Formula:

$$\text{EMA}_t = \alpha \cdot \text{Price}_t + (1 - \alpha) \cdot \text{EMA}_{t-1}$$

* Where:     
- $\text{Price}_t$ = Current Day's Price       
- $\text{EMA}_{t-1}$ = Previous Day's EMA
  
* Computed for:  
EMA-10  
EMA-20  

## 5. Output:
* Exactly 10 CSV files (one per ticker)
* Exactly 24 rows per file (2 years of monthly data)
* File naming convention:
```result_<TICKER>.csv```


## 6. Folder Structure:
```
FamPay_Assignment  
 ├── output/  
 │   └── result_&lt;TICKER&gt;.csv 
 ├── stock_data.csv  
 ├── submission.ipynb  
 └── README.md
```

 
## 7. Data Validation & Quality Checks:

The pipeline enforces:
* Exactly 24 months per ticker
* No missing OHLC values
* Logical OHLC constraints:
```
low ≤ open ≤ high
low ≤ close ≤ high
```
Assertions ensure failures are caught early.

## 8. Assumptions

* Each ticker contains 24 months of data
* Missing trading days within a month are acceptable
* Indicators are calculated using available historical data (min_periods=1)
* Calendar months are used for resampling
* For the first EMA, used the SMA as the "Previous Day's EMA"
* For the initial months where fewer than N periods are available, the Simple Moving Average (SMA) is calculated using all available historical data up to that month.

## 9. Tools & Technologies

* Python 3.13.5
* Pandas
* NumPy
No third-party technical analysis libraries were used.

## Author:
Vartika

## References:

- https://www.investopedia.com/terms/e/ema.asp
- https://groww.in/p/exponential-moving-average













  
