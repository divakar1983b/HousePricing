# HousePricingModel - "Surprise Housing" 
> Surprise Housing a US Based company plans to enter Australian market. 
> The company uses Data Analytics to buy House at low rates and later sell at higher rates. 
> 
> 
>

*![Book logo](/pricing1.jpg)

## Table of Contents
* [Objective](#Objective)
* [Data Exploration](#Data_Exploration)
* [MultiColinearity](#Multicolinearity)
* [Model Estimate](#Model_Estimate)
* [Conclusion](#Conclusion)

<!-- You can include any other section that is pertinent to your problem -->

## Objective
- Model the price of house with the available independent variables
- Model shall be used by managament to understand how prices vary with the variable.
- Management can accordingly manipulate the strategy of the firm and concentrate on the areas which yields high returns.
- Model will be a good way for management to understand the pricing dynamics of a new market.
<!-- You don't have to answer all the questions - just the ones relevant to your project. -->

## Data_Exploration
-  Variables
    -   The dataset "train.csv" has 80 features with 1460 records for predicting "SalePrice" of an Australian house. 
    -   "SalePrice" is our target variable for which the regression model needs to be built
    -   On going through the data it is found that some of the variables such as "Alley, PoolQC, Fence, MiscFeatures" are having most of the values as NAN. Hence can be dropped. 
    -   Variables such as "YearBuilt, YearRemodAdd, GarageYrBuilt" are transformed to respective ages from time of sale using formulas such as  "YrSold-YrBuilt, YrSold-YearRemodAdd, YrSold-GarageYrBuilt" resp. and the year variables are dropped. 
    -   Unimportant variables such as "ID" are also dropped
-   Continuous Variables
    -   Histogram of continuous variable gives us insight of some of the insignificant variables such as "ScreenPorch, PoolArea, MiscVal, MasVnrArea, LowQualFinSF, EnclosedPorch, BsmtFinSF2, 3SsnPorch" which can be dropped
*![Book logo](/cont1.PNG)
*![Book logo](/cont2.PNG)
*![Book logo](/cont3.PNG)
-   Categorical Variables
    -   Countplot of categorical variable gives us insight of some of the insignificant categorical variables such as "BsmtHalfBath, Utlities, Street, RoofMatl, Landslope, LandContour, Heating, GarageQual, GarageCond, Functional, Electrical, Condition2, Condition1, CentralAir, BsmtFinType2,BsmtCond,KitchenAbvGr, PavedDrive, ExternalCond" which can be dropped.
*![Book logo](/cat1.PNG)
*![Book logo](/cat2.PNG)
*![Book logo](/cat3.PNG)
*![Book logo](/cat4.PNG)
*![Book logo](/cat5.PNG)
*![Book logo](/cat6.PNG)
*![Book logo](/cat7.PNG)
-  Zones & Subclasses
    -   On going through the data_description and data we can understand that the houses are majorly categorized according to zones. 
    -   "MSZoning" variable contains the zones of each house as "RL, RM, RH, FV, C(all)". 
    -   The distribution of the house sold reveals that almost "79% of house sold are from "RL" Zone, followed by "15%" in "RM" Zone and 4% in "FV"zone and minor % in RH and C(all) zones.
    -   The Houses sold in each zone can be further sub classified into 1-Story newer, 1-Story older, 2-Story newer, 2-Story older...etc which is captured in variable "MSSubClass" with values 20 to 190 each representing a particular class.
    -   The distribution of House sold in each zone based on sub class reveals following: 
    -   In RL Zone: 44% of the houses are 1-Story newer, 24% are 2-Story newer, 8% are 1.5 Story finished and minor % in other classes
    -   In RM Zone: 23% are 1.5 Storey finished, 15% of old 1Storey and 2 Storey, 10% of Planned unit and minor % in other classes
    -   In FV Zone: 38% are 2 Storey Newer, 34% Planned unit, 20% 1Storey newer and 8% 1Storey PUD

*![Book logo](/zone1.PNG)
*![Book logo](/zone2.PNG)
-  Outliers   
    -   The Box plot distribution of "Saleprice" based on houses sold in different zones and Subclasses reveals the outliers in the data which are removed for better model prediction.

*![Book logo](/mssub1.PNG)
*![Book logo](/mssub2.PNG)
*![Book logo](/mssub3.PNG) 


## Multicolinearity
- To understand the multicollinearity amoung the different features, we shall plot the heat map between the variables, as well as use the variance_inflation_factor from statsmodels

*![Book logo](/corr1.PNG)
*![Book logo](/corr2.PNG)
*![Book logo](/vif1.PNG)
*![Book logo](/vif2.PNG)
    
- From the heatmap we can see many highly correlating features 
- Such Correlating Features are procedurally analysed through VIF function and dropped one by one to optimum number.
- Similarly "hum", "fall" were also correlated with "atemp", which were confirmed by VIF function and elimiated one by one.
    *![Book logo](/heatmap2.PNG)

## Model_Estimate
- From the correlation matrix and other plots plotted above, we can clearly see that in the year 2019 the demand was comparitively higher than year 2018.
- But the factor(feature) which caused the increase in demand in year 2019 compared to year 2018 is perhaps not captured in the available features.
- In the correlation matrix the gains between the "cnt" and different features are not the same for year 2019 and year 2018, which confirms some unknown factor.
- For estimating the linear regression model and to get better correlation, the data needs to be split for year 2018 and 2019 and proceed with model building.
    - Linear regression models for year 2018 and 2019 were estimated separately as follows
        -   OLS models for year 2018 and year 2019 were estimated separately
        -   To identify and remove possible multicollinearities amoung features P-factor, VIF, RFE were looked into. 
        -   The model estimate were repeated for 4 runs and in each run a collinear feature with high P/VIF/RFE is removed to get the final model
        -   The final model estimate is shown below
- OLS Model:

     *![Book logo](/2018model.PNG)
     *![Book logo](/2019model.PNG)

- VIF & RFE analysis:

    *![Book logo](/vifmodel.PNG)*![Book logo](/rfemodel.PNG)
    
- R2 Score & Residual plot:
- 
    *![Book logo](/r2score.PNG)
    *![Book logo](/predresd.PNG)
    
- Test data Predictions:

  *![Book logo](/pred2018.PNG)
  *![Book logo](/pred2019.PNG)
  
## Conclusion
   -   From the above models and prediction we can see that 
        - "cnt" target variable can be predicted with
            -   "atemp", "summer", "winter" & "mist"
            -   "atemp" had the maximum positive influence and correlation on the shared bike demand
            -   "summer" & "winter" had minimal positive influence on bike demand
            -   "mist" had minimal negative influence on bike demand
        - For the year 2018 
            -   Model prediction are better with R2 value for train and test @ 0.761 and 0.77
        - For the year 2019
            -   Model predictions are good with R2 value for train and test @ 0.667 and 0.55, but not as good as year 2018
            -   Some unknown factor had an effect in increasing the demand because of which the R2 value had decreased
 
<!-- You don't have to answer all the questions - just the ones relevant to your project. -->
