# NewYork City Taxi Demand Prediction
## **Objective**:

Given a region and a particular time interval, predict the no of pickups as accurately as possible in that region and nearby regions.Based on the data, machine learning model predicts the pickup demand of cabs in 10 minutes time frame. In this python notebook different machine learning model have been trained and accuracy is tested.
### **Constraints:**
* #### Latency :
Given a location and current time of a taxi driver, as a taxi driver, he/she excepts to get the predicted pickups in his/her region and the adjoining regions in few seconds. Hence, there is a medium latency requirement.
* #### Interpretability:
As long as taxi driver gets good prediction result, he/she is not be much interested in the interpretability of the result. He/she is not much interested in why he/she is getting this result. Hence, there is a no interpretability required.
* #### Relative Errors:
Mean Absolute Percentage Error will be the relative error we will consider. Let say the predicted pickups for a particular location are 100, but actual pickups are 102, the percentage error will be 2% and Absolute error is 2.
## **Data Cleaning**:
* #### Pickup/Drop-off Latitude and Longitude : 
It is inferred from the source https://www.flickr.com/places/info/2459115 that New York is bounded by the location coordinates(lat,long) - (40.5774, -74.15) & (40.9176,-73.7004). So, any coordinates not within these coordinates are not considered by us as we are only concerned with pickups which originate within New York.
* #### Trip Durations:
According to NYC Taxi & Limousine Commision Regulations the maximum allowed trip duration in a 24 hour interval is 12 hours. So we remove the points where the duration is >12 hr
* #### Speed:
We found that the 99.9th percentile value of speed is 45.31 mph. So, we consider only the data points where the computed speed is <45.31mph. We also observed that the avg speed in New York is 12.45miles/hr, i.e. a cab driver can travel 2 miles per 10min on avg
* #### Trip Distance:
The 99.9th percentile value of the distance covered in a ride is ~23 miles. So we remove rows with large trip distances
* #### Fare:
From percentile and graphical analysis, we set the upper limit of total fare to be 1000$ and consider only the data points which satisfies the limit
## **Data preparation**:

* #### Clustering/Segmentaion: 
Now we break whole of NYC into clusters/segments/regions. We choose the (lat,long) of pickup as features and we apply K-Means clustering algorithm(we find the right K using GridSearch). On choosing a cluster size of 40, we find an optimal min inter-cluster distance. Finally, we assign the cluster no. to each data point

* #### Time-binning:
We use the unix time-stamps to find the 10 min time-bins each data point belongs to(index of the 10min time interval to which that data point belongs)

* #### Smoothing time-series data: 
In our time-series data plot, there will be some 10-min bins where there are no pickups. And its not useful to predict zero pickups for a data point and moreover these points will create a problem in Moving Averages(using ratios). Thus, we smooth our training data(2015)(as in smoothing we are looking at future values) and fill with zero our test data(2016)

* #### Fourier Transform: 
From the Fourier transform plot of our time-series data we find the top amplitudes and corresponding frequencies
## **Modeling** : **Baseline Models**
Here we will use below baseline model by generating feature with ratio and previous value at a time (t-1) and will calculate Mean Absolute Percentage Error

   * ####  Moving Averages
   * ####  Weighted Moving Averages
   * ####  Exponential Moving Averages

Along with that, we will use below regression model by selecting best hyper-parameter with the help of different technique depending on hype parameter to predict the taxi demand.

   * #### Linear Regression with GridSearch
   * #### Random Forest Regressor with Random search
   * #### XgBoost Regressor with Random search

## **Conclusion**:
XGBoost Regression has performed so well that have less than 12% Error Metric (Mean Absolute Percentage Error) for train and test data.
