As practise, I attempted to solve the 2019 hashcode problem using python: https://codingcompetitions.withgoogle.com/hashcode/archive/2019

### Solution
This is a simple approach with a lot of room for improvement (there is a 4h time limit in the real competition). It basically follows two steps:

**(1)** Build a weighted graph from the input.
The vertices are the images or pairs of images to be put in the slideshow.
Here, I paired the vertical images without much thought so that their tags have small intersection (by selecting ~25 candidates randomly for each vertex and select the best one).
The weight of a pair of vertices is the score it would receive if adjacent in the final slideshow. The graph consists of all edges of positive weight.
Generally, the graphs are too large to calculate all weights (at least on my machine). In b) however, the graph is sparse and can be completely calculated.
For d) and e), we reveal only a fraction of the graph (by simply selecting a constant number of neighbours of each vertex).
For c), the graph is so small that it doesn't realy have an influence for the final score.

**(2)** Given a weighted graph $G = (V,E)$, construct a path of maximal weight (non-edges are seen as edges of weight 0 and can be used).
This is a dificult problem in general (NP-hard), and we take a greedy approach for an approximation. First, we order the edges according to their weight (if not passed ordered).
We will maintain a path forest $P$ (i.e. a set of vertex disjoint paths) at any step. Initially, $P$ is the empty graph.
Then, as long as $E$ is not empty, we select the first edge $e$ of $E$ (maximum weight). If adding $e$ to $P$ maintains a path forest, we add it, otherwise we discard it.
(In order to do this in constant time we keep a list of degrees and list which indicates which edges can't be chosen due to closing cycles).
Finally, when $E$ will be empty, we have a path forest and can combine it to a path using edges of weight $0$.

This scores just over a million points (depending on machine, time to wait for an answer and programming language).

### Room for improvement
- Do (1) and (2) at the same time, that is reveal constantly many edges, select the first edge, reveal constantly many edges, ...
(always only revealing edges which we can still select, or at least those which don't touch a vertex of degree $2$ in $P$).
This way we will have less edges available in the easy part of the process and more in the difficult part. This should help.
- Pair the images cleverer, probably also while buliding the path forest, a few at a time.
