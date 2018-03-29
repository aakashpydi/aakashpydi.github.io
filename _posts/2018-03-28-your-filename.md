---
published: false
---
---
published: true
---

## Implementing K-Means and Agglomerative Clustering Algorithms from Scratch

I implemented the k-means and agglomerative clustering algorithms from scratch. [(Link to Github Repo of Source Code)](https://github.com/aakashpydi/K_Means_Agglomerative_Clustering)

The script uses the yelp dataset.[Yelp Dataset Link.](https://www.kaggle.com/yelp-dataset/yelp-dataset/data) I verified the correctness of the implementation using the SKLearn implementations of these algorithms. 

Here is some analysis I carried out to understand and compare the two algorithms. Four attributes are focused on in the analysis (latitude, longitude, reviewCount and checkins). 

### Understanding Which Dimensions are Driving the K-Means Clustering Model (K Value = 4)

![]({{site.baseurl}}/images/kmeans_att_1.png)
![]({{site.baseurl}}/images/kmeans_att_2.png)

From the plots above, its evident that the dimensions reviewCount and Checkins are driving the clustering model. We can make this inference from the clear, and well-defined pattern of the clusters in the reviewCount vs Checkins 2D plot.

The reason for this has to do with how we use the Euclidean distance for the similarity metric in K-means clustering. It should be self-evident that the dimensions with much higher values (and range) are going to affect the final distance more than the dimensions with much lower values (and range)

Observe that in the yelp data set, the reviewCount and Checkins values are MUCH higher than the latitude and longitude values. So the result is that those dimensions affect the similarity metric substantially more than the rest. Since we use the similarity metric to evaluate the clusters, it makes sense that the clustering model is driven more by the dimensions, checkins and reviewCount.

---

### Analyse K-Means Clustering Model after Carrying Log Transformation on Two Attributes (checkins and reviewCount)


---
