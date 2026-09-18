# Source Code Documentation

## 1. Overview

The Python implementation contains functions for:

- input processing
- graph initialization
- FM partitioning
- gain calculation
- cell movement
- rollback
- recursive partitioning
- Graphviz visualization

This document describes the purpose of each major function in the source
code.

## 2. Function Summary

| Function | Purpose |
|---|---|
| `initGraph()` | Converts the adjacency matrix to an adjacency list |
| `randomPartition()` | Generates an initial two-way partition |
| `inSameSet()` | Determines whether two blocks are in the same partition |
| `printBox()` | Prints formatted execution messages |
| `initBucket()` | Initializes gains, buckets and cut information |
| `get_compliment_set()` | Determines the opposite partition |
| `find_maximum_gain_cells()` | Searches gain buckets for a movable block |
| `moveCellAndUpdate()` | Moves a block and updates affected gains |
| `rollBackToBestCut()` | Restores the best partition encountered in a pass |
| `fmPass()` | Performs one FM pass |
| `fm()` | Controls FM partitioning passes |
| `pruneAdjL()` | Creates the reduced adjacency list for recursion |
| `pruneAreaDict()` | Creates the reduced area dictionary for recursion |
| `fmRecursive()` | Performs recursive FM partitioning |
| `readInput()` | Reads the two input files |
| `get_color()` | Generates visualization colors |
| `getEdgeList()` | Generates a unique edge list |
| `drawBlockRecursive()` | Recursively draws the partition hierarchy |
| `drawBlocks()` | Creates the Graphviz graph |
| `main()` | Coordinates the complete program |

## 3. `initGraph()`

### Purpose

Converts the input adjacency matrix into the adjacency-list representation
used by the FM algorithm.

### Input

```text
Adjacency matrix
```

### Output

```text
Adjacency list
MAX_GAIN-related value
```

### Role

The adjacency list makes it possible for later functions to access the
neighbors and edge weights associated with a block.

## 4. `randomPartition()`

### Purpose

Generates the initial two-way partition of the graph.

### Output

Two sets representing the initial partitions:

```text
A
B
```

### Role

These partitions provide the starting point for FM optimization.

## 5. `inSameSet()`

### Purpose

Determines whether two specified blocks currently belong to the same
partition.

### Result

Returns a Boolean result:

```text
True
```

when the blocks belong to the same partition, otherwise:

```text
False
```

### Role

This information is required when determining whether an edge is internal or
external.

## 6. `printBox()`

### Purpose

Prints formatted messages during execution.

### Role

Used to make important stages of the FM partitioning process easier to
identify in console output.

## 7. `initBucket()`

### Purpose

Initializes the FM gain-related data structures.

### Operations

The function determines:

- internal connectivity
- external connectivity
- gain values
- gain-bucket membership
- initial cut information

### Gain

Conceptually:

\[
Gain(v) = External(v) - Internal(v)
\]

### Role

This function prepares the data required before cell movement begins.

## 8. `get_compliment_set()`

### Purpose

Returns the partition opposite to the partition containing a specified
block.

Conceptually:

```text
If block is in A -> opposite partition is B
If block is in B -> opposite partition is A
```

### Role

Used during block movement and partition manipulation.

## 9. `find_maximum_gain_cells()`

### Purpose

Searches the gain buckets for a suitable movable block.

### Selection Considerations

The function considers:

- current gain;
- current partition;
- block area;
- partition balance.

### Role

Determines the next candidate block for an FM movement.

## 10. `moveCellAndUpdate()`

### Purpose

Moves a selected block from its current partition to the opposite partition.

### Operations

The function:

1. examines neighboring blocks
2. updates affected gain values
3. updates gain buckets
4. updates the cut
5. removes the moved block from the active gain information;
6. removes the block from its old partition
7. inserts the block into its new partition

### Cut Update

Conceptually:

\[
Cut_{new} = Cut_{old} - Gain(u)
\]

where `u` is the moved block.

## 11. `rollBackToBestCut()`

### Purpose

Restores the best partition encountered during the FM pass.

### Reason

The final movement of a pass is not necessarily the movement that produced
the minimum cut.

For example:

