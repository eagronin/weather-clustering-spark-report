# Results
This section visualizes and discusses the results of the k-means clustering analysis identifying different weather patterns for a weather station in San Diego, CA. The elbowplot discussed in the previous section is shown first.  Then we plot and discuss several charts showing cluster centers for the optimal number of clusters to identify distinct weather patterns.

The analysis is discussed in the [previous section](https://eagronin.github.io/weather-clustering-spark-analyze/).

Let's first plot the elbowplot by calling elbow_plot() function described in the [previous section](https://eagronin.github.io/weather-clustering-spark-analyze/).

![](elbow plot)

The values for the number of clusters (k) are plotted against WSSE values.  The chart shows that WSSE declines as the number of clusters increases.  This is intuitive, because allowing for a larger number of clusters facilitates shorter distances between each point in the feature space and cluster centers.  However, as the number of clusters increases, the curve gradually flattnes, as the contribution to the decline in WSSE by adding a cluster diminishes. The bend in the curve (or elbow) provides an estimate for the optimal value for k.  In this plot, we see that the elbow in the curve is between 10 and 15, so let's choose k = 12. The subsequent plots of cluster centers are all based on the number of clusters k = 12.

First, we determine weather patterns on "dry days" (characterized by low relative humidity):

```python
parallel_plot(P[P['relative_humidity'] < -0.5], P)
```

![](chart 1)

While all the clusters shown on the chart above have low relative humidity, the other features, such as air tempreature and wind direction are different across these clusters.  Therefore, the clusters represent distinct weather patterns, all of which are characterized by low relative humidity.

For example, cluster 4 has low wind direction (whcih means that the samples in this cluster have wind coming from the N and NE directions) and high wind speed.  On the opposite end of the spectrum is cluster 11, characterized by high wind direction and low wind speed.

Second, we determine weather patterns on "warm days" (characterized by high air temperature): 

```python
parallel_plot(P[P['air_temp'] > 0.5], P)
```

![](chart 2)

While all clusters in this plot are characterized by realtively high air temperature, they differ in values for other features, thereby representing distinct patterns of warm weather.  

Third, we deterimne weather patterns on "cool days", such as weather samples with high relative humidity and low air temperature:

```python
parallel_plot(P[(P['relative_humidity'] > 0.5) & (P['air_temp'] < 0.5)], P)
```

![](chart 3)

For example, cluster 5 represets samples with high wind speed which, in the presence of cool temperature and high relative humidity suggest stormy weather patterns.

The only cluster that has not been included on the three previous charts is cluster 2:

```python
parallel_plot(P.iloc[[2]], P)
```

![](chart 4)

We can see that air temperature, relative humidity and wind speed for this cluster all fall within one standard deviation from the mean, suggesting a mild weather pattern.

Previous step: [Analysis](https://eagronin.github.io/weather-clustering-spark-analyze/)
