# **Problem Statement**

Private equity-run real estate development firms have been on the rise for several decades now. These firms are often out of sync with the local communities in which they build. This can lead to discord, gentrification and a general lack of community input into the building process. 

Our aim with this project is to answer the following question: How can we empower **local** developers in Ames, Iowa and equip them with the knowledge of what consumer needs drive home sale price so that they can compete with the private-equity backed 'big dogs.'

# **Data**

The 
[dataset](https://www.kaggle.com/emurphy/ames-iowa-housing-prices-dataset) includes data from over 2,000 homes in Ames, Iowa with over 80 columns of information pertaining to the interior, exterior, plot characteristics and location of the house.  The data dictionary can be found [here](https://cran.r-project.org/web/packages/AmesHousing/AmesHousing.pdf).

# **Overview of the project notebooks**
This project is composed of three notebooks. Notebook 1, is dedicated to cleaning and exploring the train data in order to get it ready for processing and modeling. Notebook 2 performs the same transformations on the data as this notebook but for the test not the train data. Notebook 3 is where the magic happens - here we will use four regression models with robust evalution of each model, in order to predict the saleprices and analyze the model's ability to make accurate predictions on other datasets. After presenting the models and evaluations, we will conclude the notebook with recommendations designed to aid the primary stakeholders identified in our problem statement. Finally, we will provide a few thoughts on further steps for this project.

# **Overview of Findings and Analysis**

## Model 1: Multiple Linear Regression
Overall the miltiple linear regression model performed significantly better than baseline with a testing RMSE of 30,050. Furthermore, the training and testing R2's were very close to eachother (.868 and .851 respectively) which does not raise concerns regarding generalization of the model to new datasets.

The 10 most important features indicated by this model are: 
- Neighborhood_StoneBr	44457.227407
- Neighborhood_NridgHt	28379.369655
- Neighborhood_NoRidge	23324.774226
- Exter Qual	14566.075020
- Kitchen Qual	13308.101193
- Overall Qual	10313.786241
- MS Zoning_RL	8852.892174
- Central Air_Y	3019.582074
- Heating QC	2918.766749
- Fireplace Qu	2704.331520

So the linear regression model is giving a lot of weight to the location of the house, especially in comparison to the later models. 

## Model 2: Polynomial Features Regression

Overall the  polynomial regression performed significantly better than baseline with a testing RMSE of 26,506. The training and testing R2's were close enough to eachother (.915 and .884 respectively) which indicates some overfitting of the model which can probably be attributed to the variance caused by introducing numerous polynomial features. 

The model outperforms the linear regression model handily with an improvement of about 3,500 units on the RMSE value. However it has a slightly larger spread between the training and testing R2s than Model 1 indicating it is potentially more overfit and has less generalizability.

The coefficients in the model indicate several areas where some colinearity might be creeping in and gives the opportunity to develop a different feature list.

## Model 3: Lasso (Production Model)

Overall the lasso model performed significantly better than baseline with a testing RMSE of 26,573. Furthermore, the training and testing R2's were very close to eachother (.895 and .883 respectively) which does not raise concerns regarding generalization of the model to new datasets.

The model is roughly equivalent in terms of its performance in RMSE to Model 2 (M2 RMSE: 26506), however it has a slightly smaller spread between the training and testing R2s than Model 2 (M2 R2 spread was 3 percent points) indicating it is potentially less overfit and has more generalizability.

The 10 most important features indicated by this model are: 
- gr_liv_area	18849.272639
- Overall Qual	15229.251278
- Exter Qual	9100.979270
- BasmtFin Sqft	8551.729808
- Kitchen Qual	7277.844457
- Neighborhood_NridgHt	7050.274077
- lot_area	6792.809039
- Total Bsmt SF	6682.241935
- Garage Area	6285.239390
- Neighborhood_StoneBr	5458.968152

## Model 4: Ridge Regression
Overall the ridge model performed significantly better than baseline
with a testing RMSE of 26,638. Furthermore, the training and testing R2's were very close to eachother (.895 and .883 respectively) which does not raise concerns regarding generalization of the model to new datasets. 

It is almost exactly the same as the Lasso model in terms of performance with an almost negligible difference in testing RMSE (64 RMSE lower) and almost identical R2 spread. This means it outperforms the simple linear regression model and slightly underperforms in comparison to the linear regression model with polynomial features but is potentially less overfit. 

The 10 most important features indicated by this model are: 
- Overall Qual	13854.222537
- Exter Qual	9092.907869
- gr_liv_area	9016.467822
- total_sqft	9016.467822
- BasmtFin Sqft	8014.327998
- Kitchen Qual	7449.568864
- Neighborhood_NridgHt	7021.861669
- Total Bsmt SF	6691.077089
- lot_area	6513.181417
- Garage Area	5689.112358

# **Conclusions**

Over the course of this notebook I have developed four models as well as a baseline to model the data that was cleaned and explored in Notebook 1. (Notebook 2 was used for cleaning the test data). Evaluation of each model has been given at the end of each model. 

Overall the production model I chose to go with was the Lasso model given the good RMSE, decent R2 scores, lack of overfitting, significant overperformance compared to baseline and chance to control for multicolinearity/do some of the feature engineering automatically.  Results are below. This generates several interesting recommendations that will be presented to our target audience in the presentation with accompanying visualizations.

The 10 most important features indicated by this model are: 
- gr_liv_area	18849.272639
- Overall Qual	15229.251278
- Exter Qual	9100.979270
- BasmtFin Sqft	8551.729808
- Kitchen Qual	7277.844457
- Neighborhood_NridgHt	7050.274077
- lot_area	6792.809039
- Total Bsmt SF	6682.241935
- Garage Area	6285.239390
- Neighborhood_StoneBr	5458.968152

Given that we are making recommendations to local developers in Ames, Iowa we want to give specific and actionable recommendations and not a list of coefficients! As such, I've broken down the interesting coefficients into four groups.

1. **House Characteristics** - Features innate to the house like  square footage, kitchen size and quality, fireplaces, storeys etc.
2. **House Add-ons** - Features not directly related to the main living area like decks, basement size, garage space etc.
3. **Plot Characteristics** - Features related to the land on which the house sits like land size, frontage onto the street and gradient of land.
4. **Location** - Where the house sits within the greater city and area - specifically neighborhoods of interest.

# **Recommendations**

Given our analysis we would recommend that developers do the following for each category:

1. **House Characteristics:** **Total square footage** above ground level as well as **internal and external quality** are key areas of focus. Furthermore, aim to design a **high quality kitchen.**

2. **Add-Ons:** Include a **large finished basement**, plenty of **garage area**, and a **large basement** in general to attract a higher sale price.

3. **Plot Characteristics:** **Total lot area**  or the size of the plot of land as a whole  is of importance to consumers, even more than the size of the garage and total basement area.

4. **Location** - The **North Ridge Heights** and **Stone Bridge** neighborhoods tend to correlate with higher saleprices. Developing an understanding of why this is the case is important.

Although these have direct relevance to our primary stakeholders (local developers) we think this is valuable data for secondary stakeholders like individuals that wish to flip houses, appraisers, tax assessors etc.

# **Next Steps**

Given additional time and data, in the first instance, there are a few areas I would like to analyze more deeply:
1. **Locational Analysis:** With additional data we’d like to explore why some areas in Ames have a larger effect on saleprice than others (correlation or causation?) and help bring this insight to understand up and coming areas in the Greater Ames area.

2. **Details:** With the current dataset we’d like to better understand how different aspects of house characteristics and types of features interact and model these interactions to better understand customer choices.

## **Sources of Note**

The following sources were consulted for ideas for how to clean the data and approach the dataset as a whole.
[Source 1](https://medium.com/mlearning-ai/a-thorough-dive-into-the-ames-iowa-housing-dataset-part-1-of-5-7205093a5a53)
[Source 2](https://nycdatascience.com/blog/student-works/predicting-house-prices-using-the-ames-iowa-house-dataset/)
[Source 3](https://www.perkinsml.me/ames-housing)
[Source 4](https://towardsdatascience.com/wrangling-through-dataland-modeling-house-prices-in-ames-iowa-75b9b4086c96)