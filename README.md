# EDA-Bank-Marketing
In this project the focus was on data analysis and working with data. The purpose was to work wirh data carefully and create outstanding ML models that would classify the new potential subscribers for new bank's deposit.

This repository contains 2 files, first one with code and description, the second one is report. Actually they are quite similar, the difference is that in Jupyter file you can observe code better or even fix it while in another you can easily read my notes, observations and conclusions

### Purpose of the project:
* Research and work with financial/bank data
* To find clients that most likely will take a deposit
* Perform EDA to explore and clean data
* Apply ML methods to solve the task

### Ultimate goal of the project
1) To find potential clients that will subscribe to deposit
2) Find as many target-client as possible
3) Ensure that business loss is minimized


Here is the info about dataset
• Data source: https://archive.ics.uci.edu/dataset/222/bank+marketing
• Dataset size: 16 features and 45211 observations
• Missing values in columns ‘contact’, ‘pdays’, ‘poutcome’
• Disbalance in target

![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/desc1.png)
![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/desc2.png)

In 3 columns there are missed value, but alreday imputed with ‘unknown’ values or -1.
Target is unbalanced.

#### There is no missing values in direct meaning, but we see, that such columns as “education” and “contact” has many unknown values and pdays has issues with -1 number
* job : 288
* education : 1857
* contact : 13020
* poutcome : 36959

No duplicates.

It is not useful to impute the columns with “unknown” values because:
* job - means there is just no data and this is not “unemployed” as there is different category for that
* education - there is no point in rename “unknown category” and this, again, shows just missing values 
* contact - it again shows just error (missing) in original data and there will not be problem for ML
  
But pdays is much more interesting and we can consider -1 as Nan as it is no more than absense of contact with clients, I imputed them as “no_previous”, but as it is numerical value I put it in different column and initial problem will replace with 0 which literally means that clients didn’t have previous contact.

#### I will make some plots to see the distribution of data and descriptive statistics:
* Histogram + KDE
* boxplot
* Q-Q plot
* log transformation (if appliable)

Here you see the first of the features plotted as an example:
![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/stat1.png)

#### All the results of distribution analysis:
1) Age
* Quartiles are: 33, 39, 48
* Distribution is close to normal but still got a tail to right with some exteme values
* Median is 39 (have outliers for right)
* Log scaling relatively handles inequalty in distribution
* By target, medians are almost equal, but IQR in ‘yes’ is quite higher than ‘no’
2) Balance
* Quartiles are: 72, 448, 1428
* Distribution is much skewed to right and have big tail
* Median is 448 and lot of outliers (especially for right part)
* Log scaling is not applicable as there are negative values (that indicate debts)
* By target, they are almost equal, both has many outliers, but for ‘no’ there are much more outliers
3) Day
* Quartiles are: 8, 16, 21
* Distribution looks like Uniform
* Overall, there are no outliers as their range is (0, 31)
4) Duration
* Quartiles are: 103, 180, 319
* Distribution is skewed to the right and got many extreme values
* Median is 180 and lot of outliers (for right)
* Log scaling is not applicable as there are 0 values
* By target, both have many outliers, but ‘no’ has more, but IQR and quartiles are significantly lower than ‘yes’
5) Campaign
* Quartiles are: 1, 2, 3
* Distribution is skewed to the right and got many extreme values
* Median is 2 and lot of outliers (for right)
* Log scaling can’t handle outliers
* By target, ‘no’ has more outliers, but IQR and quartiles are significantly lower
* By target, both are similar in IQR and a huge number of outliers, but ‘no’ has more outliers
6) Pdays
* Quartiles are: 0, 0, 0
* Distribution is skewed to the right and got many extreme values
* Most imbalanced feature, almost all values are outliers
* Log scaling is not applicable as there are 0 values
* By target, ‘no’ consist almost fully of outliers, while ‘yes’ IQR is ~ 100 and also got a lot of outliers
7) Previous
* Quartiles are: 0, 0, 0
* Distribution is skewed to the right and got many extreme values
* Most imbalanced feature, almost all values are outliers
* Log scaling is not applicable as there are 0 values
* By target, both consist almost fully of outliers, but ‘no’ stands out because of one enormous outlier among others


Next step is data visualization that helps understand the relationship between features:
![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/vis1.png)
![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/vis2.png)

Again, there is just an exapmle, for all samples check my report (pdf file)

