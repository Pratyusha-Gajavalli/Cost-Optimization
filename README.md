# Cost-Optimization
Problem Statement and Objective:
To replenish the inventory of a diamond business with a budget of $5,000,000 towards the purchase of new diamonds, quotation with buying offer prices for all the new diamonds has to be submitted to vendors.

Given dataset has historical list of diamonds (attributes of diamonds) that were purchased from various distributor vendors and the prices they retailed for. Test dataset has new list of diamonds that are currently on the market, without any prices.

Feature Engineering & Data Cleaning: 
•	‘Vendor’ variable is converted to string as it represents a category
•	‘Measurements’ variable cannot be treated as categorical due to too many categories present in it. So, it is split into three different continuous variables length, breadth, and height
•	‘Symmetry’ and ‘Shape’ variables has misspelled categories, and they are corrected to avoid getting misspelled category treated as a different individual category
•	Empty values in all the variables are converted to nulls
Missing value treatment:
Percentage of null values in each variable is calculated. 
•	‘Cut’ variable has 48.72% (~50%) missing. Hence, it is dropped to avoid bias created by replacing nulls with some average value 
•	‘Cert’ and ‘Clarity’ variables have low percentage of missing data. Hence, these data values are replaced by mode
•	‘Symmetry’ and ‘Polish’ variables have considerable amount of missing data. So, change in variation of target variables aggregate among categories, when missing values are replaced by mode, is calculated. Since, the variation is almost retained, missing values are replaced by mode
•	‘Table’ and ‘Depth’ variables has huge percentage of missing data. In this case, these missing values are replaced by mean value
Multi-collinearity check:
Presence of correlation among features decreases the accuracy of models. Dummy variables are created for all the categorical variables and correlation value for each pair of variables is generated. Since there is high correlation among some pairs, vif values are also calculated. From this analysis, it is observed that multi collinearity exists among features ‘length’, ‘breadth’, ‘height’, ‘Table’, ‘Depth’, ‘Color’.
Data splitting into train and validation sets:
Dataset with dummies is split into train (~85%) and validation (~15%) sets for both ‘LogPrice’ and ‘LogRetail’ variables as target. 
Modelling:
Linear Regression, Random Forest and Gradient Boosting models with default parameters are fitted to train dataset with ‘LogPrice’ as target variable and predicted values are calculated on validation set, resulting in RMSE values as 0.1961, 0.1922, 0.2128 respectively. Same procedure is repeated with ‘Retail’ as target variable, resulting in RMSE values as 0.2426, 0.2468, 0.2624 respectively. Since these algorithms are giving low RMSE and good fit to our data, other regression algorithms are not explored. 
Understanding Feature Importance:
Based on p-values of coefficients of variables, it is observed that ‘Depth’, ‘Table’, ‘Cert’, ‘Regions’, ‘Known_Conflict_Diamond’ variables are not significant in determining ‘LogPrice’ value and ‘Depth’, ‘Cert’ variables are not significant in determining ‘LogRetail’ value.
Selecting best model:
It is observed that R square and RMSE values for all the three algorithms are very close. Hence, Linear Regression model is chosen for its better interpretability.
Training on entire data and Prediction:
Same data cleaning steps are repeated to the test data. Dummies for missing categories of some features in test data, that are present in train data, are created with value as zero. Dummies for extra categories, that are not present in train data, are dropped. Linear regression model is fitted to entire dataset and ‘LogPrice’, ‘LogRetail’ values are predicted for test data. These predicted logarithmic values are converted to normal values representing predicted ‘Price’ and ‘Retail’ values.
Optimization of profit:
Predicted ‘Retail’ value is assumed to be the potential selling price and predicted ‘Price’ value is assumed to be the potential buying price of a diamond. This means, a particular diamond can be bought only if offer price is greater than or equal to predicted ‘Price’ value. Difference between these two prices is considered as our profit for a diamond. It is given that $5,000,000 is the budget available to buy the diamonds, such that profit generated is maximum. Hence, only some of these diamonds in test data should be chosen based on profit they can generate.
To achieve this, a binary decision variable is created representing whether a diamond should be picked or not. Hence, offer price of a diamond is decision variable multiplied by predicted price value and profit from this diamond is decision variable multiplied by difference of predicted retail and price values. These equations are created along with budget constraint and solved through Linear Programming.

