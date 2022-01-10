# KABAM IN_GAME SPENDING HABIT ANALYSIS

A game designer has posed the challenge of identifing users that are likely to spend money in their game after finishing the tutorial. The goal is to identify
new users who are likely to spend money in the game and present them with different prices. This is a binary classification problem, where only the user and device data will be available for prediction at test time. As I suspect the data will be imbalanced, we will use the ROC/AUC to select a model and also look at the Gain-Chart and Uplift for a more holistic view of the solution

Here are my first observations:


1.   The total_spend data is presented as continuous value rather than binary.
2.   Looking at the dataset statics and noticed there is a large class imbalance with most users not spending any money. This will later require rebalancing as we train our algorithm.
3.   There are several entries with NUll/NaN value in one or more of their columns.
4.   'object'/'categorical' variables' statistic show that some have thousands of unique entries which would rapidly increase our feature space when one-hot-encoded.


Steps to address the above observations

1.   Total_spend data binarized using zero as a threshold
2.   Majority randomly downsampled and minority class randomly upsampled only for the training data
3.   Since the number of NaN and NULL is rare in the positive(minority) class, I dropped all rows that have any NULL entry in the user and action table.

4.   For columns with object data type, unique elements were ranked in descending order based on their frequency and only the top k+1 (user input parameter) unique values are retained, with one unique value containing all unique entries with frequency less than the kth unique value. The (at most k+1) categories are then on-hot-encoded. 


Once encoded a tree-based classifier is trained with 'gini' as the deciding factor for a split. The performance of the model is observed via construction of an ROC-curve and PR-curve. A lift-up/ Gain Chart is also constructed to evaluate how effective the new pricing campaign would be, if based on the developed model.

It is worth noting that, while we are given the actions data from past users, we can't use data as is to train our model, since it will not be available for a new user. Therefore our prediction accuracy will not be so great, if we decide to advertize the new prices after we have data on the user our model (trained on the action) data would be able to better predict the likelihood that a user will spend money in the game. The most important features for determining whether a user will spend money or not based solely on user and device data are:


*   local day of the week - (Sat, Sun, Mon - showing high correlation with purchase)
*   local time of join - (9AM - 11PM - showing high correlation with purchase)
*   main memory size - < (6000 - showing high correlation with purchase)
*   height of device - (500 - 1000 correlating well with purchase)

Therefore, if there is no action available for the user, a special price could be advertized to users whose activity or device falls in the above category. Further improvements can be made via hyperparameter tuning - including number of top categories per feature, up/down sampling strategy as well as parameters for the trained model

![image](https://user-images.githubusercontent.com/6319868/148814252-e6584b69-b7ef-4dd8-bcb5-74c47ce28412.png)

![image](https://user-images.githubusercontent.com/6319868/148814285-14702017-0d96-41b0-991e-613492b5cc61.png)

![image](https://user-images.githubusercontent.com/6319868/148814343-f91ddd74-3f9d-4351-aa95-5293313f3ccf.png)

![image](https://user-images.githubusercontent.com/6319868/148814367-d0c3f58b-62a1-4d8e-acdd-b891cb97bfce.png)



