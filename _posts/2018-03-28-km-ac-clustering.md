---
published: false
---
---
published: true
---

## Implementing K-Means and Agglomerative Clustering Algorithms from Scratch

I implemented the k-means and agglomerative clustering algorithms from scratch. [(Link to Github Repo of Source Code)](https://github.com/aakashpydi/K_Means_Agglomerative_Clustering)

The script uses the yelp dataset.[Yelp Dataset Link.](https://www.kaggle.com/yelp-dataset/yelp-dataset/data) I verified the correctness of the implementation using the SKLearn implementations of these algorithms. 

Here is some analysis I carried out to understand and compare the two algorithms. Four attributes are used to carry out the clustering in this analysis (latitude, longitude, reviewCount and checkins). 

### Comparing K-Means vs Agglomerative Clustering Along Two Dimensions (K Value = 3)

![]({{site.baseurl}}/images/kmeans_yelp_output.png)
![]({{site.baseurl}}/images/kmeans_agg_output.png)


### Understanding Which Dimensions are Driving the K-Means Clustering Model (K Value = 4)

![]({{site.baseurl}}/images/kmeans_att_1.png)
![]({{site.baseurl}}/images/kmeans_att_2.png)

From the plots above, its evident that the dimensions reviewCount and Checkins are driving the clustering model. We can make this inference from the clear, and well-defined pattern of the clusters in the reviewCount vs Checkins 2D plot.

The reason for this has to do with how we use the Euclidean distance for the similarity metric in K-means clustering. It should be self-evident that the dimensions with much higher values (and range) are going to affect the final distance more than the dimensions with much lower values (and range)

Observe that in the yelp data set, the reviewCount and Checkins values are MUCH higher than the latitude and longitude values. So the result is that those dimensions affect the similarity metric substantially more than the rest. Since we use the similarity metric to evaluate the clusters, it makes sense that the clustering model is driven more by the dimensions, checkins and reviewCount.

---

### Analyse K-Means Clustering Model after Carrying Out Log Transformation on Two Attributes (checkins and reviewCount)

![]({{site.baseurl}}/images/kmeans_log_1.png)
![]({{site.baseurl}}/images/kmeans_log_2.png)
![]({{site.baseurl}}/images/kmeans_log_3.png)

The first key change we observe that in these plots, the dimensions Latitude and Longitude are the one’s affecting the clustering model the most (as evidenced by the clear pattern of the clusters in the graphs). The reason for this is the same as what was discussed above. As we use the Euclidean distance for the similarity metric,
the dimensions with the LARGER values (and ranges), affect the clustering model more. 

We can see that the log transformation on the dimensions checkins and reviewcounts, dramatically reduced their size causing them to have a lower range than the Latitude and Longitude dimensions. What we also observe is the WC-SSE is less variable for the
values [K = 4,8,16,32,64]. We can perhaps attribute this to the ranges of the values taken by a subset of the dimensions, coming closer to one another

---

### Analyse K-Means Clustering Model after Transformation such that each Attribute has Mean=0 and Std-Dev=1

![]({{site.baseurl}}/images/kmeans_std_1.png)
![]({{site.baseurl}}/images/kmeans_std_2.png)
![]({{site.baseurl}}/images/kmeans_std_3.png)

In this case, we can see that ALL the dimensions seem to be driving the clustering model in roughly equal part. This makes sense as the dimensions have all been scaled. Observe that, the dimensions reviewCount and checkins seem to exhibit a clearer pattern in terms of the distinction between the cluster denoted by the red color and the rest of the clusters. This again makes sense, given that the range of values for checkins and reviewCount is larger than that of latitude and longitude, after scaling. Broadly speaking, observe that both the plots exhibit fairly clear patterns. We also observe that the WC-SSE consistently goes down with higher K values

---

