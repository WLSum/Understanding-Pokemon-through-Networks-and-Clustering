# Understanding-Pokemon-through-Networks-and-Clustering

<div align="center">
  <img src="https://github.com/user-attachments/assets/3c771106-701c-4827-b09d-e223da134118" height="300" width="700">
</div>

## Introduction
This project aims to explore the type dynamics and character profiles within the Pokémon world. Each Pokémon type has specific advantages and disadvantages in the battle, making type interactions a crucial factor in competitive gameplay. Additionally, various attributes such as biometric attributes, battle stats and non-combat attributes of the Pokémon contribute to each Pokémon's unique characteristics, influencing their strategic roles in the game. 🎮

<div align="center">
  <img src="https://github.com/user-attachments/assets/44f1dda3-bd75-44b6-a170-a0fb48711fe6" >
</div>

This project involves two major analyses using network analysis and clustering:
1. Network Analysis on Pokémon Type Interactions:
  - 🎯Objective:  
    - To analyse interactions between different Pokémon types based on their attacking and defending capabilities, and to identify influential types in the attacking and defending scenarios.
  - ⚙️Method:
    1. Construct network graphs for attack and defense interactions among primary Pokémon types.
    2. Calculate PageRank and topic-specific (Personalized) PageRank, to measure the influence of each Pokémon type, highlighting which types are the most influential among the attacking and defending scenarios.
2. Clustering Pokémon Based on Characteristics:
  - 🎯Objective:
    - To discover natural groupings among Pokémon based on their attributes and to interpret these clusters in terms of type, battle strength, catchability and other features.
  - ⚙️Method:
    1. Standardize the features, then use Singular Value Decomposition (SVD) to reduce the dimensionality of the feature space.
    2. Use clustering algorithms such as K-means and agglomerative hierarchical clustering and evaluate the models using the Silhouette score to compare clustering quality.
    3. Interpret the clusters based on the features to understand the natural groupings and their implications.

These analyses provide insights into the effectiveness of Pokémon types and the nature of their characteristics, enhancing our understanding of battle strategy and character roles within the Pokémon world.

## Dataset
The complete Pokémon dataset is obtained from [Kaggle](https://www.kaggle.com/datasets/rounakbanik/pokemon/data), which contains comprehensive information on 802 Pokémon, including base stats, performance against other types, biometric data, etc. The information was scraped from [Serebii.net](https://serebii.net/) and any missing data was filled manually based on information from the same source.

The dataset covers 802 Pokémon and includes:
  - 🧬 Basic Info: name, type1, type2, against_?
  - 📏 Biometric: height_m, weight_kg
  - 🎮 Combat Stats: hp, attack, defense, sp_attack, sp_defense, speed
  - 🐣 Non-Combat: capture_rate, base_egg_steps, experience_growth, base_happiness
  - 🏆 Other: is_legendary

## Snippet of Key Results 

#### 🕸️ Network Analysis on Pokémon Type Interactions
<div align="center">
  <img src="https://github.com/user-attachments/assets/efd6e0ab-36e6-4343-be20-9ea81c795217" height=300 width=300>
</div>

We analyse type interactions where the against_? value is greater than 1 in the attacking scenario, indicating that the attacking type causes more damage to the receiving type. In the network graph above, each node represents a Pokémon type, and the directed edges represent the effective **attack** relationships. For example, an edge from Fire 🔥 to Water 💧 indicates that 🔥 type is effectively attacked by💧type, which mean it causes more damage than usual to 🔥 type when battle against 💧 type. The node size is proportional to the weight, which represents the distribution of Pokémon in each type. Larger nodes correspond to more common types, while smaller nodes correspond to rarer types. 

We measure the interaction and importance of different Pokémon types based on the behaviour in the attack-by network graph. This includes using metrics such as:
- **number of in-links** whih measure the number of types a Pokémon type can effectively attack, 
- **number of out-links** which measure the number of types a Pokémon type can effectively attacked by, 
- **PageRank score** which measures the importance of a type within the attack, where a high PageRank indicates that the type is influential in the attacking network, where it is capable of attacking more types or attacking other influential types,
- **topic-specific (personalized) PageRank** which incorporates the weight of each Pokémon type when measuring PageRank.

The network analysis suggest that when crafting a Pokémon team, incorporating high-ranking types from each network ensures broad coverage and resilience. For example, Ground, Steel, and Water types provide a versatile team that can maximizing flexibility in different battle situations. Meanwhile, although types like Ghost and Dark have lower PageRank scores, they can add strategic depth with their unique interactions, covering niche matchups that are not easily addressed by more common types.

#### 🧬 Clustering Pokémon Based on Characteristics

<div align="center">
  <img src="https://github.com/user-attachments/assets/9b4df794-b210-40d0-89f9-a4618d95f062" height=300 width=400>
</div>

In the Pokémon world, there are many features in addition to the type of the Pokémon that determines the unique characteristics of a Pokémon. To uncover natural groupings of the Pokémon, we will consider the core features to cluster the Pokémon. The core features considered include biometric attributes such as height and weight, non-combat attributes such as capture rate, base egg steps, experience growth and base happiness and battle stats such as HP, attack, defense, special attack, special defense and speed.

The figure shows a scatterplot with 801 Pokémon plotted on the first and second components of the SVD reduced standardized data in 2 dimensions. The K-Means clustering separated the Pokémon into 3 clusters. The elbow method is used to determine the optimal number of clusters (k)  and the model’s performance is measured by the within-cluster sum of squares (WCSS), also known as inertia, which measures how closely the data points within a cluster are grouped around the centroid. Silhouette score is also used to compare the clustering quality across the different sets by measuring how similar each data point is to its own cluster compared to other clusters.

Further analysis on the clustering result is done and it suggests that Pokémon are naturally grouped into 3 clusters. 
- **Cluster 0**: Common, middle to strong Pokémon
- **Cluster 1**: Common, weaker Pokémon
- **Cluster 2**: Legendary, rare, powerful Pokémon

This clustering provides insight into how Pokémon can be grouped by their characteristics and battle potential. However, there are also some boundary confusions, where certain Pokémon near the cluster boundary are not clearly distinguishable by the clustering model, indicating possible limitations in separating groups with subtle differences.

## Conclusion 
The two analyses conducted have provided valuable insights into the Pokémon world in terms of their type dynamics and character profiles. 
- 🕸️ Networks analysis reveal influential types in offensive and defensive roles using PageRank, providing information on the strategic interactions between Pokémon types. 
- 🧬 Clustering shows natural groupings based on features like stats, height/weight, and rarity, aligning with the idea that Pokémon possess varied battle strengths and characteristics which influence their roles in battle.
- 🎮 These insights can influence strategic decisions in forming a Pokémon team and also planning in the battle.


👉 More details on the analysis and result in the full report: [report.md](report.md)
