# Predicting Dengue Fever in San Juan

## Background
Dengue fever is a flu-like disease most prevalent in the tropical and subtropical areas of the world. In most cases, symptoms are similar to the flu. However, severe cases can result in hospitalization and even death. The disease is spread through mosquitoes, and therefore, the spread of the disease is highly correlated with climate factors. Understanding the relationship between climate and spread of the disease is one of many important steps to reduce the transmission of the virus.

## Methodology
The objective of the project was to predict dengue fever cases in San Juan, Puerto Rico using nearly two decades of weekly cases and climate data. I ultimately used a weighted average ensemble of several machine learning models to predict weekly dengue fever cases.
To begin, I explored the dataset. I found missing data and inconsistencies and visualized distributions and correlations. With this information, I set up a preliminary preprocessing pipeline that imputed missing data, addressed other data quality issues, and standardized features. 
Because this problem was time-dependent, I set up a time series cross-validation framework and established baseline performance using a simplified model. Then, I cross-validated several machine learning models (Poisson regression, random forest, XGBoost, histogram-based gradient boosting (HGB), and k-nearest neighbor (KNN)) and evaluated their performance using mean absolute error (MAE). 
To further improve performance, I created a more advanced preprocessing and feature engineering pipeline, including lagged features, and a custom cross-validation framework to incorporate recursive forecasting. I then reevaluated performance of the previous regressors. Lastly, I used a weighted average ensemble of the models, achieving the best cross-validation performance of any method, a 50% reduction in MAE over baseline. 

## Data
Data was sourced from the site DrivenData. In total, there were 936 weeks of dengue fever cases and climate data from San Juan, Puerto Rico, which was stored in a CSV file. 

Dengue surveillance data came from the following sources:
U.S. Centers for Disease Control and Prevention
Department of Defense's Naval Medical Research Unit 6 and the Armed Forces Health Surveillance Center
U.S. universities

Environmental and climate data was provided by the National Oceanic and Atmospheric Administration (NOAA). 

## Results

### Key Insights: Cases
The total cases has a high concentration of low-valued numbers, is non-negative, contains a long right tail, and is discrete, count data. This indicates that the target, total cases, follows a Negative Binomial or Poisson distribution (if errors' mean = variance). 

![picture alt](https://github.com/eeorenstein/Dengue_Fever_Prediction/blob/main/cases_distribution.png)

There are months with lower and less variable total cases and months with higher average total cases that contain large outliers. It appears that half of the months belong to one group and half to the other. Also, these months are consecutive, indicating that there are two seasons of dengue fever infection and that weather features are likely a major contributor to the spread. The low spread and low variability season lasts from February through June and the higher spread, high variability season goes from July through January.

![picture alt](https://github.com/eeorenstein/Dengue_Fever_Prediction/blob/main/cases_by_month.png)

### Key Insights: Climate Features
Strong correlations exist between the temperature-related variables. However, none of the variables strongly correlate with total cases and all but three of the correlations are positive. Also, week of the year, which in a way encapsulates many of these variables, is surprisingly the strongest predictor. Temperature and humidity-related variables somewhat strongly correlate with total cases whereas precipitation-related and NDVI variables are weak correlators. 

![picture alt](https://github.com/eeorenstein/Dengue_Fever_Prediction/blob/main/features_heatmap.png)

### Comparison of Regressors
Besides KNN, all models performed much better than the dummy regressor. We also see that, for each model, the version with advanced feature engineering performed better than its preliminary processed counterpart. 

![picture alt](https://github.com/eeorenstein/Dengue_Fever_Prediction/blob/main/models_mae.png)

A weighted average ensemble of the advanced pipelined random forest, XGBoost, HGB with weights of 0.06, 0.41, and 0.53, respectively, yielded the lowest average cross-validated MAE (15.6). 

## Tools
* Pandas
* XGBoost
* Scikit-learn
* Seaborn

## Future Work
I could continue this analysis in a couple of directions. I could continue to better understand the relationship between total cases and the input features through more EDA. Just based on a surface-level understanding of the virus, we know that climate and the spread of the disease are important predictors of total cases. Exploring in greater detail what the "perfect storm" of climate featuers looks like in the weeks leading up to the current week could result in additional ideas for feature generation. I would also like to compare this performance with that of deep learning methods (e.g., LSTM) and more statistical-based approaches (e.g., ARIMA). 
