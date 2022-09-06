## Objective

Assuming the role of a consultant, the project consisted on developing a model to forecast how many claims will each policy holder from a car insurer in France have in the following year. The insurance company wants to use this model to improve the policies' premiums (pricing).

The Project objective consists in developing a predictive data analytics solution for a French insurance company, following a CRISP-DM methodology. The developed model seeks to forecast the number of claims each policyholder will have in the following year. By having this information, the insurance company could adjust its pricing model for the next year’s premiums according to the predicted number of claims.

**Business Understanding** 
phase of the CRIPS-DM framework is somehow compromised since we can’t contact the insurance French company and interact with its associates and workers. It would be interesting to contact a sales team member and observe their daily activity. Besides the top management, it would be also fundamental to contact the IT department to assess the analytics and the data availability. One could also already get some key insights of the company’s data. Given the business objective, the target variable defined in this problem is the number of claims.

## Summary Statistics of the DataSet
The data presented in freMTPL2freq has a total of 678 013 observations, described along 12 different variables/attributes/descriptors/features.

The minimum and maximum values of all the numeric features seem to be in line with the variables’ units. Although it is possible to start questioning the maximum values of the features: Claim Numbers, Exposure and Vehicle Age.

## Data Understanding
The previous summary statistics and data overview also concern the Data Understanding phase. It is crucial to deepen the data understanding and examine the dataset and hence it is further verified the presence/absence of missing values, followed by a graphical visualization. It is confirmed that the data doesn’t present missing values.

Regarding the attributes’ type, there is 4 categorical variables: Area, Vehicle Brand, Vehicle Gas and Region, with the remaining variables being numeric.

### Categorical Variables
The number of levels from the Area, Vehicle Brand, Vehicle Gas and Region are 6, 11, 2 and 22, respectively. The Area’s levels are attributed alphabetically from A to F, with the areas C, D, E and A being the ones with the most observations. 

Concerning the Vehicle Brands, there are unknown and classified with the letter “B” followed by a number. The most observed Vehicle Brands are B12, B1 and B2. The Vehicle Gas classification is straightforward being determined as Regular or Diesel and is almost equally distributed with 48.99% being Diesel and the remaining as Regular. The Region is the feature with the highest levels (22). These are named with an “R” followed by a number, and according to the description, it follows the standard French Classification. 

The most observed regions among the policyholders are R24, R82, R93 and R11. It was further computed a graphical visualization of some relationships between variables, to gain further insights.

### Exploration of Categorical Variables - visual representation
1st graphic: As one can observe, regions R24, R82, R94 and R11 are the areas with the most observations. Brands B12, B1 and B2 are the ones with more emphasis on the 1st graphic. There are many regions with lower observations, for instance below 20,000 which are possible candidates to group later.

2nd graphic: When comparing the areas with the vehicle brands, it is possible to infer that areas C, D and E are the most populated among the policyholders with vehicles of brands B1, B2 and B12. Area F is the one with the least observations which later could be also grouped with another region, possibly region B.

3rd graphic: It is compared the number of claims with the policyholders’ regions. Region R24 is the region with the highest observations. There are also many regions with few observations. So, it is important to consolidate this graphic with the percentage of claims of each region. The region with the highest percentage of zero claims is R23 (96.7668%) but it has few observations and we can see on the graphic. Region R53 is the one with the lowest zero % claims (93.8868%) and it has a significant number of observations. Additionally, the region with the highest number of claims of an individual policyholder (16) is region R91 and with 11 individuals’ claims is again region R91 and also region R24.

4th graphic: From this graphic brands B1, B12 and B2 are the ones with the most observations and probably with the highest presence of different claim numbers. By consolidating with the cross table, we can see that brand B1 is the brand with the largest number of claims per policyholder, followed by brand B2. There’s also some brand with few observations that might be candidates for category grouping (B14, B13, B11, B10)

### Numerical Variables
From the histograms it is possible to get a glance of the numeric variables’ distributions. Clearly the predominant number of claims is zero, with few observations having one or more claims. This is a sign that the data is not balanced but also it makes sense given the context that this is an insurance company and the number of claims is usually way less than the number of policies. The vehicle power demonstrates a positive skew, as well as vehicle age, driver’s age, bonus malus and density. This is also in line with the median and mean values.

