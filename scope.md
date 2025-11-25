# Project Scope: ML-Enhanced Algorithmic Trading Bot

## Purpose
The ML-Enhanced Algorithmic Trading Bot is designed to execute trades on the stock market using a combination of traditional trading strategies and machine learning models. The bot integrates with the Alpaca Trade API for zero-commission trading and operates on a $100,000 paper trading portfolio. The goal is to optimize returns by leveraging ensemble-based decision-making and robust risk management.

---

## Architecture
The project is implemented in a modular Jupyter Notebook with 15 distinct cells:
1. **Imports & Setup**: Load dependencies and configure the environment.
2. **Configuration**: Define settings using a `@dataclass` with `slots=True`.
3. **Data Layer**: Fetch and preprocess data using `DataFetcher` and `FeatureEngine`.
4. **Traditional Strategies**: Implement 4 strategies (RSI, Momentum, MACD, Gap).
5-7. **ML Models**: Train and use RandomForest, XGBoost, and LSTM models.
8. **Signal Aggregator**: Combine strategy outputs using ensemble weights.
9. **Risk Management**: Enforce position sizing, stop-loss, and circuit breakers.
10. **Order Execution**: Place trades via Alpaca API.
11. **Main Bot**: Orchestrate the workflow.
12. **Testing & Validation**: Validate strategies and models.
13. **Production Run**: Execute the bot in a live environment.
14. **Performance Monitoring**: Track and log performance metrics.
15. **Manual Controls**: Allow user overrides.

---

## Trading Logic
- **Strategies**: 7 strategies with ensemble weights:
  - RSI: 12%
  - Momentum: 12%
  - MACD: 10%
  - Gap: 8%
  - RandomForest: 18%
  - XGBoost: 18%
  - LSTM: 22%
- **Symbols**: 15 liquid stocks (e.g., SPY, QQQ, AAPL, MSFT, etc.).
- **Indicators**: 25+ technical indicators, including SMA/EMA, RSI, MACD, Bollinger Bands, ATR, volume ratios, and momentum indicators.
- **ML Target**: 3-class classification (BUY/HOLD/SELL) based on 5-day forward returns with ±1.2% thresholds.
- **Signal Range**: -1 (strong sell) to +1 (strong buy). Trades are executed if |signal| > 0.55.

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

This document provides a comprehensive overview of the ML-Enhanced Algorithmic Trading Bot, enabling developers to understand, implement, and extend the project.
