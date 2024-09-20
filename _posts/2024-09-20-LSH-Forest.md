---
title: 'LSH Forest'
date: 2024-09-11
permalink: /posts/2024/09/LSH-Forest/
layout: default
tags:
  - TMAP
---
*Locality-sensitive hashing* or _LSH_, allows us to focus on pairs that are **likely** to be similar, instead of having to look at all pairs possible. It reduces therefore the amount of computational time required by a brute force approach. 

In essence, we take the items, we obtain a hash of these items and store them in buckets with other items with similar hashes.  What we are trying to accomplish is to aggregate into a bucket those items with similar hashes to then only search for similar pairs within the bucket instead of looking at all possibilities. This of course requires that similar items will have similar hashes. That is why the hash functions we're using are carefully designed. 

There are 3 fundamental steps in this process:

1. Creating multiple LSH tables, each using a **subset** of the MinHash signature
2. Each table is structured as a prefix tree (trie) for quick lookup
3. When querying, traverse the trees to find candidate nearest neighbours. 

#### What is a LSH Table? 
A LSH table is a data structure that groups similar items together based on their **hash** values. Each item -_molecules in our case-_ has its own MinHash vecotr, and all of these vectors are stored in the LSH Table. 

Let's do a practical example: 

### Creating multiple LSH tables, each using a **subset** of the MinHash signature
1. Original Fingerprint
Let's say we have a 16-bit fingerprint for a molecule: 
`1011000110101101`

2. MinHash Transformation 
We apply multiple hash functions to this fingerprint. For simplicity, let's say we use 4 hash functions, resulting in a MinHash signature of 4 integers: [42, 17, 93, 28]
 
 *To remember how this MinHash signature is calculated see the example on the [MinHash blogpost](https://afloresep.github.io/posts/2024/09/MinHashing/)*

3. LSH Table Creation Let's assume we're creating 2 LSH Tables, each using 2 of the 4 MinHash values (a subset of the MinHash)

- Table 1 uses [42, 17]
- Table 2 uses [93, 28]

We'll focus on Table 1 for this example.

4. Binary Representation 
We convert the MinHash values to binary. Let's say we're using the last 4 bits of each number: 42 -> 101010 -> 1010 17 -> 010001 -> 0001

So for Table 1, our hash becomes: **1010***0001*

### Each table is structured as a prefix tree (trie) for quick lookup
5. Trie construction
We insert this binary string into the [trie](The ****Trie data structure**** is a tree-like data structure used for storing a dynamic set of strings.). Each bit determines which path to take at each level.

```mermaid
graph TD
    A[Root] --> B0[0]
    A --> B1[1]
    B1 --> C10[0]
    B1 --> C11[1]
    C10 --> D100[0]
    C10 --> D101[1]
    D101 --> E1010[0]
    D101 --> E1011[1]
    E1010 --> F10100[0]
    F10100 --> G101000[0]
    G101000 --> H1010000[0]
    H1010000 --> I10100001[1 Leaf: Our Molecule]
```
6. Searching for Similar Items: 
When searching for ismilar items, we follow the same process. 
	1. Generate fingerprint for our molecule 
	2. Create its MinHash signature
	3. Use the subset for Table 1 (in this case, the first 2 MinHash values) 
	4. Convert to binary
	5. Traverse the trie
Once we reach a leaf node, all the items stored at this leaf and nearby leaves are considered **candidate nearest neighbours**. 

```mermaid
graph TD
    A[LSH Forest] --> B1[Table 1]
    A --> B2[Table 2]
    A --> B3[...]
    A --> B4[Table N]
    B1 --> C1[Trie 1]
    B2 --> C2[Trie 2]
    B4 --> C4[Trie N]
    C1 --> D1[Root]
    D1 --> E1[0]
    D1 --> E2[1]
    E1 --> F1[00]
    E1 --> F2[01]
    E2 --> F3[10]
    E2 --> F4[11]
    F1 --> G1[Leaf: Items]
    F2 --> G2[Leaf: Items]
    F3 --> G3[Leaf: Items]
    F4 --> G4[Leaf: Items]
```

### When querying, traverse the trees to find candidate nearest neighbours. 
Since we have several LSH tables, the algorithm actually uses all the LSH tables simultaneously, rather than selecting a single table. This **improves** the accuracy of the approximate nearest neighbour search. From each table, the algorithm collects a set of candidate nearest neighbours. The final set of candidate nearest neighbors is the union of candidates from all tables.
```mermaid
graph TD
    A[Query Molecule] --> B1[LSH Table 1]
    A --> B2[LSH Table 2]
    A --> B3[...]
    A --> B4[LSH Table N]
    B1 --> C1[Candidate Set 1]
    B2 --> C2[Candidate Set 2]
    B3 --> C3[...]
    B4 --> C4[Candidate Set N]
    C1 --> D[Union of Candidate Sets]
    C2 --> D
    C3 --> D
    C4 --> D
    D --> E[Final Candidate Set]
```
Bibliography: 
- **[Mining of Massive Datasets](http://infolab.stanford.edu/~ullman/mmds/book0n.pdf)** - Jure Leskovec, Anand Rajaraman & Jeffrey D. Ullman 
