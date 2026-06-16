# Short-Term Stock Movement Prediction using Meta-Ensemble Learning

## Overview

This project explores short-term stock price direction prediction using historical hourly stock market data. Instead of predicting the exact future stock price, the model predicts whether the next-hour price movement will be upward or downward.

The project compares classical machine learning models, deep learning models, and ensemble learning methods to evaluate whether combining different model types can produce more stable predictions than relying on a single model.

This was completed as part of an IS424 Data Mining and Business Analytics group project.

## Project Objective

The main objective is to predict next-hour stock price movement using historical OHLCV data and engineered technical indicators.

The project investigates the following question:

> Can ensemble learning methods improve short-term stock movement prediction compared to standalone machine learning and deep learning models?

## Dataset

Historical hourly stock data was retrieved from Yahoo Finance using the `yfinance` Python library.

The dataset consists of hourly OHLCV data:

* Open
* High
* Low
* Close
* Volume

The project used 10 large-cap stocks:

* Apple (AAPL)
* Microsoft (MSFT)
* Amazon (AMZN)
* Nvidia (NVDA)
* Google (GOOG)
* JPMorgan Chase (JPM)
* Exxon Mobil (XOM)
* Johnson & Johnson (JNJ)
* Procter & Gamble (PG)
* Tesla (TSLA)

Two time horizons were used:

| Dataset            |                  Date Range | Approximate Size |
| ------------------ | --------------------------: | ---------------: |
| Long-term dataset  |    March 2025 to March 2026 |      17,000 rows |
| Short-term dataset | December 2025 to March 2026 |       4,000 rows |

The target variable was defined as a binary classification label:

* `1`: next-hour price movement is upward
* `0`: next-hour price movement is downward

## Methodology

The project followed an end-to-end machine learning workflow.

### 1. Data Collection

Hourly OHLCV stock data was collected using `yfinance`.

### 2. Data Preprocessing

The raw price data was cleaned and transformed into a modelling-ready format. To avoid data leakage, the dataset was split chronologically instead of randomly.

### 3. Feature Engineering

Price-based technical features were engineered from the raw OHLCV data, including:

* Intraday range
* Percentage price change
* Volume change
* Moving-average ratios
* Rolling volatility
* Momentum indicators
* Log returns

### 4. Classical Machine Learning Models

The following classical machine learning models were trained and evaluated:

* Logistic Regression
* Decision Tree
* Support Vector Machine (SVM)

Hyperparameter tuning was performed using:

* GridSearchCV
* RandomizedSearchCV
* Time Series Cross-Validation

### 5. Deep Learning Models

Two sequence-based deep learning models were implemented using PyTorch:

* Long Short-Term Memory Network (LSTM)
* Temporal Convolutional Network (TCN)

The deep learning models used lookback windows to capture sequential market patterns:

| Dataset            | Lookback Window |
| ------------------ | --------------: |
| Long-term dataset  |        12 hours |
| Short-term dataset |         6 hours |

### 6. Ensemble Learning

The project combined classical machine learning and deep learning model outputs using soft-voting ensemble methods.

The ensemble pipeline included:

* Long-term ensemble model
* Short-term ensemble model
* Final meta-ensemble using cross-dataset fusion

The final meta-ensemble combined predictions from both temporal horizons and was evaluated on a fully unseen vault dataset.

## Model Pipeline

```text
Raw OHLCV Stock Data
        |
        v
Data Cleaning and Feature Engineering
        |
        v
Chronological Train-Test Split
        |
        v
Classical ML Models + Deep Learning Models
        |
        v
Long-Term Ensemble + Short-Term Ensemble
        |
        v
Final Cross-Dataset Meta-Ensemble
        |
        v
Prediction: Upward or Downward Price Movement
```

## Models Used

| Model Type    | Models                                   |
| ------------- | ---------------------------------------- |
| Classical ML  | Logistic Regression, Decision Tree, SVM  |
| Deep Learning | LSTM, TCN                                |
| Ensemble      | Soft Voting, Cross-Dataset Meta-Ensemble |

## Key Results

Short-term hourly stock movement prediction was found to be highly noisy, with many individual models performing close to the 50% baseline.

However, the ensemble models helped produce more stable prediction behaviour and reduced class-collapse issues observed in some individual tuned models.

| Model / Ensemble    | Accuracy | Precision | Recall | F1 Score |
| ------------------- | -------: | --------: | -----: | -------: |
| Long-Term Ensemble  |   0.5272 |    0.5437 | 0.4043 |   0.4638 |
| Short-Term Ensemble |   0.5353 |    0.5538 | 0.3003 |   0.3894 |
| Final Meta-Ensemble |   0.5146 |    0.6812 | 0.1378 |   0.2293 |

The final meta-ensemble achieved the highest precision, making it more suitable for high-confidence, low-coverage prediction scenarios. However, this came at the cost of lower recall.

## Key Takeaways

* Predicting next-hour stock price direction using only technical price-based features is highly challenging.
* Individual models often performed close to random baseline due to noisy and unstable financial time series data.
* Deep learning models such as LSTM and TCN were useful for capturing sequential patterns.
* Ensemble learning improved prediction stability compared to relying on standalone models.
* The final meta-ensemble produced fewer false positive buy signals, making it more suitable for a risk-averse prediction strategy.

## Limitations

Several limitations should be considered when interpreting the results:

* The project used only price-based technical features.
* No macroeconomic indicators, company fundamentals, news, or sentiment data were included.
* Hourly stock prediction is highly noisy and difficult to model reliably.
* The short-term dataset was relatively small compared to the long-term dataset.
* The ensemble threshold was manually selected and could be further optimized.

## Future Improvements

Future work could include:

* Incorporating financial news sentiment using NLP models such as FinBERT
* Adding macroeconomic and fundamental indicators
* Testing the pipeline on daily or multi-day prediction horizons
* Using data-driven threshold optimization
* Exploring reinforcement learning for dynamic trading threshold adjustment

## Technologies Used

* Python
* Pandas
* NumPy
* Scikit-learn
* PyTorch
* yfinance
* Matplotlib
* Logistic Regression
* Decision Tree
* Support Vector Machine
* LSTM
* TCN
* Ensemble Learning

## Repository Structure

```text
stock-movement-meta-ensemble/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   └── stock_movement_prediction.ipynb
│
├── report/
│   └── final_project_report.pdf
│
├── images/
│   ├── methodology_diagram.png
│   ├── ensemble_results.png
│   └── confusion_matrix.png
│
└── outputs/
    └── model_results_summary.csv
```

## How to Run

1. Clone the repository.

```bash
git clone https://github.com/your-username/stock-movement-meta-ensemble.git
```

2. Navigate into the project folder.

```bash
cd stock-movement-meta-ensemble
```

3. Open the notebook.

```bash
jupyter notebook notebooks/stock_movement_prediction.ipynb
```

## Disclaimer

This project is for educational and research purposes only. It is not intended to provide financial advice or trading recommendations.
