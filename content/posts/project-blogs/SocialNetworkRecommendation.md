---
title: "Social Network Graph Link Prediction"
date: 2019-08-12T20:59:09+05:30
description: blog describing what the running case study is about
menu:
  sidebar:
    name: 03 Social Network Graph Link Prediction
    parent: pblogs
    identifier: socialnetworkgraph
    weight: 10
---
Details: https://www.kaggle.com/c/FacebookRecruiting/data

**Problem Statement:** Given a directed social graph, have to predict missing links to recommend users (Link Prediction in graph). 

This case study takes lot of references from graph theory. Purely graph based link prediction.  

* **Undirected graph:** connection from both edge. (eg. Fb friends)
* **Directed graphs:** connection from one direction. It can be from both directions. (eg. Twitter/IG following)
* A **path** is a sequence of edge that you follow to reach from edge e1 to edge e5. 
* **Length of a path** is the number of edges followed to reach the destination. 
* **indegree** is the number of connections coming in to a node
* **outdegree** is the number of connections coming out of a node
* A **subgraph** is a subset of a vertices and the edge between those vertices of a complete graph.
* **Connected components:** You can go from any vertex to any other vertex when directions are present. If ignoring the directions, can one vertex be reached to any other vertex. Even if one connection is present among the communities, the entire graph becomes one. There should not be any connection between the communities. Two users in the weakly connected components (within a community) may have a stronger probability of having a connection. 

**Mapping the problem into supervised learning problem**
Generated training samples of good and bad links from given directed graph and for each link got some features like no. of followers, is he followed back, page rank, katz score, adar index, some svd fetures of adj matrix, some weight features etc. and trained ml model based on these features to predict link.

**Business objectives and constraints:**
* No low-latency requirement.
* Probability of prediction is useful to recommend ighest probability links
* We got to suggest connections which are more likely to be correct and we should not miss out on any connection.

**Performance metric for supervised learning:**
* Both precision and recall is important so F1 score is good choice
* Confusion matrix

**Data Overview**
The data is one shot at a time t. It is highly dynamic in real world. Each person or a connection is referred to as node and the connection between them is called an edge. train.csv: 1.86M users (nodes), 9.43M records (edges). The data contains two columns: 
* **source**(int64) 
* **destination**(int64); 

each pair represents a edge in graph.


**EDA Performed:**
The source can be a influencer/ celebrity or a normal person, meaning he can have hundereds and thousands of followers(destination) or a single follower. The destination can also be a influencer or a normal person. 
* Calculated minium, average and maximum number of people a connection follows. 

**Findings:** 
* Most people have fewer number of followers
* User is following fewer connections
* The possible set of bad-eges (non existent but possible connections) is very large. So instead, we sample a few edges.

**Splitting the dataset into train and test set:**
Randomly splitting the data as there is no time based stamp available. 80-20 split based on edges and not on nodes. 

**Featurization techniques:** Added several features from the source and destination nodes. Some of them were distance
based while others were derived using general graph theory techniques.

* Jaccard Index:  (the number in both sets) / (the number in either set) * 100
J(X,Y) = |X∩Y| / |X∪Y|

* Cosine Distance
* [Page Rank](https://en.wikipedia.org/wiki/PageRank)
* Shortest Path: Getting shortest path between two nodes. If nodes have an edge i.e. trivially connected then removed that edge and calculating the shortest path

* [Adar Index](https://en.wikipedia.org/wiki/Adamic/Adar_index)
* [Kartz Centrality](https://en.wikipedia.org/wiki/Katz_centrality)
* SVD Features
* Number of followers
* Number of followees
* Belongs to same weakly connect components
* Shortest path between source and destination
* Weight Features:
    * weight of incoming edges
    * weight of outgoing edges
    * weight of incoming edges + weight of outgoing edges
    * weight of incoming edges * weight of outgoing edges
    * 2*weight of incoming edges + weight of outgoing edges
    * weight of incoming edges + 2*weight of outgoing edges
* Added couple of features such as preferential attachment and svd_dot for u and v.



**Modelling Techniques used:** Performed modelling on the dataset using **RandomForest** and **Xgboost** by hypertuning the parameters using **Randomized Search**. Preferential attachment turns out to be an important feature.

**References:** Some reference papers and videos 
* https://www.cs.cornell.edu/home/kleinber/link-pred.pdf
* https://www3.nd.edu/~dial/publications/lichtenwalter2010new.pdf
* https://kaggle2.blob.core.windows.net/forum-messageattachments/2594/supervised_link_prediction.pdf
* https://www.youtube.com/watch?v=2M77Hgy17cg
