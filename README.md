# Facebook Social Circle Analysis with NetworkX

In this repo you can find a network analysis of Facebook social circles using NetworkX which is an open-source package. For tutorial you can check [this](https://github.com/CambridgeUniversityPress/FirstCourseNetworkScience/tree/master/tutorials). Firstly, a function is created to explore and summarize any network. It summarizes any induced subgraph of the input network.

## 1. Network Summaries

**Input:**

* `edgelist`: an edge list for the network
* `vertices`: labels of vertices in the network (must correspond with the entries in edgelist)
* `subgraph`: a list of vertex labels from vertices you would like to visualize. Set the default to be vertices.
* `directed`: a logical indicating whether or not the network is directed

**Output:**

1. Visualizations
    * The induced subgraph that has vertex set subgraph and edge set equal to all the edge weights between the vertices in subgraph.
    * The degree distribution of subgraph. In the case that directed = TRUE, provide plots of both the in- and out- degree distributions.
    * The betweenness centrality distribution of the subgraph vertices (where betweenness is calculated on the induced subgraph not on the entire graph)
    * The eigenvector centrality distribution of the subgraph vertices (where eigenvector cen- tralities are calculated on the induced subgraph not on the entire graph)
2. Local Summaries: For each vertex in subgraph, store its eigenvector centrality, betweenness centrality, the degree, and closeness centrality
3. Global Summaries: Store the diameter, clustering coefficient, number of nodes, number of edges for the network, number of connected components, size of the largest connected component of the induced subgraph from the subgraph vertices.

## 2. Social Circle Analysis

People often categorize their friends in terms of social circles. At Facebook, it is of interest to discover an individual’s social circles in a completely data- driven fashion. The [paper](http://i.stanford.edu/~julian/pdfs/nips2012.pdf) considered this problem and used individuals’ ego-network – the network of their friend’s friendships on Facebook. 

For this part, we will look at the social circles for an individual considered in a large Facebook dataset in the paper mentioned above. Our goal is to investigate the similarities and differences between the social circles of an individual in the study. First, the data facebook.tar.gz at https://snap.stanford.edu/data/ego-Facebook.html is downloaded.
Then below graphs are visualized:

1. The ego-networks visualization for vertex 107 using the function created. Note, the file 107.edges is the edge list for node u.

2. Degrees of separation: “6 degrees of separation” experiment at Facebook, which in 2016 showed that on average two people are 3.57 friends apart. Let’s repeat this experiment on the Facebook data we have from above. Full data set of facebook combined.txt.gz used and run the following experiment:
    1. choose 2 nodes at random in the network
    2. calculate the shortest path between them and store this value
    3. repeat (a) and (b) 1000 times

The distribution of the 1000 shortest path lengths plotted.

## 3. Community Detection

You can find a tutorial for community detection [here]( https://github.com/CambridgeUniversityPress/FirstCourseNetworkScience/tree/master/)
For visualization `spring_layout` will be used. New inputs are added to above function.

**Output:**

* When `directed = TRUE`, return the reciprocity for every vertex in subgraph as well as the average reciprocity of the vertices in subgraph.
* The vertex labels with the highest eigenvector, betweenness, closeness, and degree (in- and out- if `directed = TRUE`) centralities.
* Return the local clustering coefficient for each vertex in subgraph.
* When `directed = FALSE`, return the cluster labels for all vertices that result from the
application of the Girvan-Newman modularity maximization algorithm.
* When `directed = FALSE`, return the modularity of the network partition identified by the Girvan-Newman modularity maximization algorithm.
* The number of isolates in the subgraph.

**Visualization:**

* If `directed = TRUE`, add a histogram for the local reciprocity distribution to your suite of visualizations.
* Replace the simple hairball plotting of the network you did previously with a plot of the subgraph that is drawn using the Fruchterman-Reingold force-directed algorithm. Further- more, color the nodes according to the community labels that were obtained from the Girvan- Newman algorithm mentioned in the output above.

---

The above function ran on the ego networks for nodes 0 and 107, making sure to get communities from the Girvan-Newman algorithm. Now, let’s assess how closely the identified communities from community detection match the social circles defined for the graph.

Below visualizations applied in the notebook:

* Plotted the network via the force-directed layout twice. In one plot, color the nodes according to the identified communities. In the other plot, color the nodes according to the social circle labels identified in the .circles files. Note - there may be overlap among the circles. 

* For the 5 largest communities, plotted a bar chart which shows the number of individuals from each social circle in that community. (E.g., if community 1 has 15 nodes where 10 nodes come from social circle 0 and 5 nodes from social circle 1, plot a bar chart showing 10 and 5).
* Calculated the adjusted rand score to gauge the similarity between the cluster labels and the social circle labels for each vertex.

---

The node2vec algorithm is a popular algorithm that identifies vertex features (embeddings) for each vertex in an observed graph. These embeddings act as low-dimensional summaries of the vertices and can be used for machine learning tasks like clustering, classification, and prediction. Links to the source code, data, and publication are available here: https://snap.stanford.edu/ node2vec/.

* Incorporated the node2vec algorithm (with default parameter settings) in the network analysis function. In particular, return D-dimensional embeddings for each vertex in the graph where D is chosen by the user as input. Verified that the new function works on the simple 20 node graph with D = 10.
* Ran node2vec on the ego networks for vertex 0 and 107 from the Facebook data with D = 30. With the embeddings, following machine learning tasks applied for the subgraphs of vertex 0 and 107:
    * Clustered the nodes using the node × embeddings matrix
    * A simple prediction model is built where the input features are the node embeddings and the response is the social circle label of the node.