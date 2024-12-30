# TeraHAC-algorithm
This project is part of the TeraHAC algorithm.
The function GetEpsilon retrieves the ϵ parameter from
the configuration, defaulting to 0.1 if not specified. This parameter
controls the approximation level, balancing speed
and accuracy.
In the Cluster and HierarchicalCluster methods,
other essential parameters, such as the linkage threshold
(t), are extracted. The linkage threshold determines
the minimum similarity for merging clusters. These parameters
are passed into the core implementation function,
TeraHacImplementation. This function is the main
driver for the TeraHAC algorithm. It processes the graph iteratively
until no further merges are possible based on the
pruning threshold. Key steps include:
• The graph is represented as a ClusteredGraph, containing
metadata such as nodes, edges, and cluster assignments.
• The node to partition array maps each node to its
corresponding cluster.
• The pruning threshold is calculated as t/(1 + ϵ) to determine
which nodes remain active based on edge weights.
Nodes are marked as active if they have edges with
weights exceeding the pruning threshold. This ensures
that only nodes with significant similarity connections are
considered for clustering.The implementation uses a twostep
process in each iteration. Affinity Clustering and
Subgraph HAC. In Affinity Clustering Active nodes are
grouped into clusters using a size-constrained affinity algorithm,
ensuring clusters are small enough for parallel
processing. For Subgraph HAC Within each cluster, the
ApproximateSubgraphHacWrapper is called to perform
(1 + ϵ)-good merges. The results include:
• Cluster merges.
• Updated similarity metadata.
This step is parallelized over clusters to optimize performance.
After subgraph HAC, the graph is contracted by updating
cluster mappings and reducing the graph size. After
each round:
• Nodes with no active edges (below the pruning threshold)
are removed, reducing the graph size.
• The process repeats with the updated graph until no active
nodes remain.
Performance metrics, such as the number of outer rounds, are
logged for analysis. The Cluster method produces a flat
clustering by flattening the hierarchical structure based on the
linkage threshold. The HierarchicalCluster method
creates a full dendrogram representing the hierarchy of clusters.
A parallel dendrogram is converted into a standard dendrogram
format.Each dendrogram node contains its parent
node and merge similarity, providing a complete hierarchical
view of the clustering process.
