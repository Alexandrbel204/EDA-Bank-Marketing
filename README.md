# EDA-Bank-Marketing
In this project the focus was on data analysis and working with data. The purpose was to work wirh data carefully and create outstanding ML models that would classify the new potential subscribers for new bank's deposit.

This repository contains 2 files, first one with code and description, the second one is report. Actually they are quite similar, the difference is that in Jupyter file you can observe code better or even fix it while in another you can easily read my notes, observations and conclusions

Here is the info about dataset
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

