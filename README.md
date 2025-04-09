# Stock Price Super-Resolution: Hourly to 2-Minute Prediction

## Abstract

Financial time series are complex, especially at high frequencies. Accessing minute-level data can be challenging, yet crucial for many analyses. This project explores "time-series super-resolution" – predicting intra-hour (2-minute) price movements using only historical hourly OHLC (Open, High, Low, Close) data. We employ an LSTM-Attention framework designed to capture temporal trends while enforcing strict boundary constraints, ensuring the predicted sequence starts and ends at the known hourly anchor points. While the model adheres to these constraints, results indicate significant challenges in accurately reconstructing realistic intra-hour dynamics solely from hourly data.

## Problem Statement

High-frequency trading and analysis benefit greatly from minute-level data, which is often unavailable or costly. Many datasets only provide hourly or daily records.

**Research Question:** Can low-resolution (hourly) stock data be used to approximate high-resolution (2-minute) data by leveraging historical trends and deep learning models?

## Key Concepts & Approach

1.  **Time-Series Super-Resolution:** Framing the problem as generating a high-resolution sequence from a low-resolution input, analogous to image super-resolution.
2.  **LSTM (Long Short-Term Memory):** Used as an **Encoder** to read and understand patterns in the historical sequence of hourly data.
3.  **Attention Mechanism:** Allows the model to focus on the most relevant past hourly data points when predicting the details of the target hour.
4.  **Decoder (Fully Connected Layers):** Takes the context from the Encoder and Attention mechanism to generate the sequence of predicted 2-minute values.
5.  **Boundary Constraints:** A crucial post-processing step ensures the predicted 2-minute sequence's start and end points exactly match the known, observed values for that hour.

## Data

*   **Source:** Yahoo Finance API
*   **Input Data:** Hourly OHLC stock data.
*   **Target Data (for training/evaluation):** 2-Minute OHLC stock data for the corresponding hours.
*   **Preprocessing:** Data cleaning (duplicates removal), missing value imputation (linear interpolation), normalization (MinMaxScaler).

## Methodology Overview

1.  **Data Collection & Preparation:** Automated scripts download and organize hourly and 2-minute data. Data is cleaned, scaled, and structured into sequences (e.g., 24 past hours -> 30 target 2-minute intervals).
2.  **Model Architecture:** An Encoder-Decoder framework:
    *   **Encoder:** Multi-layer LSTM processes the hourly sequence.
    *   **Attention:** Multi-Head Attention focuses on relevant historical steps.
    *   **Decoder:** Fully Connected layers generate the 2-minute OHLC predictions.
3.  **Training:** Trained using Mean Squared Error (MSE) loss on the intermediate 2-minute points, optimized with Adam, and using a learning rate scheduler.
4.  **Evaluation:** Performance assessed using MAE, RMSE, R-squared (R²), and trend correlation against the actual 2-minute data.

## Results & Discussion

*   The model successfully enforces the start and end boundary constraints for the predicted hour.
*   However, the prediction of *intermediate* 2-minute price movements is poor.
*   Key metrics (like **negative R² scores**) indicate the model performs worse than a simple baseline (e.g., predicting the mean).
*   Predicted paths often exhibit unrealistic volatility and oscillations, failing to capture the true shape of intra-hour price action.
*   **Conclusion:** Historical hourly OHLC data alone appears insufficient to accurately reconstruct high-frequency dynamics using this architecture. Significant information is likely lost between hourly observations.

*(See the included poster and presentation for detailed plots and metrics.)*

## Future Work Ideas

*   Incorporate additional features (Volume, Volatility indicators, Sentiment).
*   Explore alternative architectures (Transformers, Diffusion Models).
*   Train on significantly more historical data.
*   Implement flexible context lengths based on market conditions.

## Acknowledgements

*   Supervisors: Mark Daly, Michael Russell
*   Technological University of the Shannon (TUS)
