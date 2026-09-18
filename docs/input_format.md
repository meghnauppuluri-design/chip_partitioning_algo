# Input Format

## 1. Overview

The program reads two files:

```text
input_1.txt
input_2.txt
```

`input_1.txt` describes the blocks.

`input_2.txt` describes the connectivity between the blocks.

---

## 2. `input_1.txt`

The first file contains:

1. number of blocks;
2. width of each block; and
3. height of each block.

Reference input:

```text
{6, {10, 5}, {8, 6}, {4, 13}, {10, 3}, {8, 4}, {4, 1}}
```

The first value:

```text
6
```

represents:

```text
N = 6
```

The remaining pairs represent:

```text
{width, height}
```

for each block.

Therefore:

```text
Block 0 -> {10, 5}
Block 1 -> {8, 6}
Block 2 -> {4, 13}
Block 3 -> {10, 3}
Block 4 -> {8, 4}
Block 5 -> {4, 1}
```

---

## 3. Block Area

The program calculates block area using:

\[
Area = Width \times Height
\]

For the reference input:

| Block | Width | Height | Area |
|------:|------:|-------:|-----:|
| 0 | 10 | 5 | 50 |
| 1 | 8 | 6 | 48 |
| 2 | 4 | 13 | 52 |
| 3 | 10 | 3 | 30 |
| 4 | 8 | 4 | 32 |
| 5 | 4 | 1 | 4 |

---

## 4. `input_2.txt`

The second input file contains the weighted adjacency matrix.

For `N = 6`, the matrix contains six rows and six columns.

Reference input:

```text
{0 1 3 2 4 2
 1 0 3 1 2 1
 3 3 0 1 1 2
 2 1 1 0 2 3
 4 2 1 2 0 1
 2 1 2 3 1 0}
```

---

## 5. Matrix Interpretation

An entry:

\[
C[i][j]
\]

represents the connectivity weight between block `i` and block `j`.

For example:

```text
C[0][1] = 1
```

means that the connectivity weight between blocks `0` and `1` is `1`.

Similarly:

```text
C[0][4] = 4
```

means that the connectivity weight between blocks `0` and `4` is `4`.

---

## 6. Reference Matrix

The complete reference matrix is:

|   | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---:|---:|---:|---:|---:|---:|
| **0** | 0 | 1 | 3 | 2 | 4 | 2 |
| **1** | 1 | 0 | 3 | 1 | 2 | 1 |
| **2** | 3 | 3 | 0 | 1 | 1 | 2 |
| **3** | 2 | 1 | 1 | 0 | 2 | 3 |
| **4** | 4 | 2 | 1 | 2 | 0 | 1 |
| **5** | 2 | 1 | 2 | 3 | 1 | 0 |

The diagonal entries are zero because the reference data does not represent
a block as being connected to itself.

---

## 7. Input Processing

The function:

```text
readInput()
```

reads both files.

It produces the information required by the remainder of the program,
including:

- adjacency matrix;
- block dimensions; and
- block areas.

The adjacency matrix is subsequently passed to:

```text
initGraph()
```

which constructs the adjacency-list representation used by the FM algorithm.
