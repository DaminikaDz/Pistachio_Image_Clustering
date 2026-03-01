# Pistachio Type Clustering from Images

This project investigates unsupervised clustering of pistachio varieties (Siirt and Kirmizi) based on features extracted from images.

Two types of feature representations were analyzed:

1. Precomputed geometric and color features  
2. Deep embeddings extracted using ResNet and Vision Transformer (ViT)

The goal was to evaluate how well different feature spaces enable separation of pistachio types using clustering algorithms.

### Clustering Algorithms

- KMeans  
- DBSCAN  
- HDBSCAN  

### Feature Spaces

- Geometric + color features  
- PCA-reduced features  
- t-SNE projections  
- ResNet embeddings  
- ViT embeddings  

### Evaluation Metrics

- Silhouette Score  
- Calinski–Harabasz Index  
- Davies–Bouldin Index  
- Distribution of true pistachio labels within clusters  

## Results

### Classical Features

Best result: **KMeans (k = 2) after PCA**

- Silhouette: 0.5186  
- Calinski–Harabasz: 3286.13  
- Davies–Bouldin: 0.6572  
- Cluster sizes: 1121, 1027  
- Noise: 0  

### t-SNE Features

Best result: **KMeans (k = 4)**

- Silhouette: 0.4437  
- Calinski–Harabasz: 2443.89  
- Davies–Bouldin: 0.7785  
- Cluster sizes: 594, 576, 497, 481  
- Noise: 0  

### Full Embeddings

KMeans (k = 2) produced mixed clusters, but  
k = 4 showed partial separation of pistachio types.

### ResNet Embeddings

Best separation: **KMeans (k = 6)**  
Clusters 0, 1, 5 → mostly Kirmizi  
Other clusters → mostly Siirt  

### ViT Embeddings

Best separation: **KMeans (k = 4)**  
Clusters 0, 1, 3 → mostly Kirmizi  
Clusters 2, 4 → mostly Siirt  

### DBSCAN and HDBSCAN

Across all feature versions:

- many points labeled as noise  
- or one large cluster with small fragments  
- parameter tuning (eps, min_samples) did not produce stable clustering  

## Conclusions

- KMeans consistently achieved the best clustering quality  
- Deep embeddings (ResNet, ViT) enabled better separation than classical features  
- Density-based methods (DBSCAN, HDBSCAN) were ineffective for this dataset  

## Project Structure

notebooks/
- eda.ipynb
- feature_engineering.ipynb
- modeling

validation/
- validation.ipynb

## Data

The project uses two sources of pistachio data:

- `Pistachio_28_Features_Dataset` – tabular dataset containing geometric and color features extracted from pistachio images  
- `Pistachio_Image_Dataset.zip` – original pistachio images used to compute deep embeddings (ResNet, ViT)

The image dataset is provided as a compressed archive due to its size.

## Independent Validation: Jellyfish Species Clustering

As an additional study, we independently validated another project involving clustering of image-derived features from six jellyfish species.

The original work reported only Agglomerative Clustering results. We extended the analysis by evaluating KMeans and DBSCAN to determine which clustering approach performs best on the dataset.

### KMeans

Best configuration: **k = 4**

- Silhouette Score: 0.0470  
- Calinski–Harabasz Index: 188.18  
- Davies–Bouldin Index: 3.9362  
- Noise: 0  
- Cluster sizes: 1841, 1336, 976, 741  

Despite the low Silhouette value, KMeans achieved better overall data separation than other methods.

### DBSCAN

Parameters: **eps = 16, min_samples = 6**

- Number of clusters: 8 (one dominant)  
- Silhouette Score: 0.0438  
- Davies–Bouldin Index: 1.8243  
- Noise points: 220  

Cluster distribution:

- Cluster 0: 4598  
- Remaining clusters: very small (7–18 samples)

DBSCAN assigned most samples to a single cluster, indicating parameter mismatch or overly dense data structure.

### Agglomerative Clustering

To evaluate separability under reduced class complexity, the model was applied to a subset of three out of six species (n_clusters = 3).

Cluster distribution:

- Cluster 0: 1030  
- Cluster 1: 1289  
- Cluster 2: 137  

Cluster 0 contained 93% of one specific species, indicating good separation for that class. However, overall cluster balance was poor.

### Validation Conclusions

- KMeans (k = 4) produced the most stable and evenly distributed clusters, despite low metric values  
- DBSCAN performed poorly, assigning most data to one cluster  
- Agglomerative Clustering separated one species well in the reduced-class setting but produced highly imbalanced clusters  

This validation confirms that centroid-based clustering is more suitable than density-based or hierarchical methods for this dataset.

## How to run

Open notebooks in Jupyter and execute cells sequentially.
