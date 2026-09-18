# Partition Visualization

## 1. Overview

The program uses Graphviz to generate a graphical representation of the
recursive FM partition hierarchy.

The visualization represents both:

1. the recursive grouping of blocks
2. the weighted connectivity between individual blocks

## 2. Visualization Functions

The visualization is implemented using:

```text
get_color()
getEdgeList()
drawBlockRecursive()
drawBlocks()
```

## 3. Individual Blocks

A single block is represented as a rectangular Graphviz node.

The node contains:

```text
Block ID
Width × Height
Best Cut
```

Conceptually:

```text
+----------------+
|       4        |
|      8x4       |
|   best_cut=... |
+----------------+
```

This makes the physical block dimensions visible directly in the generated
graph.

## 4. Recursive Groups

Recursive partitions are represented using Graphviz subgraph clusters.

For example:

```text
+-------------------------------------------+
| Parent Partition                          |
|                                           |
|   +----------------+ +----------------+   |
|   | Partition A    | | Partition B    |   |
|   |                | |                |   |
|   | +----+ +----+  | | +----+ +----+ |   |
|   | | 0  | | 1  |  | | | 2  | | 3  | |   |
|   | +----+ +----+  | | +----+ +----+ |   |
|   +----------------+ +----------------+   |
|                                           |
+-------------------------------------------+
```

The nesting of clusters represents the recursive partition hierarchy.

## 5. Hierarchy

Suppose recursive FM generates:

```text
               {0,1,2,3}
                /     \
               /       \
            {0,1}     {2,3}
             / \       / \
            0   1     2   3
```

The graphical representation uses nested Graphviz clusters corresponding to
this recursive structure.

## 6. Colors

The function:

```text
get_color()
```

generates colors used by the visualization.

The colors visually distinguish generated blocks or partition clusters.

## 7. Edge Generation

The function:

```text
getEdgeList()
```

constructs the edge list used for visualization.

Because the graph is undirected, connectivity can appear in both directions
in the adjacency list.

For example:

```text
0 -> 1
1 -> 0
```

represents one undirected edge.

`getEdgeList()` prevents the same edge from being drawn twice.

## 8. Weighted Connectivity

Original connectivity edges are drawn between the individual block nodes.

For example:

```text
+---+        4        +---+
| 0 |-----------------| 4 |
+---+                 +---+
```

The edge label represents the corresponding connectivity weight.

## 9. Recursive Drawing

The function:

```text
drawBlockRecursive()
```

traverses the recursive partition structure.

If the current partition contains one block, it draws an individual block.

If the current partition contains two recursively generated child
partitions, it creates Graphviz clusters and recursively draws both children.

## 10. Graph Construction

The function:

```text
drawBlocks()
```

creates the Graphviz graph and combines:

- recursive partition clusters
- individual block nodes
- connectivity edges
- connectivity-weight labels

## 11. Rendering

After the graph is created, the main program performs Graphviz operations to:

```text
view
render
save
```

the generated graph.

The resulting visualization provides a graphical representation of the final
recursive FM partition hierarchy.
