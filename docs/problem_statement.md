# Recursive partitioning using the FM Algorithm

## 1. Objective

To develop a Python program for recursively partitioning a given set
of blocks using the Fiduccia Mattheyses (FM) algorithm until each
partition contains a single block.

## 2. Inputs

The program should accept:

- Number of blocks N
- Width and height of each block
- N × N weighted adjacency matrix

The adjacency matrix represents the number of interconnects between
blocks.

## 3. Input Files

The input is provided using:

- `input_1.txt`
- `input_2.txt`

`input_1.txt` contains block dimensions.

`input_2.txt` contains the weighted adjacency matrix.

## 4. Partitioning Objective

The FM algorithm is used to divide the blocks into two partitions
while considering partition balance and inter-partition cut weight.
The resulting partitions are recursively partitioned until every
leaf partition contains one block.

## 5. Output

The program should print the generated partitions and corresponding cut
information.
It should also generate a graphical representation of the recursive
partition hierarchy (using Graphviz).
