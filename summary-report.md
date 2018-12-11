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
An in depth analysis of community structure changes over time in temporal networks can be useful for gaining insight into why these changes occur, and what impact the changes might have. For example, observing the ebb and flow of community membership over time and the effects of a diffusion process can potentially provide further understanding of the long-term dynamics of the spreading phenomena on an evolving network.
<br><br>
Computing modularity for each time-slice of the network permits the ability to compare the community structures of adjacent temporal slices of the dynamic network. The magnitude of each community (in terms of relative size to the network as a whole at that time) in each time-slice is visualized which shows the flow of membership across time. Clustering Similarity is computed from adjacent pairs of modularity and aggregated over the entire time-span of the network data set. This produces a measure of frustration for each vertex which is used to identify stable and unstabble nodes in the network. The stability measure of nodes, with respect to community formation and membership, can indicate which are foundational group members and which are potentially "floaters" (perhaps influencing cross-community diffusion).
<br><br>
The Temporal Network Analysis (TeNA) Python package is being developed to provide analytical functions and visualization methods for these analysis techniques. TeNA is built upon proven Python packages: NetworkX, Numpy, Pandas, CluSim, and uses an implementation of the Louvain community detection algorithm.
</p>

# Introduction

5 paragraphs (more or less):

> 1. What is the problem?
> 2. Why is it interesting and important?
> 3. Why is it hard? (E.g., why do naive approaches fail?)
> 4. Why hasn't it been solved before? (Or, what's wrong with previous proposed solutions? How does mine differ?)
> 5. What are the key components of my approach and results? Also include any specific limitations.

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

**Why is it hard?**   
(maybe skip this)

**Why hasn't it been solved before?**   
Or, what's wrong with previous proposed solutions? How does mine differ?   
(maybe skip this)

**What are the key components of my approach and results?**   
Also include any specific limitations.

- aligning community assignments
- clustering similarity across temporal community assignments
- identify stable nodes which may be important in community formation and preservation across a span of time
- how do we measure similarity between clusterings?
- how do we observe clusterings across a span of time?
- what visualization techniques can we utilize?
- what can or should we look for from clustering over time?
- how do we identify "stable" nodes across clusterings?

# Discussion
## Network Flow

what is this? 
- in the context of looking at Community Structure across time, it shows how the communities form, expand, retract, and dissolve
- we can use Alluvial Flow Diagrams to show the flow of community membership across time

why is it interesting?

how do we like to visualize this?

## Consistent Cluster Assignment

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
