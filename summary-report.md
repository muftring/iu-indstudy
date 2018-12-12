# <p style="text-align: center;">Community Evolution in Temporal Networks</p>
<br>
<p style="text-align: center;">
<i>
Michael Uftring<br>
Indiana University<br>
DSCI-D699-30784 - Independent Study<br>
Fall, 2018
</i>
</p>
<br>

<p style="padding-left:40px;"><b>Abstract.</b>
An in depth analysis of community structure changes in temporal networks can be useful for gaining insight into why these changes occur and what impact the changes might have. For example, observing the ebb and flow of community membership over time and the effects of a diffusion process could potentially provide an understanding of the long-term dynamics of the spreading phenomena on an evolving network.
<br><br>
Computing modularity for each time-slice permits the ability to compare the community structures of adjacent temporal periods of a dynamic network. The magnitude of each community (in terms of relative size to the network as a whole at that time) in each time-slice can be visualized; this shows the flow of community membership over time. Similarly, clustering similarity is computed from adjacent pairs of modularity and aggregated over the entire time-span of the network data set. This produces a measure of frustration for each vertex. Frustration can be used to identify stable and unstabble nodes in the network. The node stability measure, with respect to community formation and membership, can indicate which nodes are foundational group members and which are potentially "floaters" (perhaps affecting cross-community diffusion).
<br><br>
The Temporal Network Analysis (TeNA) Python package is being developed to provide analytical functions and visualization methods for these types of analysis techniques. TeNA is built upon proven Python packages: NetworkX, Numpy, Pandas, CluSim, and uses an implementation of the Louvain community detection algorithm.
</p>

# Introduction
Most real networks are not static. Over time, network structures change: nodes and links are added and removed. Given this kind of flux in the compositional elements, other network structures are subject to change as as a result.

Community structures are generally formed in networks based on the links connecting nodes. Communities reflect a greater-than casual relationship amongst the grouped nodes. The structural changes that occur in networks result in changes to the community structures. As nodes and links are added and removed, the communites in a network will form, expand, contract, and dissolve. Understanding the dynamics of community structure can provide insight into the behavioral patterns and processes on the network. The objective of this project is to provide tools to aid in garnering insights from the evolution of community structure in temporal networks.

When we consider processes on networks as they execute across a span of time, we want to gain some insight into how the processes are affected by the network structure or how the process will effect change on the network structure. For example, the nature of diffusion is well understood for static networks. Observing and understanding the dynamic ebb and flow of community structures over time may provide some insight into how spreading is started, increased, decreased, or prevented in temporal networks. Ultimately we would like to use this understanding and gained insight to become able to make predictions on processes, but this is outside the scoop of the current project. 

The key components of the approach presented here are:

- *Aligned Cluster Numbering*: Most clustering algorithm implementations assign nodes to numbered groups. Each numbered group represents a network community. While the clustering algorithms will not vary the membership in the groups across executions (except as possibly perturned by their stochastic nature), the number assigned to each groups may vary. This was observed to be especially true when executing the clustering on a sequence of temporal slices of networks. In order to consistently analyze a sequence of communities across a span of time, we introduce a mechanism to align the cluster numbering across the entire sequence.
- *The Ebb and Flow of Community Membership*: A visualization technique which shows the temporal nature of a network from the perspective of communities. The communities are presented as nodes, and the links between nodes show the fluctuation in community membership.
- *Identifying Stable Nodes*: Using an element-based clustering similarity measure we can compute an overall score for each node in the network across the entire temporal span. The score will indicate stability of the node, relative to its clustering (community assignment) over time. This score is also called level of frustration.

# Discussion
## Aligned Cluster Numbering
We need to be able to trace the lifecycle of community structures in temporal networks. Beyond the initial community formation (from the inaugural time-slice of the temporal network), we need to be able to observe and correlate the changes as time progresses. In order to follow each community, we need a means to consistently identify the communities. We achieve this by aligning the cluster number assignemnts across each time-slice of the temporal network.

Aligning cluster numbering is a process of tracing ancestry. The process entails a comparison of two temporally adjacent cluster assignments, and execution of set of steps:
1. **compute grouping**: Collects group membership at a specific time. Returns a collection of the groups with their members.
2. **determine similarity**: Computes the similarity between two groups. Similarity is based on *set operations*: how much of group1 is in group2 is determined by using the set intersection operation.
3. **find contributors**: For each group, determine which of the previous groups contribute membership. 
4. **determine origins**: Based on the contributors (found in the prior step), the previous group that provides the largest contribution to the current group is deemed to be the origin. In some cases there is a sole contributor. In some cases one previous group is the sole contributor to more than one current group (the previous group has split). Care is taken such that each previous group is the origin for just one current group, and all group numbering is unique (thus mirroring the results of the actual cluster assignment).
5. **rename communities**: Based on the origins, if a current group's origin is different than its current assignment, the group is renamed.

A simple example is presented in the following figures and tables. The network consists of ten nodes observed at three points in time. At time 1, the network has two equal sized communities. At time 2, one node from each of the two original communities (nodes 4 and 9) form a third community. And at time 3, one node from each of the two original communities (nodes 3 and 6) join the third community. The nodes are colored according to their cluster assignment (0: red, 1: blue, 2: green), and the coloring is consistent across each time-slice, and in both the unaligned and aligned results.

Without aligning the cluster numbering, we observe the results presented in Figure 1 and Table 1. At time 2 we observe that the newly formed cluster (nodes 4 and 9) is assigned number 1, and the original cluster assigned 1 (nodes 5, 6, 7, and 8) is now assigned 2. Intuition tells us that cluster 1 should survive at time 2 with the four remaining nodes (5, 6, 7, and 8) and a new cluster assigned number 2 should be formed with the moving nodes (4 and 9). Figure 2 and Table 2 present these results when aligning the cluster numbering, and now the cluster numbering is more intuitive.

