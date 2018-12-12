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




When we consider processes on networks as they execute across a span of time, we want to gain some insight into how the processes are affected by the network structure or how the process effects change on the network structure. For example, the nature of diffusion is well understood for static networks. Observing and understanding the dynamic ebb and flow of community structures over time may provide some insight into how spreading is started, increased, decreased, or prevented in temporal networks. Ultimately we would like to use this understanding and gained insight to become able to make predictions on processes, but this is outside the scoop of the current project. 

The key components of the approach presented here are:

- *Aligned Cluster Numbering*: Most clustering algorithm implementations assign nodes to numbered groups. Each numbered group represents a network community. While the clustering algorithms will not vary the membership in the groups across executions (except as possibly perturned by their stochastic nature), the number assigned to each groups may vary. This was observed to be especially true when executing the clustering on a sequence of temporal slices of networks. In order to consistently analyze a sequence of communities across a span of time, we introduce a mechanism to align the cluster numbering across the entire sequence.
- *Identifying Stable Nodes*: Using an element-based clustering similarity measure we can compute an overall score for each node in the network across the entire temporal span. The score will indicate stability of the node, relative to its clustering (community assignment) over time. This score is also called level of frustration.



# Discussion

**What is the problem?**
- we would like to gain some insight into how and why networks and their communities change over time
- how can we observe the flow of community structure over time?
- what might this tell us?
- how can we gain some understanding about the impact of the flow?

Supporting notes:
- what is a network?
- networks are dynamic, they change over time
- what is a community?
- communities are groups of related entities
  - in networks, the entities are generally vertices (nodes) that are interconnected by edges (links)
  - the relationship shared amongst the group of entities is specific to the network
  - examples:
    - social network: a group of friends
    - work / research: collaboration network
    - publication: citation network
- communities change over time


**Why is it interesting and important?**
- knowing this may reveal insights into the structure or processes working on the network, such as spreading phenomena (which could be information or disease)
- is there a means to make predictions about the network structure and its processes (future work)?

**Approach:**
- aligning community assignments
- clustering similarity across temporal community assignments
- identify stable nodes which may be important in community formation and preservation across a span of time
- how do we measure similarity between clusterings?
- how do we observe clusterings across a span of time?
- what visualization techniques can we utilize?
- what can or should we look for from clustering over time?
- how do we identify "stable" nodes across clusterings?

## The Ebb and Flow of Community Membership

what is this? 
- in the context of looking at Community Structure across time, it shows how the communities form, expand, retract, and dissolve
- we can use Alluvial Flow Diagrams to show the flow of community membership across time

why is it interesting?

how do we like to visualize this?

## Consistent Cluster Numbering

**Desire:**
- have consistent modularity assignments across the temporal slices
- why? without this, using color in the Alluvial Flow Diagram does not present well

**Problem:**
- the community (number) assigned by the Louvain implementation [reference] is not necessarily consistent across executions
  - i.e., nodes 1 and 2 may be clustered together in time-slice 1 and 2, but the Louvain implementation may call it cluster 0 the first time and cluster 1 the second time
  - show simple example (from notebook)
- this makes it difficult to show consistent cluster membership across time

**Solution:**
- align the modularity assignments across the temporal slices
- process temporally adjacent modularities to make the cluster assignments consistent across time
- find contributors
- rename modularity assignments

Network flow across time can be represented as a directed acyclic graph (DAG). The DAG can be presented as columns of temporally related nodes. 

**Example:**
- small network example in *Exploring Network Changes* Jupyter Notebook
- a few nodes, changing links and community assignments over time
- show issues
- show resolution

## Clustering Similarity

what is this? 
- a mechanism to comapre the similarity of two clustering results

why is it interesting or important?
- to find stable nodes: what are stable nodes?
- to find unstable nodes: what are unstable nodes?
- knowing these may help in understanding how communities in networks form, expand, retract, and dissolve
- and it may provide some insight into spreading phenomena within a community, and between communities

# Primary School Data
- provide reference
- explain data set
- do we know anything from the original paper?
- explain analysis
- show Alluvial Flow Diagrams
- present stable and unstable nodes (frustration)
- discuss computation of this at different time granularities
- what, if anything, do we learn from this?

# Related Work
(maybe skip this section, and instead cite relevant or supporting work inline and assemble list of references at the end)

papers:
1. Alex and YY's paper
2. the Primary School paper (though it may not be directly relevant)
3. Fundamental structures of dynamic social networks (the PNAS paper)
4. ???

# Conclusion

What did we find? What can we conclude?

# Future Work

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

-----

https://cs.stanford.edu/people/widom/paper-writing.html
