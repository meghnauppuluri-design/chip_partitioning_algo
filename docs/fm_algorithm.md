# FM Recursive Partitioning Algorithm

## 1. Overview

The program performs recursive graph partitioning using the
**Fiduccia–Mattheyses (FM) algorithm**.

The major processing stages are:

```text
Input
  |
  v
readInput()
  |
  v
initGraph()
  |
  v
fmRecursive()
  |
  v
fm()
  |
  v
fmPass()
  |
  v
Two-Way Partition
  |
  +----------------+
  |                |
  v                v
Partition A    Partition B
  |                |
  v                v
Recursive FM    Recursive FM
```

## 2. Graph Initialization

The input connectivity is represented using an adjacency matrix.

The function:

```text
initGraph()
```

converts this matrix into an adjacency-list representation.

Conceptually:

```text
Adjacency Matrix
       |
       v
   initGraph()
       |
       v
Adjacency List
```

The adjacency list stores the neighboring blocks and corresponding
connectivity weights.

## 3. Initial Partition

The function:

```text
randomPartition()
```

generates the initial two-way partition.

The blocks are separated into:

```text
Partition A
Partition B
```

The FM algorithm then attempts to improve this partition.

## 4. Internal and External Connections

For a block in one partition, an edge may be:

### Internal

Both endpoints are in the same partition.

```text
Partition A

0 -------- 1
```

### External

The endpoints are in different partitions.

```text
Partition A       Partition B

     0 ------------- 2
```

External edges contribute to the partition cut.

## 5. Cut

The cut is the total weight of edges whose endpoints belong to different
partitions.

Conceptually:

Cut(A, B) = Sum of w(u, v), for all u in A and v in B

The FM procedure attempts to obtain a lower cut while respecting the
partition balance condition implemented in the program.

## 6. Gain

The gain of a block represents the effect of moving that block to the
opposite partition.

Conceptually:

Gain(v) = External(v) - Internal(v)

A positive gain means that moving the block can reduce the current cut.

A negative gain means that moving the block increases the cut at that stage.

## 7. Gain Bucket Initialization

The function:

```text
initBucket()
```

calculates:

- block gains;
- initial cut;
- gain-bucket information.

Conceptually:

```text
Gain +3 : blocks with gain +3
Gain +2 : blocks with gain +2
Gain +1 : blocks with gain +1
Gain  0 : blocks with gain  0
Gain -1 : blocks with gain -1
...
```

## 8. Selecting a Cell

The function:

```text
find_maximum_gain_cells()
```

searches the available gain buckets.

The selected block must satisfy the balance condition used by the
implementation.

Therefore, selection considers both:

```text
Gain
  +
Partition Balance
```

## 9. Moving a Cell

The selected cell is moved using:

```text
moveCellAndUpdate()
```

For example:

```text
Before:

A = {0, 1, 2}
B = {3, 4, 5}

Move block 2 from A to B

After:

A = {0, 1}
B = {2, 3, 4, 5}
```

After the movement, the cut is updated according to the gain of the moved
block.

Conceptually:

$$
Cut_{\text{new}} = Cut_{\text{old}} - Gain(u)
$$

## 10. Updating Neighbor Gains

When a block moves, its incident edges may change from:

```text
internal -> external
```

or:

```text
external -> internal
```

Therefore, gains of affected neighboring blocks also change.

The function:

```text
moveCellAndUpdate()
```

updates the corresponding gains and gain buckets.

## 11. Locking

A block that has moved during the current FM pass is locked.

A locked block is not moved again during the same pass.

Conceptually:

```text
Select
   |
   v
Move
   |
   v
Lock
   |
   v
Continue
```

## 12. FM Pass

The function:

```text
fmPass()
```

performs one complete FM pass.

The flow is:

```text
Initialize gains and buckets
           |
           v
Find movable cell
           |
           v
Move cell
           |
           v
Lock cell
           |
           v
Update neighbor gains
           |
           v
Update cut
           |
           v
Record move
           |
           v
Continue
           |
           v
Rollback to best cut
```

## 13. Rollback

The best partition may occur before the final movement of an FM pass.

Example:

```text
Initial cut = 15

Move 1 -> 12
Move 2 ->  9   <-- best
Move 3 -> 10
Move 4 -> 13
```

The function:

```text
rollBackToBestCut()
```

restores the partition corresponding to the best cut encountered during the
pass.

In this example:

```text
Best cut = 9
```

## 14. Multiple FM Passes

The function:

```text
fm()
```

controls the FM partitioning process over multiple passes.

Conceptually:

```text
Initial Partition
       |
       v
    FM Pass
       |
       v
Better Partition
       |
       v
    FM Pass
       |
       v
Final Partition
```

## 15. Recursive Partitioning

The function:

```text
fmRecursive()
```

performs recursive partitioning.

Suppose the original graph contains:

```text
{0, 1, 2, 3, 4, 5}
```

After the first FM partition:

```text
              {0,1,2,3,4,5}
               /           \
              /             \
             A               B
```

The algorithm then independently processes `A` and `B`.

This continues recursively.

## 16. Graph Pruning

For recursive processing, the program constructs smaller graphs containing
only the blocks belonging to the current partition.

The functions:

```text
pruneAdjL()
pruneAreaDict()
```

prepare the corresponding adjacency and area information.

## 17. Base Case

Recursive partitioning terminates when only one block remains in the current
partition.

```text
Current Partition
       |
       v
     {Block}
       |
       v
      STOP
```

This block becomes a leaf of the recursive partition hierarchy.

## 18. Complete Flow

```text
                  INPUT FILES
                       |
                       v
                  readInput()
                       |
                       v
                   initGraph()
                       |
                       v
                  fmRecursive()
                       |
                       v
                      fm()
                       |
                       v
                    fmPass()
                       |
             +---------+---------+
             |                   |
             v                   v
        Partition A         Partition B
             |                   |
             v                   v
       pruneAdjL()          pruneAdjL()
       pruneAreaDict()      pruneAreaDict()
             |                   |
             v                   v
       fmRecursive()        fmRecursive()
             |                   |
             +---------+---------+
                       |
                       v
             Single-block leaves
                       |
                       v
              drawBlockRecursive()
                       |
                       v
                  drawBlocks()
                       |
                       v
             Graphviz Visualization
```
