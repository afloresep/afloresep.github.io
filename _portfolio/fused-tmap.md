---
title: "Fused Fingerprints TMAP"
excerpt: "Visualization of large ChemBL dataset merging MAPC fingerprints for every entry with same Target ID<br/><img src='/images/500x300.png'>"
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

Code at 
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

In order to perform the minimum value operation provided by numpy we have to convert the tmap VectorUint object into a numpy array. This is simply done by using np.array() function. So we do that:

```
df_processed = df.loc[valid_indices].copy()
df_processed['fingerprint_vector'] = pd.Series(fingerprints, index=valid_indices)
df_processed['fingerprint_vector'] = df_processed['fingerprint_vector'].apply(np.array)
```
Now we calculate the resulting vector: 

```
result = df_processed.groupby('Target_ID').agg({
        'fingerprint_vector': lambda x: np.min(np.vstack(x), axis=0).tolist(),
        **{col: 'first' for col in df_processed.columns if col not in ['fingerprint_vector', 'Target_ID']}
    }).reset_index(
```
> You can find a more in-depth explanation of this piece of code down the page

In summary:
- The code groups the data by `Target_ID`.
- For each group, it computes the element-wise minimum of the `fingerprint_vector` column (to combine fingerprints for the same target).
- For all other columns, it simply keeps the first value in each group.
- The result is a new DataFrame with one row per `Target_ID` and a combined (minimized) fingerprint for each target.

**Now we re-run TMAP: **

<img src='/images/fused_tmap.png'>

Well, it worked!
However, we still have to work on the label data as there isn't much information we can retrieve out of this. 

For that we will change the amount of unique values. 
`df['target_protein_class'].unique()` returns 68 different values so we will reduce it to ~10 by using a new function that simply takes all the words an searches for more global values such as 'enzyme', 'transporter' or 'membrane receptor'
```
def map_protein_class(value):

    if pd.isna(value):
        return np.nan

    value = value.lower().strip()

    if 'enzyme' in value:
        if 'kinase' in value:
            return 'kinase'
        elif 'protease' in value:
            return 'protease'
        elif 'cytochrome p450' in value:
            return 'cytochrome p450'
        else:
            return 'enzyme'
    elif 'ion channel' in value:
        return 'ion channel'
    elif 'transporter' in value:
        return 'transporter'
    elif 'transcription factor' in value:
        return 'transcription factor'
    elif 'membrane receptor' in value:
        return 'membrane receptor'
    elif 'epigenetic regulator' in value:
        return 'epigenetic regulator'
    else:
        return 'other'  # Default category for unmatched classes
```

Fixing this and other labels with a similar function, we re-run the code: 

<img src='/images/nice_label_tmap.png'>
That's more like it. 


### The fusing explained: 
This piece of code is aggregating data in a pandas DataFrame `df_processed` based on the values in the `Target_ID` column. 

1. **`df_processed.groupby('Target_ID')`**:
- This groups the DataFrame `df_processed` by the values in the `Target_ID` column. Grouping means all rows that have the same `Target_ID` will be treated as a single group for subsequent operations.

2. **`.agg({ ... })`**:

- The `.agg()` method allows you to specify how each column within each group should be aggregated (i.e., combined or summarized).
- In this case, a dictionary is passed that specifies how to aggregate two different types of columns:
    1. The `fingerprint_vector` column.
    2. All other columns except `fingerprint_vector` and `Target_ID`.

 3. **`'fingerprint_vector': lambda x: np.min(np.vstack(x), axis=0).tolist()`**:
- This specifies how the `fingerprint_vector` column should be aggregated.
- `x` is a list of fingerprint vectors for each group (where each `Target_ID` may have multiple associated fingerprints).
- `np.vstack(x)` stacks the individual vectors in `x` vertically to form a 2D array, where each fingerprint vector becomes a row in that array.
- `np.min(..., axis=0)` computes the minimum value for each element across the stacked fingerprint vectors column-wise, i.e., it finds the minimum value for each position in the fingerprint vectors.
- `.tolist()` converts the result back into a Python list.
- The result is a single fingerprint vector per `Target_ID`, where each element is the minimum value from the corresponding positions in the group's vectors.

 4. **`**{col: 'first' for col in df_processed.columns if col not in ['fingerprint_vector', 'Target_ID']}`**:
- This handles the aggregation of all other columns (excluding `fingerprint_vector` and `Target_ID`).
- A dictionary comprehension is used to specify that for each other column, the `'first'` value in the group should be taken. This means that for all non-fingerprint columns, only the first value in the group will be retained, and all other values will be discarded.
- This is common when columns contain metadata that doesn't change across rows for the same `Target_ID`.
 
5. **`.reset_index()`**:
- After the grouping and aggregation are complete, the `.reset_index()` method is used to convert the grouped `Target_ID` back into a regular column in the resulting DataFrame (as opposed to being part of the DataFrame’s index).
- This ensures that the output is a clean DataFrame where `Target_ID` is a standard column, not an index.


