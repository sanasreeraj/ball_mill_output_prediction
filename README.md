## Description

This project involves the development of a predictive machine learning model for a Ball Mill classification system. The objective is to estimate the proportion of ground material retained in three specific sieve size ranges:

- Greater than 35 mesh (>35) – Coarse material
- Between 35 and 200 mesh (35 to 200) – Intermediate material
- Less than 200 mesh (<200) – Fine material

The model is built using historical process data, including parameters such as feed rates, motor currents, system temperatures, and differential pressures. A Random Forest Regressor is used to perform multi-output regression, offering high accuracy in predicting material distributions.

The resulting model is intended for real-time deployment in industrial environments, such as SCADA (Supervisory Control and Data Acquisition) or DCS (Distributed Control Systems), enabling proactive control strategies and continuous process optimization.

---

## Methodology

### 1. Data Collection

The dataset comprises:
- Ungrounded Rock Data (UGR)
- Grounded Rock Data (GR)
- Associated real-time process parameters

These datasets include both sensor-generated and manually recorded values.

---

### 2. Data Preprocessing

A robust data preprocessing pipeline was implemented to ensure data quality and consistency before model training. Key steps include:

- **Filtering Inconsistent Samples:**  
  Records were filtered based on defined minimum and maximum thresholds provided in `minmax.xlsx`. Columns with inconsistent or missing range definitions were logged for manual inspection.

- **Handling Missing Values:**  
  - For numeric columns: missing values were replaced with the column mean.
  - For categorical/object columns: missing values were filled using the mode (most frequent value).

- **Sensor Fault and Shutdown Removal:**  
  Records containing invalid entries such as `'BAD'`, `'ERROR'`, negative values, or non-numeric noise were removed. This ensured only valid operating states were used for training.

- **Outlier Detection and Removal:**  
  Outliers were removed using the Interquartile Range (IQR) method, calculated between the 10th and 90th percentiles. This helps in minimizing the influence of anomalous readings.

- **Feature Engineering:**  
  A new feature named `combinations` was created to represent material mix ratios (e.g., `T:60:A:40`) by combining the non-zero values of key feedstock components. This feature adds context to the operating condition for each record.

---

### 3. Data Visualization

To better understand the data distribution and variable interactions, several exploratory visualizations were generated:

- **Boxplots:**  
  Used to observe feature-wise data spread across different material combinations and to identify extreme values.

- **Pair Plots:**  
  Provided pairwise comparisons of numerical features, helping in identifying correlation and possible multicollinearity between features.

- **Correlation Heatmap:**  
  Visualized the Pearson correlation coefficients between all selected features and output variables. This helped identify strongly and weakly correlated features to guide feature selection.

---

### 4. Feature Set

The following process parameters were selected as input features based on domain knowledge and correlation analysis:

- Weigh_Feeder_rate_A (TPH)
- Weigh_Feeder_rate_B (TPH)
- Ball_Mill_Amps
- Ball_Mill_Vent_System_Temperature (°C)
- Ball_Mill_Vent_Fan_Amps
- Bag_Filter_Differential_Pressure (mmWC)
- Screw_Conveyor_Current (Amps)
- Bucket_Elevator_Current (Amps)
- Material composition ratios (T, A, S, M)
- Additional derived features such as combinations and other system readings

---

## Modeling Approach

### Model Used: Random Forest Regressor

Random Forest was selected due to its robustness, ability to model non-linear relationships, and support for multi-output regression. The model offers the following advantages:

- High tolerance to outliers and noisy data
- Capability to handle multicollinearity and unscaled features
- Provides feature importance rankings to understand input impact

Model hyperparameters were selected based on grid search and empirical tuning.

---

## Model Evaluation

The trained model was evaluated using a test subset consisting of 15 randomly selected data points:

- Predictions were accurate for approximately 12 of the 15 samples.
- The remaining 3 samples had minor deviations from the actual values.
- Evaluation metrics include:
  - Coefficient of Determination (R² Score)
  - Mean Squared Error (MSE)
  - Visual comparisons of predicted vs. actual sieve size distributions
