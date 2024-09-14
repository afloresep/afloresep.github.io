---
title: "Fused Fingerprints TMAP"
excerpt: "Visualization of large ChemBL dataset merging MAPC fingerprints for every entry with same Target ID<br/><img src='/images/nice_label_tmap.png'>"
collection: portfolio
---

This project aims to provide a comprehensive visualization of the most active compounds for a diverse array of targets sourced from the ChemBL database.

#### Objectives: 
* Visualization of Top Compounds: Identify and visualize the top 5 most active compounds for approximately 15,000 distinct biological targets.
* Fingerprint Merging and Minhashing: For each target, take 5 compounds and calculate their fingerprint using MAP*. These fingerprints, which encapsulate the chemical characteristics of each compound, are then merged to create a unified representation (merged_fp). Subsequently, the merged fingerprints are subjected to minhashing techniques to reduce dimensionality and facilitate efficient comparison (i.e. calculating Jaccard distances).
* Run TMAP



## 1. Data Analysis of the Dataframe

The dataset contains information about chemical compounds, their targets, and various properties. It consists of 38,370 entries with 11 columns, including both categorical and numerical data. Data was selected to have the 5 most active compounds (`canonical_smiles`) for every `Target_ID` in the ChemBL database.
The full dataset and code can be found [here](https://github.com/afloresep/fused_target_tmap/blob/master/dataset.csv)

### Basic Information

- Total entries: 38,370
- Number of features: 11

### Data Types and Missing Values

| Column                | Data Type | Non-Null Count | Missing Values |
|-----------------------|-----------|----------------|----------------|
| canonical_smiles      | object    | 38370          | 0              |
| Target_ID             | object    | 38370          | 0              |
| target_name           | object    | 38370          | 0              |
| standard_type         | object    | 38370          | 0              |
| standard_relation     | object    | 38370          | 0              |
| standard_value        | float64   | 38370          | 0              |
| standard_units        | object    | 38370          | 0              |
| target_protein_class  | object    | 24710          | 13660          |
| Target_Taxonomy       | object    | 38370          | 0              |
| Target_organism       | object    | 38370          | 0              |
| Target_type           | object    | 38370          | 0              |

### Unique Value Counts

| Column                | Unique Values |
|-----------------------|---------------|
| canonical_smiles      | 25860         |
| Target_ID             | 7016          |
| target_name           | 5697          |
| standard_type         | 46            |
| standard_relation     | 3             |
| standard_value        | 6678          |
| standard_units        | 2             |
| target_protein_class  | 68            |
| Target_Taxonomy       | 60            |
| Target_organism       | 928           |
| Target_type           | 20            |

Observations:
- Not every `canonical_smiles` is unique, suggesting there are compounds that bind several Targets. 
- There are 7016 Targets.
- The dataset covers a single Target_Taxonomy, Target_organism, and Target_type.
- We will have to do reduce the unique values in some of the categories for the TMAP as color coding is not a good idea for <50 unique 
- The numerical values like the IC50, Ki or Kd cannot be directly compared. We will use this when we display the merged compounds we will take the most active one of the 5 and show its IC50, molecule etc. 
### First Run using MAPC fingerprints
Running a First TMAP using MAP* fingerprints. Before fusing the fingerprints I want to make sure everything runs normally and visualize the TMAP with all the data available to then change necessary parameters.

Code [here](https://github.com/afloresep/fused_target_tmap)
1. Import data
2. Calculate fingerprints
3. Run TMAP which includes: 
	- LSH Indexing and coordinates generation 
	- Coordinates and Layout configuration
	- Plotting using Faerun

After a few trials and readjusting the node_size etc we have this: 

<img src='/images/first_tmap.png'>

### Merging the Fingerprints
The fingerprint is basically a vector of n-dimensions in a special type of format used in TMAP called 'VectorUint'. Think of it as a numpy array. To merge the fingerprint one just have to select the minimum value on each index for all the fingerprints with same target ID. The resulting vector will have a combination of all the vectors -fingerprints- from the different compounds and therefore will enconde the most relevant features. 

In order to perform the minimum value operation provided by numpy we have to convert the tmap VectorUint object into a numpy array. This is simply done by using `np.array()` function. So we do that:


df_processed = df.loc[valid_indices].copy()
df_processed['fingerprint_vector'] = pd.Series(fingerprints, index=valid_indices)
df_processed['fingerprint_vector'] = df_processed['fingerprint_vector'].apply(np.array)
