# Sunspots

This is a project that investigates _anomaly detection_/_outlier detection_. There is a nice introduction to anomaly detection in [Ref1](#Ref1).
1. **Point Anomalies**
Data points are considered independent, and we are looking for points that are unusual. In this case, anomalies can be considered as points that are well separated from large clusters. Clustering or density methods are useful in this case.
e.g.
  * Detecting a sensor that is miscalibrated / broken.
2. **Contextual anomalies**
A datum is anomalous in a specific context, but not otherwise
e.g.
  * daytime level power usage in the middle of the night
  * "the cat sat on the **sky**" (sky is not that unusual of a word, and is the expected type - noun - but it is unusual in this context).
  * Finding a transaction that is unusual for a specific individual.
3. **Collective anomalies**
A collection of data values is anomalous with respect to the entire data set, but not individual values. This case can be broken down further:
  - a) Events in unexpected order (e.g. breaking rhythm in an ECG)
  - b) Unexpected value combinations (e.g. buying a large number of expensive items)

The project described here uses sunspot data, taken from [Ref2](#Ref2). This is an example of (mostly) collective anomalies -- were do we see data that has a value that is unexpected given the point we are at in the cycle.

## Approaches

### 1. Time series
This was the original approach in [Ref2](#Ref2). A point is determined to be an outlier if the number of sunspots lies too far away from the moving average value. Specifically, the moving average is calculated, and then the residual (difference between moving average and predicted values) is taken. The standard deviation of the residuals gives a rough idea how good a job the moving average does. A point that is more than 3 standard deviations from the moving average is labelled an outlier / anomaly.

  There are two slightly different approaches to determine the standard deviation. One is to use all the residuals in the data set. The other is to use the "moving" standard deviation as well.

  This code mostly duplicates what exists in [Ref2](#Ref2), but tidies it up a little bit, and eliminates some now depreciated functions.

### 2. RNN
In this approach, a RNN is trained to predict values using the first 2000 months as training data. The predictions of the forecasting model, where you give `n_steps` input points to the sequence and use it to predict the next point works reasonably well. The generative approach, where you give the first `n` months and then just keep generating predictions suffers from the vanishing gradient problem.

### 3. LTSM

  [ ] To be implemented

# References

#### Ref1 https://iwringer.wordpress.com/2015/11/17/anomaly-detection-concepts-and-techniques/
#### Ref2 https://www.datascience.com/blog/python-anomaly-detection
#### Ref3 https://blog.statsbot.co/time-series-prediction-using-recurrent-neural-networks-lstms-807fa6ca7f
