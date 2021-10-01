# Bank Helper



## Overview

Our stakeholder is a mortgage lender who would like more accurate appraisals to reduce risk for home loans. We analyzed the King County (CA) House Sales dataset using machine learning to develop a model for predicting the value of a house. The model accurately predicts the value of a house with information available to the bank at time of appraisal, and would be a good tool for making loan decisions. We recommend using this model along with existing appraisers to reduce risk and increase profit margins.


## Business Problem

Our stakeholder is a mortgage lender who wants to increase the accuracy of their appraisals in order to reduce the risk of default, especially loans which have the minimum possible down payment (20%) without Private Mortgage Insurance. These loans are worth 80% of the purchase price of the house. If a borrower defaults immediately, our stakeholder wants confidence they'll be able to re-sell the house and cover the entire loan. At the same time, they do not want artificially low appraisals, as those would drive clients to competing lenders. Specifically, we want to maximize the number of appraisals which are between 80% and 105% of the true value of the house in order to minimize risk while remaining attractive to borrowers.


## Data Understanding

This project uses the King County House Sales dataset, which can be found in `kc_house_data.csv` in the data folder in this repo. The description of the column names can be found in `column_names.md` in the same folder. As with most real world data sets, the column names are not perfectly described, so you'll have to do some research or use your best judgment if you have questions about what the data means.


- Where did the data come from, and how do they relate to the data analysis questions?
    The data come from house sales in King County, and they help us relate all of the features we are interested in (sqft, waterfront, renovated or not)
- What do the data represent? Who is in the sample and what variables are included?
    Only houses that have sold are in the sample, and variables include comparisons to nearby houses (_15 suffixed variables), metrics about the house that was sold and its lot (sqft), whether and when renovations were last done, and the original year it was built.
- What is the target variable?
    The target variable is the sale price of the house. A secondary target could be views; which could be used as a proxy for time-on-market.
- What are the properties of the variables you intend to use?
    Almost all of the variables we intend to use are numeric, except one binary variable (waterfront or not). Some of the variables are cyclic in nature (month), which we hope to capture in our feature selection.


### First Model

After decide use sklean, first thing to try is `LinearRegression()`.


## Modeling

Try different model:

* Ridge(random_state = RANDOM_SEED),
* BayesianRidge(),
* LinearRegression(),
* RandomForestRegressor(random_state = RANDOM_SEED),
* GradientBoostingRegressor(random_state = RANDOM_SEED),
* neural_network.MLPRegressor(solver="lbfgs", random_state = RANDOM_SEED)
* XGBRegressor() 

Use pipline with PCA, PolynomialFeatures or StandardScaler, use GridSearchCV to found the best hyperparameters:



Questions to consider:

We found explainability was good enough with partial_dependence plots, so we did not restrict our analysis to easily explainable models like linear regression. Explainability was less important because getting the right answer on average is the most important thing for making a profit as a mortgage lender.

Some variables had clear nonlinear effects (yr_built, latitude, longitude), which made it hard to get good performance from a linear model. We tried many different regressors built into scikitlearn with default parameters to decide which models were worth tuning. After we found that GradientBoostingRegressor was best, we decided to install and use xgboost (third party library for boosting decision trees) to see if that improved performance.

We decided xgboost was best, so we used GridSearchCV to find good hyperparameters without overfitting. We had to leave it running overnight several days in a row, but the results got us very close to our goal R^2 of .9.


## Conclusions

The model is very good at predicting house prices in 2014-2015. Training on data outside this period will be necessary to help it understand larger trends in housing prices.
Using this model to appraise houses nearly guarantees interest made from loans will cover money lost to bad appraisals + default, even under very adverse assumptions (12% foreclosure rate, 2% APR).
We assumed the market remained stable during foreclosures; we did not analyze the case where a market crash depresses housing values simultaneously with default. That could increase losses significantly in a worst case scenario
This model could definitely generate a profit, but client should work with us to determine expected ROI using more realistic assumptions of default rate and APR to evaluate whether this model is more profitable than their existing process.