![](simple-unaligned-t1a.png)
![](simple-unaligned-t2a.png)
![](simple-unaligned-t3a.png)   
**Figure 1**: Simple Network - 3 Time-slices, Nodes Colored with Community Assignment (Unaligned)

| Node | Time 1 | Time 2 | Time 3 |
| ---- | ------ | ------ | ------ |
| 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 0 |
| 2 | 0 | 0 | 0 |
| 3 | 0 | 0 | 1 |
| 4 | 0 | 1 | 1 |
| 5 | 1 | 2 | 2 |
| 6 | 1 | 2 | 1 |
| 7 | 1 | 2 | 2 |
| 8 | 1 | 2 | 2 |
| 9 | 1 | 1 | 1 |
**Table 1**: Simple Network, Unaligned Cluster Numbering

![](simple-aligned-t1a.png)
![](simple-aligned-t2a.png)
![](simple-aligned-t3a.png)   
**Figure 2**: Simple Network - 3 Time-slices, Nodes Colored with Community Assignment (Aligned)

| Node | Time 1 | Time 2 | Time 3 |
| ---- | ------ | ------ | ------ |
| 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 0 |
| 2 | 0 | 0 | 0 |
| 3 | 0 | 0 | 2 |
| 4 | 0 | 2 | 2 |
| 5 | 1 | 1 | 1 |
| 6 | 1 | 1 | 2 |
| 7 | 1 | 1 | 1 |
| 8 | 1 | 1 | 1 |
| 9 | 1 | 2 | 2 |
**Table 2**: Simple Network, Aligned Cluster Numbering

## The Ebb and Flow of Community Membership
A helpful tool is a visualization of the ebb and flow of community membership in a temporal network. The visualization shows how the communities form, expand, retract, and dissolve. We can clearly see the contributions and origins of each community at each time-slice, as described earlier in *Aligned Cluster Numbering*. To visualize the flow of community membership we use Alluvial Flow Diagrams (also known as Sankey Diagrams). 

Flow is computed by looking at temporally adjacent, aligned cluster numbering (community assignments). We determine all unique "from" and "to" combinations present in the adjoining community assignments. Then we accumulate all traffic in the appropriate from-to bucket; this produces the magnitude of each flow. The flow itself is a directed acyclic graph, where each node represents a community and each edge represents the traffic of membership.

The flow visualization can be presented using three different mechanisms:
- NetworkX and Matplotlib, with a directed acyclic graph layout computed using the GraphViz `dot` program
- the `ipysankey` Jupyter Notebook widget
- as an SVG in an HTML file using **Semiotic**

The ebb and flow of community membership for the simple network presented earlier is shown in Figure 3. Each time-slice is vertically aligned in the visualization. Here we can see how group 0 (red) and group 1 (blue) contribute to group 2 (green) across two time periods. The edges in the graph show the magnitude, the number of community members, travelling from node to node.

![](simple-flow.png)   
**Figure 3**: Simple Network, Flow of Community Membership

## Clustering Similarity
Clustering similarity measures are used to indicate how similar two clustering results are. There are several different clustering similarity measures. Here we use the element-based clustering similarity measure provided by the *CluSim* Python package. This measure is useful for at least two reasons. First, it is robust to <something>. Second, it can give us a similarity measure for each node. With the node-granular measure computed across each temporal span, we can aggregate the values to determine overall node frustration.

Frustration is a measure of how similarly a node has been clustered between two clusterings. The aggregated frustration over the time-span of a temporal network can be an indicator of how stable a node is over time. A stable node would have a high aggregated node-granular clustering similarity value, meaning is has been clustered consistently over time. Lower values indicate that a node has not been clustered consistently over time.

Knowing node-granular clustering similarity value, or frustration, can yield some insight into the network community structures. Stable nodes may be foundational nodes for communities: these may be the nodes "with attraction" and around which the communities form. Unstable nodes, also called "floaters," tend to move from community to community. This behavior may explain the effectiveness or ineffectiveness of observed network processes like diffusion across communities.

# Primary School Data
- provide reference
- briefly explain the data set
- do we know anything from the original paper?
- explain analysis
- show Alluvial Flow Diagram
- present stable and unstable nodes (frustration)
- discuss computation of this at different time granularities
- what, if anything, do we learn from this?

# Conclusion
Aligned cluster numbering is essential for visualizing the ebb and flow of community membership.

With element-based cluster similarity measures we can gain some understanding into how and why communities in networks form, expand, retract, and dissolve, and potentially gain some insight into processes executing on the network.

# Future Work
- this is an evolving tool set, and Python package
- ideally we will continue to improve and expand this beyond the scope of this project

To Do List:
- data set framework (abstraction) 
  - to load diverse temporal network data sets into common data format
  - for use with TeNA
- API to permit temporal window size (where applicable)
- annotate Alluvial Flow diagrams with least and most frustrated nodes 
  - to show community important nodes, and free floaters
- real-time capabbilities:
  - stream network data in
  - live Alluvial Flow Diagram construction and update
- Alluvial Flow Diagram navigation:
  - scroll "wide" diagram
  - zoom in / zoom out 
- publish TeNA package
- docs for TeNA package
- spreading phenomena (dynamics)
- performance: make TeNA work well with small and large data sets
- further analysis projects (UIUC research)

# References

1. Alex and YY's paper
2. the Primary School paper (though it may not be directly relevant)
3. Fundamental structures of dynamic social networks (the PNAS paper)
4. ???
5. https://cs.stanford.edu/people/widom/paper-writing.html
