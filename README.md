📌 Paris Temperature Prediction

📝 Project Overview

The Paris Temperature Prediction project focuses on analyzing and forecasting temperature trends in Paris using historical weather data. As climate change becomes increasingly severe, accurately understanding temperature patterns is crucial for urban planning, agriculture, and resource management.

🎯 Objectives & Applications

Main Goals:

* Explore long-term temperature trends in Paris.

* Build predictive models using historical temperature data.

Real-World Applications:

* Support meteorological and environmental agencies in tracking climate change.

* Provide tools for policymakers, farmers, and researchers.

* Serve as a foundation for early-warning systems or environmental simulations.


📁 Project Structure

├── data/   
 │   ├── paris_temperature.csv    
├── notebooks/    
│   ├── eda_stationarity.ipynb    
│   ├── prophet_model.ipynb      
│   ├── daily_model_lgb_rf.ipynb      
│   ├── weekly_model_lgb_rf.ipynb     
├── src/      
│   ├── features.py      
│   ├── train_model.py      
│   ├── evaluate.py      
├── README.md


🔍 Methodology

The project applies a comprehensive data science pipeline, including statistical techniques and modern machine learning models, to forecast daily and weekly temperatures in Paris.

1. Test for Stationarity

Purpose: Determine whether the time series is statistically stable — a requirement for many forecasting models.

Method:

* Augmented Dickey-Fuller (ADF) Test

   * Null Hypothesis (H₀): The series is non-stationary (has trends or cycles).

  * If p-value < 0.05 → Reject H₀ → Series is stationary

2. Differencing Operations
   
If the series is non-stationary, apply first or second-order differencing to stabilize the mean.

df['Temp_diff'] = df['Temperature'].diff().dropna()

Goal: Flatten the series by removing upward/downward trends.

3. Quick Forecasting with Prophet
   
Goal: Use Meta's Prophet model for fast and robust time series forecasting.

Prophet Highlights:

* Automatically handles trends and seasonality.

* Allows integration of holidays/events for better accuracy.

Steps:

* Rename columns: Date → ds, Temperature → y

* Train and forecast with a basic Prophet setup.

4. Feature Engineering
Create new features from the datetime column, such as:

* dayofweek, month, quarter, is_weekend, dayofyear

* Lag Features: Previous days' temperatures


df['temp_lag1'] = df['Temperature'].shift(1)

df['temp_lag7'] = df['Temperature'].shift(7)

5. Daily Forecasting Models
   
Models Used:

* LightGBM Regressor: Fast, efficient gradient boosting for large datasets.

* Random Forest Regressor: Robust ensemble model, good against overfitting.

Steps:

* Split data into train/test sets (e.g., train until 2021, test on 2022).

* Use engineered features for model training.

* Evaluate results using:

     * MAE (Mean Absolute Error)

     * RMSE (Root Mean Square Error)

     * MAPE (Mean Absolute Percentage Error)

6. Weekly Forecasting Models
Preparation:

* Aggregate daily data to weekly using:


df_weekly = df.resample('W').mean()

* Re-apply feature engineering and lag creation.

Application:

* Train LightGBM and Random Forest on weekly data to predict next week's temperature.

* Weekly models help smooth out short-term noise and improve long-term patterns.

📊 Visualization & Evaluation

* Plot predicted vs. actual temperatures (daily & weekly).

* Compare model performance:

    * Prophet vs. LightGBM vs. Random Forest

    * Daily vs. Weekly Models

* Analyze prediction errors, especially at anomaly points or underperforming periods.