Boxplots provide insights regarding the possible existance of outliers for each variables. Using the ones plotted above (for the numerical variables) some of them clearly present outliers: ClaimNb, Exposure, VehPower, VehAge, DrivAge and BonusMalus. As for the density variable, higher density does not necessarily represent the existance of outliers.

Using the figure above, where it is represented the pearson correlation between the numerical features, some pairs present relatively higher correlation between them when compared to others. For instance, BonusMalus and DrivAge (0,48 in absolut terms).

## Data Preparation
### Outliers
DriveAge: As the goal is to infer the number of claims per year, elderly people have higher claim frequency and thus the 1% and 99% percentiles were considered as outliers.

Area: As for this variable, higher values do not necessarily represent the existance of outliers since it is correlated with the number of inhabitants.

Exposure: The default exposure is one and thus values higher than this were considered as errors and assumed to be 1.

ClaimNb: Using the boxplots, number of claims per year higher than 7 were considered as outliers as the number os instances with these values was low.

VehAge: The older vehicles in this data set were considered outliers, since they can represent an old car collection or even a museum's collection since those vehicles still need insurance, but for the scope of our project these vehicles are not relevant to number of claims. Based on the graph, the outliers were considered for vehicles with age above 35 years.

Driver’s Age: The top and bottom 1% were considered as outliers.

Bonus Malus: Taking into consideration that values above 100 are considered "Malus" and the maximum value in our sample is 230, observations higher than the intermediate point between those (163) were considered outliers.

### Group variables
Region: The regions with less claim observations were aggregated, this includes zones with less than 20 000 observations, maintaining the regions with bigger claim quantities.

Driver’s Age: Based on the plot, we can identify four stages: growth, stable, descending and residual. We grouped Driver’s Age from age 18 to 30 (young adults), 31 to 52 (adults), 53 to 75 (mature) and above 75 (elderly).

Vehicle brand: The vehicle brands were grouped according to the claims observations which brand had, meaning brands with less than 20 000 claim observations and the brands with less than 40 000 were grouped together.

Vehicle Power: The vehicle power categories with less than 20 000 claim observations were grouped together.

The IDpol variable was eliminated because it does not explain the number of claims.

After computing the regression itself, using the values obtained for the p-value of each variable, we reached to the conclusion that Region 53 and Region 24 should be group to better explain the target variable (claim frequency).

After running the model for the first time, we decided to evaluate the explanatory power of each variable using the obtained p-value for each. We reached to the conclusion that some of these variables, according to this measure, were not statistically significant to explain the target variable. Thus, we proceeded to iteratively eliminate those, analysing the new p-values each time. By the end of all iterations, we decided to drop variables Region, Area, VehBrand and VehGas.

## Logistic Regression
The dataset was split into train and test using the below function:

A summary statistics was created since it was considered relevant to determine which variables should be dropped from our dataset for this regression.

A RFECV graph was plotted and allowed to conclude that: 
  - After 4 features, the score increases almost exponetially (corresponds to the VehAge variable).
  - After 6 features the evolution of the score stabilizes

Using the Learning Curve graph above, one can observe that, as the training instances increases, the gap between the training score and the cross validation score decreases.

Despite the fact that the results obtained by the model in the training set are similar to the ones obtained using the test set, all obtained measures show us that the model predictive power can be considered weak. One possible reason for such results is the fact that the initial dataset is extremely inbalanced (many more observations reporting zero claims than higher than zero).

## Decision Tree
**The Data Preparation process for the Decision tree followed the same rationale used before, for the linear regression model.**

Simmilarly to the Linear Regression Model, the Learning curve obtained for the Decision Tree model follows the same evolution as training instances increase, i.e, the gap between training score and the cross validation score tends to decrease.

As one can observe from the obtained coeficients, all of them influence positively the number of claims per year. Also, it is possible to observe that some of them are zero, which means that those specific variables (for instance, Region 31 does not explain the number of claims even though Region 91 does.

Similarly to what was observed using the Linear Regression Model, for the Decision Tree model the results obtained in the training set are similar to the ones obtained using the test set. Evethough, onde again, all obtained measures show us that the model predictive power can be considered weak.

Initialy, one could infer that the Decision Tree Model should return more robust results. Although, this was not the case. One possible reason for such is the fact that the initial dataset is extremely inbalanced (many more observations reporting zero claims than higher than zero). As well as that, a higher number of observations, meaning, a larger dataset, could lead to better results.

**Hope you find this project interesting!**
