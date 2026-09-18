# Recursive FM Block Partitioning

Python implementation of **recursive block partitioning using the Fiduccia–Mattheyses (FM) algorithm**.

The program reads block dimensions and a weighted adjacency matrix, performs recursive two-way FM partitioning until each partition contains a single block, and generates a hierarchical Graphviz visualization.

## Documentation Navigation Hub

| Document                                         | Description                                   |
| ------------------------------------------------ | --------------------------------------------- |
| [Problem Statement](https://github.com/meghnauppuluri-design/chip_partitioning_algo/blob/main/docs/problem_statement.md)   | Project objective and requirements            |
| [Input Format](https://github.com/meghnauppuluri-design/chip_partitioning_algo/blob/main/docs/input_format.md)             | Block dimensions and adjacency matrix format  |
| [FM Algorithm](https://github.com/meghnauppuluri-design/chip_partitioning_algo/blob/main/docs/algorithm.md)                | FM algorithm and recursive partitioning flow  |
| [Code Documentation](https://github.com/meghnauppuluri-design/chip_partitioning_algo/blob/main/docs/code_documentation.md) | Function-by-function explanation              |
| [Visualization](https://github.com/meghnauppuluri-design/chip_partitioning_algo/blob/main/docs/visualization_graphviz.md)           | Graphviz hierarchical partition visualization |
| [Output Format](https://github.com/meghnauppuluri-design/chip_partitioning_algo/blob/main/docs/output_format.md)           | Console and graphical output                  |

## Source Code

The complete implementation is available here:

**[FM Partitioning Source Code](https://github.com/meghnauppuluri-design/chip_partitioning_algo/blob/main/src/fm_partitioning.py)**

## Output

The program produces:

* Recursive block partitions
* Partition cut information
* Hierarchical Graphviz visualization
