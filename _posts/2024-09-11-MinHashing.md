---
title: 'TMAP'
date: 2024-09-11
tags:
    - TMAP
    - MinHashing
---

### MinHashing:
TMAP employs **[MinHashing](https://afloresep.github.io/posts/2024/09/MinHashing/)**, which is better suited for comparing **Jaccard similarity** between sets, particularly for molecular fingerprints represented as binary vectors (presence/absence of molecular features).

1. **MinHashing**:
    - Instead of using random hyperplanes -_see Random Hyperplane LSH_, TMAP applies **MinHash** functions to generate **hash values** that summarize each fingerprint. The MinHash is designed to approximate the Jaccard similarity between two sets, which measures the overlap between two sets.
    - MinHash works by generating multiple hash functions, and for each function, the minimum value encountered in the binary vector is stored. This generates a signature (a smaller vector) for each datapoint.
2. **Binary Vector Construction**:
    - The resulting "MinHash signatures" are essentially reduced representations of the original binary fingerprints. These signatures still maintain the property that similar datapoints will have similar signatures.
3. **Similarity Measurement**:
    - Instead of Hamming distance, **Jaccard similarity** is used here. The similarity between two MinHash signatures is measured by how many of the hash values (or "minimums") match between them.
4. **Use Case**:
    - MinHashing and Jaccard similarity are ideal for datasets where you're comparing the overlap of sets or attributes. This makes MinHashing very effective for comparing **molecular fingerprints** in chemical databases, as it's **more about the shared structural features than the orientation of the vectors in space**.


> Data Types for TMAP 
> **VectorUints** and **VectorFloat** e.g. `fingerprints = [tm.VectorUint(enc.encode(s)) for s in df['canonical_smiles'`



### Use different descriptors/fingerprints
The descriptors/fingerprints need to be converted to [Minhash vectors](http://tmap.gdb.tools/#tmap.Minhash) first. It supports binary, indexed, string and also int and float weighted vectors as input and returns a list of Minhash vectors (List of `VectorUint`)

Methods for different types of input:

`batch_from_binary_array`. MinHash vectors from **binary** arrays. The input vectors need to be a list of `VectorUchar`.

`batch_from_int_weight_array`. Create MinHash vectors from **integer** arrays (not only zeros and ones). The input vectors need to be a list of `VectorUint`.

`batch_from_sparse_binary_array`. Create MinHash vectors from **sparse binary** arrays. The input vectors need to be a list of `VectorUint` – A list of vectors containing indices of ones in a binary array.

`batch_from_string_array`. Create MinHash vectors from **string** arrays. The input vector is a list of `list` or `string`.

`batch_from_weight_array`. Create MinHash vectors from **float** arrays. The input vectors need to be a list of `VectorFloat`. – A list of vectors containing float values. **Keyword Arguments**: **method** (str) – The weighted hashing method to use (`ICWS` (default) or `I2CWS`).


## Simple example of TMAP
```
from mhfp.encoder import MHFPEncoder 
from mhfp.lsh_forest import LSHForestHelper 

perm = 512 
enc = MHFPEncoder(perm) 
fingerprints = [tm.VectorUint(enc.encode(s)) for s in df['molecules' 

lf = tm.LSHForest(perm) 

# Add the Fingerprints to the LSH Forest and index lf.batch_add(fingerprints) 
lf.index() 

cfg = tm.LayoutConfiguration() #configuration parameters for tmap layout cfg.node_size = 1 / 26 #size of nodes which affects the magnitude of their repelling force. Decreasing this values generally resolves overlaps in a very crowded tree 

cfg.mmm_repeats = 2 #number of repeats of the per-level layout algorithm 

cfg.sl_extra_scaling_steps = 10 #sets the number of repeats of the scaling 

cfg.k = 20 #number of nearest neighbours used to create the k-nearest neighbour graph 

cfg.sl_scaling_type = tm.RelativeToAvgLength #Defines the relative scale of the graph 

x, y, s, t, _ = tm.layout_from_lsh_forest(lf, cfg) 

set1 = plt.get_cmap("Set1").colors 

bin_cmap = ListedColormap(["#DC267F", "#648FFF"], name="bin_cmap") 

f = Faerun( 
	view="front", coords=False, title="", clear_color="#FFFFFF", 
	) 

f.add_scatter( 
	"GDB13S_vs_USPTO_TMAP", { "x": x, "y": y, "c": [ df['labels'].values.tolist(), ], "labels": df['molecules'].values.tolist(), }, shader="smoothCircle", point_scale=5, max_point_size=20, legend_labels=[None], categorical=[True], colormap=[bin_cmap], series_title=['Jaccard distance'], has_legend=True, ) 
	
f.add_tree("GDB13S_vs_USPTO_TMAP_tree", {"from": s, "to": t}, point_helper="GDB13S_vs_USPTO_TMAP") 
f.plot('GDB13S_vs_USPTO_TMAP', template='smiles')
```

## Building the k-NN Graph

Once you have the Jaccard similarities between pairs of molecules, **TMAP** constructs a **k-nearest neighbor (k-NN) graph**. Here's how this works:

- **Nodes**: Each molecule (or MinHash signature) becomes a **node** in the graph. In your case:
    
    - Molecule A's node is represented by its MinHash signature `[4, 2, 2]`. [To see how  MinHash signature is calculated see Example Step by Step ]
    - Molecule C's node is represented by its MinHash signature `[1, 2, 1]`.
- **Edges**: Edges are created between nodes (molecules) based on their **similarity**. The similarity in this case is the **Jaccard similarity**, which you've calculated as `0.33` between molecules A and C.
    
    - The edge between A and C would be weighted by this similarity (or, equivalently, by the **distance**, which is `1 - Jaccard similarity = 1 - 0.33 = 0.67`).
- **k-Nearest Neighbors**: TMAP typically looks for the **k-nearest neighbors** for each node. If you set `k = 10`, the algorithm would search for the **10 most similar molecules** for each molecule based on their Jaccard similarity.
    
    - In practice, the LSH Forest enables fast approximate nearest neighbor searches, so we avoid comparing every molecule with every other molecule

## Constructing the Minimum Spanning Tree (MST)

After constructing the k-NN graph, TMAP builds a **Minimum Spanning Tree (MST)** from this graph. Here’s what happens:

- The **MST** is a subgraph that connects all the nodes in the k-NN graph while minimizing the total edge weight. The edge weight represents the **distance** (which is `1 - Jaccard similarity`).
- The MST ensures that all molecules (nodes) are connected in a way that minimizes the overall distance while preserving the **topological structure** of the data.

For our example:

- Molecule A and Molecule C would be connected in the MST if they are part of the minimal spanning structure that captures the overall layout.
- The edge between A and C would be assigned the distance of `0.67`, representing their dissimilarity.

The MST ensures that even distant points remain connected through indirect relationships, helping to visualize the **global structure** of the data.

## Generating the Layout in 2D Space

Once the MST is constructed, the next step is to generate a **2D layout** for visualization.

Here’s how this works:

- TMAP uses a **force-directed layout algorithm** to place the nodes (molecules) in 2D space.
    - **Repulsion**: Each node repels all other nodes, like electric charges repelling each other. This ensures that nodes are not all clustered in one spot.
    - **Attraction**: The edges between nodes (based on their similarity) act like **springs**. If two nodes are highly similar (high Jaccard similarity, low distance), the "spring" pulls them closer together. If they are dissimilar, they are pushed farther apart.
- As the algorithm iterates, the nodes settle into positions where:
    - Highly similar molecules (those with higher Jaccard similarity) end up **closer together** in the layout.
    - Dissimilar molecules are positioned **further apart**.

In our example:

- Molecule A and Molecule C will be placed in 2D space based on their similarity (or distance). Since they have a Jaccard similarity of `0.33` (distance of `0.67`), they will be placed **somewhat far apart** in the final layout.

## Visualizing the Data

After the layout is computed, the results can be visualized using tools like **Faerun**. The visualization consists of:

- **Nodes**: Each molecule (data point) is represented by a node in the 2D space.
    
    - Nodes can be **colored** based on different attributes (e.g., chemical class, activity level).
        - `x` and `y` are the coordinates of each molecule in the 2D layout.
        - `c` represents the class or group that the molecule belongs to (used for color coding).
        - `labels` contain metadata like the molecule's SMILES string or ChEMBL ID.
- **Edges**: The MST is also visualized to show how the data points are connected.
    
    - The edges reflect the relationships between molecules based on their similarity (distance).
    - In your code:
        - This adds the tree (MST) to the visualization, showing connections between molecules.
- **Interactivity**: Users can zoom, pan, and hover over nodes to see more information about each molecule. This helps explore clusters and relationships in the dataset.
    

## Example: How Molecule A and Molecule C Appear in the Visualization

- **Molecule A** and **Molecule C** will be represented by nodes in the 2D layout. Since they have a **Jaccard similarity of 0.33**, their nodes will not be too close but not too far apart either.
- If Molecule A and C are part of the same chemical class, they might have similar neighbors, forming a **cluster**. If they are from different classes, they could be positioned in different regions of the layout.
- The **MST** may show an edge connecting them if they are part of the global spanning tree that minimizes the total distance.

## 5. Exploring the Structure of the Data

With the layout and visualization complete, you can explore the **clusters** and **patterns** in your data:

- **Clusters**: Groups of molecules that are **close together** in the layout represent clusters of highly similar molecules. These might represent molecules with similar chemical properties or activities.
- **Outliers**: Molecules that are positioned **far away** from others could be outliers or represent unique chemical structures.
- **Relationships**: The **edges** in the MST show important relationships between molecules, allowing you to see how different parts of the dataset are connected.
