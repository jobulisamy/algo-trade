# Project Scope: Independent Model and Strategy Trading

## Overview
This project implements an algorithmic trading bot for Alpaca API, where each model and strategy operates and trades independently. There is no ensemble signal aggregation. Each model and strategy generates its own trading signal and executes trades based on its own logic and thresholds.

---

## Architecture
The project is implemented in a modular Jupyter Notebook with 15 distinct cells:
1. **Imports & Setup**: Load dependencies and configure the environment.
2. **Configuration**: Define settings using a `@dataclass` with `slots=True`.
3. **Data Layer**: Fetch and preprocess data using `DataFetcher` and `FeatureEngine`.
4. **Traditional Strategies**: Implement 4 strategies (RSI, Momentum, MACD, Gap).
5-7. **ML Models**: Train and use RandomForest, XGBoost, and LSTM models.
8. **Risk Management**: Enforce position sizing, stop-loss, and circuit breakers.
9. **Order Execution**: Place trades via Alpaca API.
10. **Main Bot**: Orchestrate the workflow.
11. **Testing & Validation**: Validate strategies and models.
12. **Production Run**: Execute the bot in a live environment.
13. **Performance Monitoring**: Track and log performance metrics.
14. **Manual Controls**: Allow user overrides.

---

## Key Features
- **Independent Model Execution:**
  - Random Forest, XGBoost, and LSTM models run separately.
  - Each model loads its own trained parameters and makes predictions on the latest data.
  - Trades are executed based on each model's signal and threshold.

- **Independent Strategy Execution:**
  - Traditional strategies (RSI Mean Reversion, Momentum Breakout, MACD Volume, Gap Fade) run independently.
  - Each strategy generates its own signal and executes trades if its signal meets the defined threshold.

- **Order Execution:**
  - Each model and strategy places trades directly through the Alpaca API when its own signal triggers a buy or sell.
  - No aggregation or weighting of signals across models/strategies.

- **Risk Management:**
  - Risk management checks (position sizing, stop loss, take profit, exposure) are applied per model and per strategy.
  - Optionally, track positions and risk per model/strategy.

- **Performance Tracking:**
  - Trades and performance are tracked separately for each model and strategy.
  - Results are reported and visualized independently.

---

## Excluded Features
- No ensemble signal aggregation or weighted combination of signals.
- No use of ensemble weights or ensemble thresholds.
- No signal aggregator logic.

---

## Benefits
- Clear visibility into the performance of each model and strategy.
- Easier to analyze, optimize, and debug individual components.
- Flexible framework for adding, removing, or updating models and strategies independently.

---

## Risk Parameters
- **Position Sizing**: Max 15% per position, 90% total exposure.
- **Stop Loss**: 3%.
- **Take Profit**: 7%.
- **Circuit Breaker**: Halt trading if portfolio loss exceeds 3% in a day.
- **Minimum Position**: $1,000.

---

## Data Features
- **Historical Data**: 60 days of historical data for model training.
- **LSTM Sequences**: 30-day sequences for time-series analysis.
- **Technical Indicators**: SMA, EMA, RSI, MACD, Bollinger Bands, ATR, volume ratios, etc.
- **Target Variable**: 5-day forward returns categorized into BUY, HOLD, SELL.

---

## Machine Learning Models
- **RandomForest**: Scikit-learn implementation for feature-based classification.
- **XGBoost**: Gradient boosting model for robust predictions.
- **LSTM**: TensorFlow/Keras model for time-series analysis.
- **Training**: Weekly retraining with 60 days of historical data.
- **Persistence**: Models saved in the `models/` directory.

---

## Operational Details
- **Check Intervals**: Every 5 minutes.
- **Data Caching**: Reduce API calls by caching data every 5 minutes.
- **Execution**: Zero-commission trading via Alpaca API.
- **Logging**: Performance metrics logged to `bot.log`.

---

## Key Design Decisions
1. **Zero Commissions**: Optimize for signal quality, not trade frequency.
2. **Proportional Spreads**: Larger positions incur higher costs.
3. **Aggressive Caching**: Minimize API rate limits.
4. **Training Data**: Use 60 days of historical data; LSTM uses 30-day sequences.
5. **Notebook Modularity**: Each cell demonstrates functionality with test outputs.

---

## Dependencies
- **Python Version**: 3.12.
- **Libraries**:
  - `alpaca-trade-api==3.0.2`
  - `pandas==2.1.4`
  - `numpy==1.26.2`
  - `scikit-learn==1.3.2`
  - `xgboost==2.0.3`
  - `tensorflow==2.15.0`
  - `python-dotenv==1.0.0`
  - `tqdm`, `ipywidgets`, `matplotlib==3.8.2`, `seaborn==0.13.0`

---

## Coding Standards
- **Type Hints**: Use modern syntax (e.g., `list[str]`, `dict[str, float]`).
- **Dataclasses**: Use `@dataclass(slots=True)` for configuration.
- **Error Handling**: Use `try-except` blocks for API calls.
- **File Operations**: Use `pathlib.Path` instead of `os.path`.
- **Debugging**: Use f-strings with `=` for inline debugging.
- **Conditionals**: Use `match/case` for multi-way conditionals.
- **Progress Bars**: Use `tqdm.notebook` for long operations.
- **Documentation**: Google-style docstrings for all functions.

---

## Steps to Recreate the Project
1. **Set Up Environment**:
   - Install Python 3.12.
   - Create a Conda environment: `conda create -n algotrade python=3.12`.
   - Activate the environment: `conda activate algotrade`.
   - Install dependencies: `pip install -r requirements.txt`.
2. **Prepare Data**:
   - Fetch historical data for 15 symbols using Alpaca API.
   - Compute technical indicators and preprocess data.
3. **Train Models**:
   - Train RandomForest, XGBoost, and LSTM models on historical data.
   - Save models to the `models/` directory.
4. **Configure Bot**:
   - Define risk parameters and trading logic in the configuration cell.
5. **Run Bot**:
   - Execute the notebook cells sequentially.
   - Monitor performance and adjust parameters as needed.
6. **Deploy**:
   - Set up a server to run the bot continuously.
   - Schedule weekly model retraining.

---

## Usage
- Run the notebook to generate signals and execute trades for each model and strategy independently.
- Review trade results and performance metrics for each component separately.

---

This document provides a comprehensive overview of the Independent Model and Strategy Trading Bot, enabling developers to understand, implement, and extend the project.
