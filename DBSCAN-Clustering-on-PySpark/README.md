# Clustering-on-PySpark
Implement DBSCAN on PySpark      
- Density-based spatial clustering of applications with noise (DBSCAN) is a data clustering algorithm proposed by Martin Ester, Hans-Peter Kriegel, Jörg Sander and Xiaowei Xu in 1996.(WIKI) It is powerful clustering while dealing with dataset that has specific curves like circle and robust to outliers. We use it in many cases and it is popular in scikit-learn.      
- However,in most recent Spark version(2.2.0), the MLlib only supports clustering algorithms including K-means, Latent Dirichlet allocation(LDA), Bisecting k-means and Gaussian Mixture Model (GMM)these 4 methods.     
- In this notebook, I build DBSCAN model in PySpark so we can use it deal with big data.     
- More about DBSCAN : https://en.wikipedia.org/wiki/DBSCAN
 ## Challenges
- In the distributed environment, the sample points are distributed in different partitions, so it is difficult to traverse directly between different partitions. To solve this problem,we divide the partitions of different sample points into multiple batches and pull them to the driver, then <b>broadcast them to each excutor in turn</b>, calculate the distance respectively, and union the final result, so as to indirectly realize double traversal.

- In the distributed environment, provisional clustering clusters are distributed in different partitions, so it is impossible to directly scan the global core point list and merge clusters having the same core points.
To do this, <b>merge each provisional cluster within each partition, then reduce the number of partitions and repartition, and then merge each provisional cluster within each partition. Repeat this process continuously,</b> and finally divide all temporary clusters into one partition to complete the merging of all temporary clusters.    
reference: https://www.heywhale.com/home/column/5fe6aa955e24ed00302304e0
## Compare
<img src="https://user-images.githubusercontent.com/64514218/170573855-2d069d12-4f6b-4e08-988f-2dc38a3dc0d0.png" width="500" height="400" />
<img src="https://user-images.githubusercontent.com/64514218/170573871-9ffb42c8-2c5e-421e-a584-9fa559444323.png" width="500" height="400" />
