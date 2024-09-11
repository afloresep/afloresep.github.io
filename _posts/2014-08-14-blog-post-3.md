---
title: 'MinHashing'
date: 2024-09-11
permalink: /posts/2024/09/MinHashing/
tags:
  - TMAP
  - category1
  - category2
---

TL;DR: 
MinHashing is a way to reduce the dimensionality of fingerprints. Instead of comparing 512-bit vectors directly _-these are the vectors that we obtain after the fingerprint is applied to a SMILES format molecule-_, MinHash reduces these vectors to smaller hashes that preserve similarity between molecules.

### MinHashing with Fingerprints
The goal is to efficiently estimate the Jaccard similarity between sets (or fingerprints). 
To do this, MinHash applies **permutations** to the elements of the binary vectors (representing sets) and selects the **minimum hashed value** for each permutation.

### Basic Steps in MinHashing with Fingerprints:
1. Represent the data as **binary vectors** (i.e., fingerprints).
2. Apply **permutations** to the indices of these vectors.
3. For each permutation, find the **minimum index** where the fingerprint vector has a `1`.
4. Repeat this process for multiple permutations to get a **MinHash signature** for each vector.

### Example Step by Step
#### Step 1: Start with a Matrix of Fingerprints (Binary Vectors)

Let’s assume you have a simple dataset of 3 fingerprints (molecules) represented as binary vectors, where `1` indicates the presence of a feature and `0` indicates its absence.

Here is the **fingerprint matrix** (each row is a molecule, and each column is a feature):

| Molecule A     | 0 | 1 | 0 | 1 | 0 |
| -------------- | - | - | - | - | - |
| **Molecule B** | 1 | 0 | 1 | 1 | 0 |
| **Molecule C** | 1 | 1 | 0 | 0 | 1 |


- Molecule A: Features 2 and 4 are present.
- Molecule B: Features 1, 3, and 4 are present.
- Molecule C: Features 1, 2, and 5 are present.
#### Step 2: Apply a Permutation to the Indices

A **permutation** is just a rearrangement of the row indices. For example, if we permute the rows as follows:

- Permutation 1: [3, 5, 2, 1, 4]

This means that in this permutation, feature 3 is considered first, then feature 5, and so on.
#### Step 3: Find the Minimum Index for Each Fingerprint (Vector)

Now, for each fingerprint, we go through the permutation and pick the **minimum index** where the vector has a `1`.

Let's go through the permutation `[3, 5, 2, 1, 4]` for each fingerprint.

1. **Molecule A** (vector: `[0, 1, 0, 1, 0]`):
    - Start with feature 3: value is `0`.
    - Next is feature 5: value is `0`.
    - Next is feature 2: value is `1` → **Minimum index** = `2` (Feature 2).
2. **Molecule B** (vector: `[1, 0, 1, 1, 0]`):
    - Start with feature 3: value is `1` → **Minimum index** = `3` (Feature 3).
3. **Molecule C** (vector: `[1, 1, 0, 0, 1]`):
    - Start with feature 3: value is `0`.
    - Next is feature 5: value is `1` → **Minimum index** = `5` (Feature 5).

So after applying the first permutation `[3, 5, 2, 1, 4]`, we get the following **MinHash values**:

- Molecule A → MinHash = 2
- Molecule B → MinHash = 3
- Molecule C → MinHash = 5
#### Step 4: Repeat for Multiple Permutations

To improve the estimate of Jaccard similarity, you repeat the process using multiple random permutations. Each permutation provides one "hash value" for each fingerprint.

Let’s apply a second permutation: `[4, 1, 5, 2, 3]`.

1. **Molecule A** (vector: `[0, 1, 0, 1, 0]`):
    
    - Start with feature 4: value is `1` → **Minimum index** = `4` (Feature 4).
2. **Molecule B** (vector: `[1, 0, 1, 1, 0]`):
    
    - Start with feature 4: value is `1` → **Minimum index** = `4` (Feature 4).
3. **Molecule C** (vector: `[1, 1, 0, 0, 1]`):
    
    - Start with feature 4: value is `0`.
    - Next is feature 1: value is `1` → **Minimum index** = `1` (Feature 1).

Now after the second permutation, we get the following MinHash values:

- Molecule A → MinHash = 4
- Molecule B → MinHash = 4
- Molecule C → MinHash = 1

#### Step 5: Build the MinHash Signature

After applying multiple permutations (let’s say we applied 3 permutations in total), each fingerprint will have a **MinHash signature**, which is a vector of the minimum hash values from each permutation.

For example, after 3 permutations, the MinHash signatures might look like this:

- **Molecule A**: `[2, 4, 3]`
- **Molecule B**: `[3, 4, 2]` 
- **Molecule C**: `[5, 1, 1]`

Each element in these vectors represents the minimum index found in one of the permutations.

#### Step 6: Calculate Jaccard Similarity Using MinHash

Once you have the **MinHash signatures** for each molecule, you can approximate the **Jaccard similarity** between any two molecules by comparing their signatures.

For example:

- **Molecule A** vs. **Molecule B**:
    
    - Compare `[2, 4, 3]` with `[3, 4, 2]`.
    - They match in 1 out of 3 positions (`4`).
    - Jaccard similarity estimate = `1/3 = 0.33`.
- **Molecule A** vs. **Molecule C**:
    
    - Compare `[2, 4, 3]` with `[5, 1, 1]`.
    - No matches.
    - Jaccard similarity estimate = `0/3 = 0.0`.

The more permutations you apply, the better the approximation of the true Jaccard similarity.
