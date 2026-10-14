# Incremental Parametric Cut (IPC) procedure for Densest Subgraph Problem

This repository provides an exact densest subgraph solver, using the Incremental Parametric Cut (IPC) implementation of the HPF (Pseudoflow) algorithm for minimum cut (and maximum flow). In the proxy parametric minimum cut problem, the capacity of source-adjacent arcs is monotone non-increasing in the parameter lambda whereas the capacity of sink-adjacent arcs is monotone non-decreasing in lambda. This solver requires that the weights of the arcs and the nodes be positive; node values (see `--valuedNodes`) may be positive, zero or negative.

This implementation follows the Incremental Parametric Cut procedure as described in:

    D. S. Hochbaum, “Flow is best, fast and scalable: The incremental parametric cut for maximum density and other ratio subgraph problems,” in The 16th International Joint Conference on Knowledge Discovery, Knowledge Engineering and Knowledge Management (KDIR 2024), Porto-Portugal, 17-19 November, 2024, Proceedings, 2024.

An experimental study of this implementation is presented in:

    D. S. Hochbaum, A. Irribarra-Cortés, O. Goldschmidt, R. Asín-Achá, “Fast and Optimal Incremental Parametric Procedure for the Densest Subgraph Problem: An Experimental Study,” arXiv:2509.14993 [cs.DS], 2025. https://arxiv.org/abs/2509.14993

This implementation is based on the original implementation of the [Simple Parametric HPF](https://riot.ieor.berkeley.edu/Applications/Pseudoflow/parametric.html).

# Compiling

A makefile is provided. To compile the `IPC-DSP` executable, use:

```
mkdir bin
make
```

# Usage

The executable reads the input file from stdin. To execute one of the examples, use:
```
./bin/IPC-DSP < examples/example_unweighted.txt
```

# Options

This software provides the following options to modify its default behaviour.

`--help` : Print a help message

`--accuracy C` : Set the accuracy of the density to C decimal places. Note that a value of C too high may produce errors, especially on big graphs or graphs with big weights. By default, 4 decimal places are used.

`--startLambda L` : Start the incremental procedure with feasible subgraph density L. Note that a value of L larger than the optimal may produce errors. By default, the software starts with the density of the entire graph.

`--weightedEdges` : Use this option if the edges have non-unit, positive integer weights.

`--weightedNodes` : Use this option if the nodes have non-unit, positive integer weights.

`--valuedNodes` : Use this option if the nodes have values: integers that may be positive, zero or negative, added to the numerator of the density. A node value is equivalent to a self-loop of that weight on the node. By default, every node value is 0.

`--dumpDensest FILE` : Dump the list of nodes that are in the optimal solution to FILE.

# Input format

The input describes an undirected graph G = (V, E) with `n` = |V| nodes, labeled 1, ..., `n`, and `m` = |E| edges. Each node i has a weight q_i > 0, which counts in the denominator of the density, and a value u_i, which may be positive, zero or negative and counts in the numerator. Each edge [i, j] has a weight w_ij > 0. The solver finds a non-empty subset S of V that maximizes

$$
\frac{\displaystyle\sum_{[i,j] \in E,\; i,j \in S} w_{ij} \;+\; \sum_{i \in S} u_i}{\displaystyle\sum_{i \in S} q_i}
$$

**Optional fields.** Three fields are optional. Each one appears in the input file only if its option is given; otherwise it is omitted from the file and takes its default value:

- `WEIGHT_NODE_i` is q_i. It is read only with `--weightedNodes`; default 1.
- `VALUE_NODE_i` is u_i. It is read only with `--valuedNodes`; default 0.
- `WEIGHT_EDGE_k` is the weight of the k-th edge. It is read only with `--weightedEdges`; default 1.

All fields are integers separated by white space. In the format below, optional fields are shown in brackets `[ ]`.

**Edge convention.** Each undirected edge is listed exactly once, as `TAIL HEAD` with `TAIL` < `HEAD`.

**Format.** The first line contains `n` and `m`. If at least one of `--weightedNodes` or `--valuedNodes` is given, then come `n` node lines, where line i holds the optional fields of node i, in the order shown; otherwise these lines are absent. Then come `m` edge lines.

```
n m
[WEIGHT_NODE_1] [VALUE_NODE_1]
[WEIGHT_NODE_2] [VALUE_NODE_2]
...
[WEIGHT_NODE_n] [VALUE_NODE_n]
TAIL_EDGE_1 HEAD_EDGE_1 [WEIGHT_EDGE_1]
TAIL_EDGE_2 HEAD_EDGE_2 [WEIGHT_EDGE_2]
...
TAIL_EDGE_m HEAD_EDGE_m [WEIGHT_EDGE_m]
```

See directory [examples](examples) for sample input files; each file name indicates which options to use (e.g., `example_weighted_valued_nodes_edges.txt` requires `--weightedNodes --valuedNodes --weightedEdges`).

# License

Copyright © 2024. The Regents of the University of California (Regents). All Rights Reserved.

Permission to use, copy, modify, and distribute this software and its documentation for educational, research, and not-for-profit purposes, without fee and without a signed licensing agreement, is hereby granted, provided that the above copyright notice, this paragraph and the following two paragraphs appear in all copies, modifications, and distributions. Contact The Office of Technology Licensing, UC Berkeley, 2150 Shattuck Avenue, Suite 510, Berkeley, CA 94720-1620, (510) 643-7201, for commercial licensing opportunities. Created by Ayleen Irribarra and Dorit S. Hochbaum, Department of Industrial Engineering and Operations Research, University of California, Berkeley. This work is adapted from the Pseudoflow implementation by Bala Chandran and Dorit S. Hochbaum available at https://riot.ieor.berkeley.edu/Applications/Pseudoflow/maxflow.html

IN NO EVENT SHALL REGENTS BE LIABLE TO ANY PARTY FOR DIRECT, INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES, INCLUDING LOST PROFITS, ARISING OUT OF THE USE OF THIS SOFTWARE AND ITS DOCUMENTATION, EVEN IF REGENTS HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

REGENTS SPECIFICALLY DISCLAIMS ANY WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE SOFTWARE AND ACCOMPANYING DOCUMENTATION, IF ANY, PROVIDED HEREUNDER IS PROVIDED “AS IS”. REGENTS HAS NO OBLIGATION TO PROVIDE MAINTENANCE, SUPPORT, UPDATES, ENHANCEMENTS, OR MODIFICATIONS.
