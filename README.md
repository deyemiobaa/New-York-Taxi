# New-York-Taxi-Spend-Analysis
##### A supervised regression model to predict taxi fares based on hours of the day and location

### Why I chose this problem: 
It's very possible for taxi drivers NewYork to miss out on opportunities because they operate in areas that are too competitive because of the high number of other taxi drivers operating there while ignoring potential income generating areas in their city

### How I intend to use the solution:
Once we have a close estimate of how much taxi drivers makes per hour in a region/county, the head of the taxi operations can allocate drivers to serve the city better based on the predictions. Also, drivers would spend much less time trying to figure out where to position themselves to get passengers

##### A rundown of the what I did in this projects

### Data Importing, Cleaning and Exploration:
![Data_Source (January-2019):](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

Here is the list of features that can be used for model development and came with the original data: (‘PULocationID’, ‘transaction_date’,’ transaction_month’,’ transaction_day’, ‘transaction_hour’, ‘trip_distance’,’ total_amount’, ‘count_of_transactions’)
You can refer to [this document](https://www1.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf) for the meaning of each feature.
There were values in the "total_amount" that was as high as 600,000 and values below zero. 
I removed these datapoints to make our data much better for the model

![Total_amount_before_cleaning](/images/total_amt_bef_cln.png)

Cleaned Negative & Extremely High Values

The total_amount looks like this after cleaning the bad data
![Total_amount_after_cleaning](/images/total_amt_aft_cln.png)

### Feature Engineering
I explored and created new features in the data including holidays, weekends, and Borough information. The first two features were created from the existing data and the Borough information was gotten from the same page as the original data. I contains more information that represents the PickUp Location IDs

### The algorithms I tried and the results
I used Decision Trees, Random Forest and Gradient Boosting. The benchmark model is a Decision Tree. In the benchmark model, only the original features of the model as stated above are included. And on the normal models I used all original features plus the newly created ones.

Here are the performance results before tuning:

| Algorithm         |  MAE  |  RMSE  |   R2  |
|-------------------|:-----:|:------:|:-----:|
| Benchmark model   | 9.758 | 14.703 | 0.215 |
| Decision tree     | 8.211 | 13.334 | 0.354 |
| Random forest     | 7.544 | 13.465 | 0.342 |
| Gradient boosting | 8.314 | 13.199 | 0.367 |


I selected the gradient boosting model for tuning
Reason: It's a model that gives good accuracy and also it's r2 score and RSME is the best. 

Here are the best parameter values:
n_estimators = 1000
max_features = 'log2'
min_samples_split = 5
min_samples_leaf = 20
learning_rate = 0.05
max_depth = 10

n_estimators = 200
max_features = 'auto'
min_samples_split = 5
min_samples_leaf = 4
learning_rate = 0.1
max_depth = 4

I went with the first parameter values because it gives me better metrics and runs under the shortest time

The performance compared to previous models is:

| Algorithm         |  MAE  |  RMSE  |   R2  |
|-------------------|:-----:|:------:|:-----:|
| Benchmark model   | 9.758 | 14.703 | 0.215 |
| Decision tree     | 8.211 | 13.334 | 0.354 |
| Random forest     | 7.544 | 13.465 | 0.342 |
| Gradient boosting | 8.314 | 13.199 | 0.367 |
| Tuned Gradient Boost | 7.160 | 12.413 | 0.440 |


Here is the True vs. Predicted value plot for the tuned Gradient Boosting model. X-axis is the true values and y-axis is the predicted values.

![Performance graph of tuned Random Forest](/images/Gradient_boost_final.png)


### Next steps
From the plot above, you can see that the performance can be improved. A number of things I can do is:
* limiting the regions included in this analysis. Removing the regions that do not normally get a lot of taxi traffic. This might be a good action to take depending on the problem at hand. (If the goal is to service all of NYC no matter what, we should keep those data points)
* hand selecting borough that should be included in the model. Again this should be decided based on the problem at hand and how this model is going to be used. But if the goal is solely to increase model performance, only including boroughs with the most transactions can increase the performance since likely most mistakes come from boroughs with fewer data points. Though this assumption should be checked before taking this action.

🙂
