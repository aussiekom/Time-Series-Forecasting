### Anomaly Detection in Time Series Data 

It is useful to detect anomalies in time series data before modelling for forecasting. Many forecasting models are autoregressive, meaning that they take into account past values to make predictions. A past outlier will definitely affect the model, and it can be a good idea to remove that outlier to get more reasonable predictions.

**Types of anomaly detection tasks in time series**
There are two main types of anomaly detection tasks with time series data:

* **Point-wise anomaly detection**
* Pattern-wise anomaly detection

In the first type, we wish to find single points in time that are considered abnormal. *For example, a fraudulent transaction is a point-wise anomaly.*

The second type is interested in finding subsequences that are outliers. *An example of that could be a stock that is trading at an abnormal level for many hours or days.*


### In this code only *point-wise anomaly detection* was implemented, and three different methods were explored for outlier detection in time series data.

* First, a **robust Z-score** that uses the mean absolute deviation (MAD). This works well when the data is normally distributed and if the MAD is not 0.

* Second, **isolation forest**, which a machine learning algorithm that determines how many times a dataset has to be partitioned in order to isolate a single point. If few partitions are necessary, then the point is an outlier. If many partitions are required, then the point is likely an inlier.

* Finally, **local outlier factor (LOF) method**, which is an unsupervised learning method that compares the local density of a point to that of its neighbours. Basically, if the density of a point is small compared to its neighbours, it means it is an isolated point, and likely an outlier.
