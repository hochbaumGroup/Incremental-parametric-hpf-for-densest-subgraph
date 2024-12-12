# Incremental cut procedure for Densest Subgraph Problem

This repository provides an exact densest subgraph extractor, using an incremental parametric implementation of pseudoflow for minimum cut on directed graphs. In the proxy parametric minimum cut problem, the capacity of source-adjacent arcs is monotone non-increasing in the parameter lambda whereas the capacity of sink-adjacent arcs is monotone non-decreasing in lambda. This solver requires that the weights of the arcs and the nodes be positive.

This implementation follows the procedure as described in:

    D. S. Hochbaum, “Flow is best, fast and scalable: The incremental parametric cut for maximum density and other ratio subgraph problems,” in The 16th International Joint Conference on Knowledge Discovery, Knowledge Engineering and Knowledge Management (KDIR 2024), Porto-Portugal, 17-19 November, 2024, Proceedings, 2024.

This implementation is based on the original implementation of the [Simple Parametric HPF](https://riot.ieor.berkeley.edu/Applications/Pseudoflow/parametric.html).

# Compiling

A makefile is provided. To compile the `incremental` executable, use:

```
mkdir bin
make
```

# Usage

The executable reads the input file from stdin. To execute one of the examples, use:
```
./bin/incremental < examples/example_unweighted.txt
```

# Options

This software provides the following options to modify its default behaviour.

`--help` : Print a help message

`--accuracy C` : Set the accuracy of the density to C decimal places. Note that a value of C too high may produce errors, especially on big graphs or graphs with big weights. By default, 4 decimal places are used.

`--startLambda L` : Start the incremental procedure with feasible subgraph density L. Note that a value of L larger than the optimal may produce errors. By default, the software starts with the density of the entire graph.

`--weightedEdges` : Use this option if the edges have non-unit, non-negative utilities.

`--weightedNodes` : Use this option if the nodes have non-unit, non-negative costs.

`--dumpSourceSet FILE` : Dump the list of nodes that are in the optimal solution to FILE.

# Input format

The input graph, with ``N`` nodes and ``M`` edges, is to be provided in the following text format:

```
n m
[WEIGHT_NODE_1]
[WEIGHT_NODE_2]
...
[WEIGHT_NODE_n]
FROM_EDGE_1 TO_EDGE_1 [WEIGHT_EDGE_1]
FROM_EDGE_2 TO_EDGE_2 [WEIGHT_EDGE_2]
...
FROM_EDGE_m TO_EDGE_m [WEIGHT_EDGE_m]
```

Remember that if the graph is weighted, option `--weightedEdges` must be used if the edges have weights (non-unit, non-negative utilities) and `--weightedNodes` if the nodes have weights (non-unit, non-negative costs).

See directory [examples](examples) for sample input files.

# License