#### Interpretation of visualizations:
* Retired and student take deposts significantly higher that others
* Blue-collars, managment and technicians are majority in jobs
* Most of people are married
* Single people take deposits more often
* Most of people have secondary education
* People with tertiary education usually more often take deposits
* Most of people don’t default and they take more deposits
* More people have housing loan but than no and less take deposts
* Majority of people don’t have personal loan and they take deposits more often
* Majority of people contact in cellular way and take more deposits
* Most of deposits are taken in May with a huge separation, but in % division the leaders are October, September, March and December
* Outcome of the previous marketing campaign is more unknown, but in % division it is ‘success’

Heatmap correlation:
![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/heat.png)

#### Heatmap interpretaion
1) Positive correlations
* Balance & Age (0 - 0.25)
* Day & Campaign (0 - 0.25)
* Day & no_prev_contact (0 - 0.25)
* Campaign & no_prev_contact (0 - 0.25)
* pdays and previous (0.25 - 0.50)
2) Negative correlations
* previous & no_prev_contact (0.5 - 0.75)
* pdays & no_prev_contact (0.75 - 1)
Overall, negative correlations are mostly stronger, while postives are more but weaker. Most correlated features are ‘pdays’ and ‘no_prev_contact’

#### Checking Outliers
After applying IQR:
* lower = q1 - 1.5 * IQR
* upper = q3 + 1.5 * IQR
We see that:
- pdays: 18.26% values — potential outliers (IQR).
- previous: 18.26% values — potential outliers (IQR).
- no_prev_contact: 18.26% values — potential outliers (IQR).
- balance: 10.46% values — potential outliers (IQR).
- duration: 7.16% values — potential outliers (IQR).
- campaign: 6.78% values — potential outliers (IQR).
- age: 1.08% values — not many outliers.
- day: outliers are (0.00%).

#### To deal with outliers I am going to use:
1) Winsorization
* change outliers on border values
* do not delete anything
* good for financial data
* make distributions more ‘normal’
2) Log-transform
* Applied only to columns with positive values
* Deals with huge assymetry in data
* Not applicable to zero or negative values

![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/win1.png)
![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/win2.png)


### ML part
I mostly used one-hot encoding and LabelEncoder for binary columns and SMOTE to balance data

##### List of models I will use:
1. Logistic Regression
* captures global dependencies well.
* Interpretable (coefficients can be viewed).
* Usually weaker than trees, but has a good baseline.
2. Random Forest
* More powerful due to the ensemble of trees.
* Better at capturing complex dependencies.
* Robust to outliers and multicollinearity.
* Almost always produces good results.
3. XGBoost
* The most powerful model for tabular data.
* Can highlight weak signals in the data.
* Typically has the highest ROC-AUC.
* Provides good interpretation using feature_importances and SHAP.

The results were not satisfying for me, so I used threshhold tuning for Recall and F1
![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/res1.png)
![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/res2.png)
![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/res3.png)

### Final best result
XGBoost with threshold = 0.35
- precision = 0.57
- recall = 0.67
- f1-score = 0.62

Here is its feature importance plot
![Image alt](https://github.com/Alexandrbel204/EDA-Bank-Marketing/blob/main/pictures/f1.png)

#### Two factors are important for bank deposit campaigns: 
1) not missing a client who might agree (Recall)
2) not wasting budget and time on “empty” calls (Precision)

## Conclusion
The dataset shows strong skewness, heavy-tailed financial-like distributions, and several categorical variables that meaningfully differentiate subscribers from nonsubscribers. Correlations are mostly weak, indicating that each feature provides unique information. After applying IQR-based winsorization, selective log transforms, and careful handling of
missing categories, the dataset is clean, well-structured, and ready for machine learning.

The optimal solution is XGBoost with threshold = 0.35, providing the best balance between
business efficiency (Precision) and customer coverage (Recall). The model is well-interpretable
through feature importance, stable across metrics, and aligns with expected marketing behaviors.
This makes it the best candidate for deployment in a bank deposit campaign.

## Future work and recommendations
1. Improve Feature Engineering
2. Handle Duration More Robustly
3. Use More Advanced Models
4. Hyperparameter Optimization
5. Time-Aware Validation
6. Cost-Sensitive Learning
7. Explainability & Monitoring
8. Deploy and Integrate with Business Workflow





