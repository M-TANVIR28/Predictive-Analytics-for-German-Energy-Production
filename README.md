# Predictive Analytics for German Energy Production

## Project Overview

This project focuses on time series analysis and forecasting of German total energy generation. The primary goal is to build and evaluate models for predicting future energy generation based on historical data.

## Dataset

The project utilizes historical energy generation data for Germany, sourced from a specific time period. The raw data includes quarter-hourly measurements of actual energy generation.
* **Data Source:** Data was collected from **Azure Blob Storage**.
* **File Example:** `Actual_generation_202301010000_202507300000_Quarterhour (1).csv`
* **Total Data Size:** The complete dataset and project files total approximately 35 MB.

## Tools Used

* **Azure Machine Learning Workspace:** Development and experimentation were primarily conducted within the Azure ML Workspace, utilizing its integrated JupyterLab environment.
* **Python:** The core programming language for data analysis, modeling, and visualization.
* **Git & GitHub:** For version control and collaboration, with the repository hosted on GitHub.

## Methodology

The project followed a standard machine learning pipeline:

1.  **Data Loading & Configuration:**
    * Data was accessed from Azure Blob Storage using `azure.storage.blob.BlobServiceClient`.
    * **Note on Security:** Initial versions of the notebook contained sensitive access keys. These have since been **removed** from the public repository to ensure security. Best practices for production involve loading such credentials securely from environment variables (e.g., Azure Key Vault).

2.  **Data Cleaning & Preprocessing:**
    * Handling of missing values and data type conversions.
    * Resampling or aggregation as necessary for the time series.

3.  **Exploratory Data Analysis (EDA):**
    * Analysis of historical trends, seasonal patterns (daily, weekly, yearly), and any observed anomalies in the energy generation data.

4.  **Feature Engineering:**
    * Creation of time-based features (e.g., hour of day, day of week, month, year) to capture temporal patterns.

5.  **Model Training:**
    * **Naive Forecast (Lag-1):** Used as a simple baseline model, predicting the next value as the current value.
    * **XGBoost:** A gradient boosting model known for its performance in structured data.
    * **Simple Linear Regression:** A basic regression model for comparison.

6.  **Model Evaluation & Benchmarking:**
    * Models were evaluated using common time series metrics: Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE) on a test set.
    * **Key Finding:** The **Naive Forecast (Lag-1) significantly outperformed** the more complex XGBoost and Simple Linear Regression models on the test set.
        * **Naive Forecast:** MAE: 149.99 MWh, RMSE: 206.47 MWh
        * **XGBoost:** MAE: 723.63 MWh, RMSE: 1420.04 MWh
        * **Simple Linear Regression:** MAE: 249.36 MWh, RMSE: 857.99 MWh

7.  **Residual Analysis:**
    * Analysis of the prediction errors (residuals) revealed patterns by hour and day. This suggests that even the best models struggled with certain time-of-day or day-of-week effects, indicating potential areas for further feature engineering or model complexity.

## Conclusion & Future Work

The surprisingly strong performance of the Naive Forecast highlights the significant autocorrelation present in energy generation data, where the immediate past is a very strong predictor of the near future. This makes it an excellent baseline for any further model development.

**Future work could include:**
* Exploring more advanced time series models (e.g., ARIMA, Prophet, Neural Networks adapted for time series).
* Incorporating external factors like weather data (temperature, wind speed, solar radiation) which are highly correlated with energy generation.
* Further feature engineering to capture more complex non-linear relationships or multi-seasonal patterns.
* Implementing robust secret management for production environments (e.g., using Azure Key Vault or Managed Identities) to avoid hardcoding credentials.

## Files in this Repository

* `GermanEnergyForecastingProject.ipynb`: The main Jupyter Notebook containing the analysis, model training, and evaluation.
* `RawData/`: Folder containing the raw energy generation CSV files.
* `README.md`: This file.


## How to Run the Project (Locally)

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/M-TANVIR28/GermanEnergyForecasting.git](https://github.com/M-TANVIR28/GermanEnergyForecasting.git)
    cd GermanEnergyForecasting
    ```
2.  **Install Dependencies:**
    (You would list Python libraries here, e.g., `pip install pandas numpy scikit-learn xgboost matplotlib seaborn azure-storage-blob`)
3.  **Obtain Data:**
    Ensure you have access to the necessary Azure Blob Storage data (or update the notebook to use local data if you plan to include it directly). **Remember not to hardcode credentials!**
4.  **Run the Notebook:**
    Open `GermanEnergyForecastingProject.ipynb` in your preferred Jupyter environment (Jupyter Lab, Jupyter Notebook, VS Code with Jupyter extension) and run the cells.
