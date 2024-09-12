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
The full dataset and code can be found [here](https://github.com/afloresep/fused_target_tmap/blob/master/alex_dataset.csv)

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

## Categorical Variables Distribution

### Top 5 Categories for Each Categorical Variable

1. Target_ID:
   [Distribution of top 5 Target_IDs]

2. target_name:
   [Distribution of top 5 target names]

3. standard_relation:
   [Distribution of standard relations]

4. target_protein_class:
   [Distribution of top 5 protein classes]

5. Target_Taxonomy, Target_organism, Target_type:
   (Each has only one category, representing 100% of the data)

## Numerical Variables Analysis

### Standard Value

- Mean: [Mean value]
- Median: [Median value]
- Standard Deviation: [Standard deviation]
- Minimum: [Minimum value]
- Maximum: [Maximum value]

![Distribution of Standard Value](standard_value_distribution.png)

Observations:
- [Comments on the distribution of standard_value]

## Relationships and Correlations

### Standard Value by Target Type

![Standard Value by Target Type](standard_value_by_target_type.png)

Observations:
- [Comments on the relationship between standard_value and Target_type]

## Top 10 Target_IDs by Frequency

[List of top 10 Target_IDs and their frequencies]

## Conclusions and Recommendations

1. The dataset is focused on a small number of specific targets (5 unique Target_IDs), which suggests a specialized study or screening campaign.

2. There's a significant amount of missing data in the `target_protein_class` column. Further investigation into why this data is missing and its potential impact on analysis is recommended.

3. All entries share the same standard_type, standard_units, Target_Taxonomy, Target_organism, and Target_type. This uniformity might be beneficial for comparative analyses but limits the diversity of the dataset.

4. The `standard_value` shows a wide range of values. Further analysis of this distribution in relation to different targets or compound properties could yield interesting insights.

5. Consider exploring the relationship between the chemical structures (represented by SMILES) and the standard_value. This might require additional cheminformatics tools to extract meaningful features from the SMILES strings.

6. Given the focused nature of the dataset, it might be valuable to compare these results with similar datasets targeting different organisms or protein classes to identify any unique characteristics of these specific targets.

7. For future data collection, efforts to complete the `target_protein_class` information could enhance the dataset's value for protein-related analyses.

This report provides an initial overview of the dataset. Depending on the specific research questions or goals, further in-depth analyses might be necessary.