```text
Cut = 14
Cut = 10
Cut =  8  <- best
Cut =  9
Cut = 12
```

The function restores the state associated with:

```text
Cut = 8
```

## 12. `fmPass()`

### Purpose

Performs one complete FM pass.

### Major Operations

```text
Calculate partition areas
        |
        v
Initialize gains/buckets
        |
        v
Select movable block
        |
        v
Move block
        |
        v
Lock block
        |
        v
Update partition sizes
        |
        v
Continue movements
        |
        v
Rollback to best cut
```

### Output

Returns the best partition and cut information found during the pass.

## 13. `fm()`

### Purpose

Controls the overall FM partitioning procedure.

### Operations

The function:

1. creates the initial partition;
2. performs FM passes;
3. compares partition cut results;
4. stops when the pass condition used by the implementation is reached;
5. returns the resulting two-way partition.

## 14. `pruneAdjL()`

### Purpose

Creates a reduced adjacency list for recursive partitioning.

### Example

Suppose the original graph contains:

```text
{0, 1, 2, 3}
```

and recursion needs to operate only on:

```text
{0, 1}
```

the other blocks and their corresponding connections are removed from the
graph used by that recursive call.

### Role

Allows FM to operate independently on each recursively generated subgraph.

## 15. `pruneAreaDict()`

### Purpose

Creates the corresponding reduced block-area dictionary for a recursive
subgraph.

### Role

Keeps the area information consistent with the blocks remaining in the
current recursive partition.

## 16. `fmRecursive()`

### Purpose

Recursively applies FM partitioning.

### Base Case

When the current partition contains one block:

```text
STOP
```

### Recursive Case

Otherwise:

```text
Current Graph
     |
     v
    FM
   /  \
  /    \
 A      B
 |      |
 v      v
FM(A)  FM(B)
```

### Output

Produces the recursive partition structure together with cut information.

## 17. `readInput()`

### Purpose

Reads:

```text
input_1.txt
input_2.txt
```

### `input_1.txt`

Provides:

- number of blocks;
- width;
- height.

The function constructs block-dimension information and calculates block
areas.

### `input_2.txt`

Provides:

- weighted adjacency matrix.

### Output

The parsed data is returned for graph construction and FM partitioning.

## 18. `get_color()`

### Purpose

Generates a color used by the Graphviz visualization.

### Role

Different generated colors help visually distinguish parts of the recursive
partition hierarchy.

## 19. `getEdgeList()`

### Purpose

Converts the adjacency-list connectivity information into a unique edge list
for visualization.

### Reason

The graph is undirected, so an adjacency list can contain both:

```text
u -> v
v -> u
```

The function prevents the same undirected edge from being added twice to the
visualization.

## 20. `drawBlockRecursive()`

### Purpose

Recursively traverses the partition hierarchy and constructs Graphviz nodes
and clusters.

### Leaf Partition

When the partition contains one block, the function creates the graphical
block node.

The node includes:

- block ID;
- block dimensions;
- best-cut information.

### Internal Partition

When the partition contains subpartitions, Graphviz clusters are created and
the function recursively processes the children.

## 21. `drawBlocks()`

### Purpose

Creates the main Graphviz graph.

### Operations

The function:

1. creates the Graphviz graph;
2. calls `drawBlockRecursive()`;
3. obtains the graph edge list;
4. draws connectivity edges;
5. attaches connectivity weights as edge labels.

### Output

Returns the completed Graphviz graph object.

## 22. `main()`

### Purpose

Coordinates the complete application.

### Processing Flow

```text
readInput()
     |
     v
initGraph()
     |
     v
fmRecursive()
     |
     v
Print Final Partitions
     |
     v
drawBlocks()
     |
     v
View / Render / Save Graph
```

### Final Console Information

The function prints:

```text
FINAL PARTITIONS
Partition: ...
Best cut ...
```

and then creates the graphical representation of the partition hierarchy.

## 23. Program Entry Point

The source contains the standard Python direct-execution condition:

```text
if __name__ == '__main__':
```

When the Python file is executed directly, the program uses:

```text
./input_1.txt
./input_2.txt
```

as its input files and invokes the main processing function.
