# Understanding-Pokemon-through-Networks-and-Clustering

<div align="center">
  <img src="https://github.com/user-attachments/assets/3c771106-701c-4827-b09d-e223da134118" height="300" width="700">
</div>

## Introduction
This project aims to explore the type dynamics and character profiles within the Pokémon world. Each Pokémon type has specific advantages and disadvantages in the battle, making type interactions a crucial factor in competitive gameplay. Additionally, various attributes such as biometric attributes, battle stats and non-combat attributes of the Pokémon contribute to each Pokémon's unique characteristics, influencing their strategic roles in the game.

<div align="center">
  <img src="https://github.com/user-attachments/assets/44f1dda3-bd75-44b6-a170-a0fb48711fe6" >
</div>

This project involves two major analyses using network analysis and clustering:
1. Network Analysis on Pokémon Type Interactions:
  - Objective:  
    - To analyse interactions between different Pokémon types based on their attacking and defending capabilities, and to identify influential types in the attacking and defending scenarios.
  - Method:
    1. Construct network graphs for attack and defense interactions among primary Pokémon types.
    2. Calculate PageRank and topic-specific (Personalized) PageRank, to measure the influence of each Pokémon type, highlighting which types are the most influential among the attacking and defending scenarios.
2. Clustering Pokémon Based on Characteristics:
  - Objective:
    - To discover natural groupings among Pokémon based on their attributes and to interpret these clusters in terms of type, battle strength, catchability and other features.
  - Method:
    1. Standardize the features, then use Singular Value Decomposition (SVD) to reduce the dimensionality of the feature space.
    2. Use clustering algorithms such as K-means and agglomerative hierarchical clustering and evaluate the models using the Silhouette score to compare clustering quality.
    3. Interpret the clusters based on the features to understand the natural groupings and their implications.

These analyses provide insights into the effectiveness of Pokémon types and the nature of their characteristics, enhancing our understanding of battle strategy and character roles within the Pokémon world.

## Dataset
The complete Pokémon dataset is obtained from [Kaggle](https://www.kaggle.com/datasets/rounakbanik/pokemon/data), which contains comprehensive information on 802 Pokémon, including base stats, performance against other types, biometric data, etc. The information was scraped from [Serebii.net](https://serebii.net/) and any missing data was filled manually based on information from the same source.

Relevant data fields in the dataset for this project includes:
1.	**pokedex_number**: Entry number of the Pokémon in the National Pokédex, indicating the unique identifier for a Pokémon.
2.	**name**: The English name of the Pokémon.
3.	**type1**: Primary type of the Pokémon.
4.	**type2**: Secondary type of the Pokémon, if the Pokémon is dual-type.
5.	**against_?**: Eighteen fields denoting the damage multiplier the Pokémon takes when attacked by the particular type. These values indicate type advantages and disadvantages.
6.	**height_m**: Height of the Pokémon, in metres. 
7.	**weight_kg**: Weight of the Pokémon, in kilograms. 
8.	**capture_rate**: Chance of catching the Pokémon, higher values indicate easier capture. 
9.	**base_egg_steps**: Number of steps required to hatch the Pokémon's egg, indicating the breeding effort needed.
10.	**experience_growth**: Rate at which the Pokémon gains experience, impacting its levelling speed. 
11.	**base_happiness**: Pokémon’s initial happiness, which can influence evolution, as well as the power of moves like Return (high happiness) and Frustration (low happiness). 
12.	**hp**: Base hit points of the Pokémon, determining how much damage it can withstand before fainting.
13.	**attack**: Base physical attack strength of the Pokémon.
14.	**defense**: Base physical defense of the Pokémon, determining resistance to physical attacks.
15.	**sp_attack**: Base special attack strength of the Pokémon.
16.	**sp_defense**: Base special defense of the Pokémon, determining resistance to special attacks.
17.	**speed**: Base speed of the Pokémon, influencing move order in battle.
18.	**is_legendary**: Denotes if the Pokémon is legendary, where legendary Pokémon are rare and generally more powerful.

### Analysis on core features

Core features include biometric attributes such as height and weight, non-combat attributes such as capture rate, base egg steps, experience growth and base happiness and battle stats such as HP, attack, defense, special attack, special defense and speed.

<div align="center">
Table 1: Descriptive statistics for biometric ad non-combat attributes.

| &nbsp; | **height_m** | **weight_kg** | **capture_  <br>rate** | **base_egg_  <br>steps** | **experience_  <br>growth** | **base_  <br>happiness** |
| --- | --- | --- | --- | --- | --- | --- |
| **mean** | 1.16 | 60.94 | 98.96 | 7191.01 | 1054996.00 | 65.36 |
| **std** | 1.07 | 108.51 | 76.41 | 6558.22 | 160255.80 | 19.60 |
| **min** | 0.1 | 0.1 | 3   | 1280 | 600000.00 | 0   |
| **25%** | 0.6 | 9   | 45  | 5120 | 1000000.00 | 70  |
| **50%** | 1   | 27.3 | 60  | 5120 | 1000000.00 | 70  |
| **75%** | 1.5 | 63  | 170 | 6400 | 1059860.00 | 70  |
| **max** | 14.5 | 999.9 | 255 | 30720 | 1640000.00 | 140 |
</div>

Table 1 shows significant variability in height and weight, where the average height and weight of a Pokémon are 1.16m and 60.94 kg, respectively. However, the height can range from 0.1m to 14.5m and the weight can range from 0.1kg to 999.9kg, suggesting a broad diversity among Pokémon species and across evolution. The high mean for capture rate indicates that most Pokémon are of relatively easier to catch, as the average is influenced by the common Pokémon that are the majority. Conversely, the high base egg steps and experience growth imply that rarer Pokémon often demand more time and effort to breed and train.

<div align="center">
Table 2: Descriptive statistics for battle stats.

| &nbsp; | **hp** | **attack** | **defense** | **sp_attack** | **sp_defense** | **speed** |
| --- | --- | --- | --- | --- | --- | --- |
| **mean** | 68.96 | 77.86 | 73.01 | 71.31 | 70.91 | 66.33 |
| **std** | 26.58 | 32.16 | 30.77 | 32.35 | 27.94 | 28.91 |
| **min** | 1   | 5   | 5   | 10  | 20  | 5   |
| **25%** | 50  | 55  | 50  | 45  | 50  | 45  |
| **50%** | 65  | 75  | 70  | 65  | 66  | 65  |
| **75%** | 80  | 100 | 90  | 91  | 90  | 85  |
| **max** | 255 | 185 | 230 | 194 | 230 | 180 |
</div>

Table 2 shows that most Pokémon generally possess relatively balance battle stats, with the majority of the Pokémon have the battle stats lies within 1 standard deviation from the mean. However, the presence of extreme values suggests that there are Pokémon that are either exceptionally weak or exceptionally strong in specific combat abilities.

<div align="center">
  Figure 1: Correlation heatmap of core features.
  <img src="https://github.com/user-attachments/assets/bdb7d015-974b-40e8-8f7e-cb7270692b96" >
</div>

The correlation heatmap in Figure 1 shows that height and weight are strongly correlated, aligning with the expectation that larger Pokémon tend to weigh more. Height and weight also show a moderately positive relationship with hit points (HP), indicating larger Pokémon tend to have higher HP, allowing them to withstand more damage during battles. There is a positive correlation between attack and defense stats, suggesting that Pokémon with stronger attack capabilities also tend to have better defense. This indicates the tendency for Pokémon to evolve in a balanced manner, rather than specializing solely as attackers or defenders. Notably, the capture rate has a negative correlation with base stats such as HP, attack, and defense, implying that Pokémon that are harder to catch typically possess stronger combat abilities, highlighting the rarity and power of such Pokémon. However, base egg steps and base happiness shows moderately negative correlation, suggesting that Pokémon which require more breeding effort may have lower initial happiness, which may be the case for rare Pokémon such that more time and effort need to be invested to improve Pokémon's happiness to fully realize their potential.

### Analysis on Pokémon type
<div align="center">
  Figure 2: Distribution of single-type and dual-type of Pokémon.
  <img src="https://github.com/user-attachments/assets/0709de9f-3004-441c-b17d-49576047ab70" >
</div>

Pokémon can have either a single type or two distinct types, with 18 possible types in total. In dual-type Pokémon, the order of types does not matter, meaning a Grass/Poison type Pokémon is considered the same as a Poison/Grass type Pokémon. Single-type and dual-type Pokémon are nearly equally represented in the Pokémon world, with a slight majority of dual-type Pokémon at 51.2%, as shown in Figure 2.

<div align="center">
  Figure 3: Number of Pokémon in each primary type.
  <img src="https://github.com/user-attachments/assets/f1fdaedb-d8a8-4d33-a416-1d9437eca2cd" >
</div>

While the order of types has no impact on a Pokémon’s type effectiveness, we will assess the dual-type Pokémon according to the order documented in the National Pokédex, where National Pokédex records information on all known Pokémon. As shown in Figure 3, the most common primary types are Water and Normal, while the least common primary type is Flying.

<div align="center">
  Figure 4: Heatmap of secondary type for each primary type.
  <img src="https://github.com/user-attachments/assets/331ddf23-0a98-412f-931f-c40d8b8f760a" >
</div>

The heatmap in Figure 4 shows that Flying is the most common secondary type, especially when paired with Normal as the primary type. This suggests that Flying types more frequently appear in dual-type Pokémon, explaining why they are less common as a primary type. Conversely, it is relatively rare for Pokémon to have Normal or Bug as a secondary type. Additionally, Pokémon with Water and Bug as their primary types are more likely to have a secondary type, likely due to the overall higher frequency of Pokémon in these categories.

## Network Analysis on Pokémon Type Interactions
A Pokémon's type has a direct impact on its battle performance, influencing both the damage it causes and the damage it receives when interacting with other types. Dual-type Pokémon have combined advantages and disadvantages which bring complexity to this analysis of type interactions. Therefore, for simplicity, we will focus on the type interactions among single-type Pokémon, where the strengths and weaknesses of each type are straightforward.

We will analyse the interactions among the different Pokémon types in both attack and defense scenarios using the against_? variable, which represents the damage multipliers taken by a Pokémon when attacked by a specific type. For example, against_dragon indicates the damage a Pokémon sustains from Dragon-type attacks. The variables are interpreted as follows:

  - Value < 1: Less damage than usual, suggesting the Pokémon’s type defends well against that particular type.
  - Value = 1: Normal damage, suggesting neutral interaction where the type neither effectively defends nor is particularly vulnerable to that particular type.
  - Value > 1: More damage than usual, suggesting the type is more vulnerable to attack from that particular type. Conversely, this suggests an attacking ability for the attacking type.
    
This variable is used to identify which types excel in attack or defense scenarios, uncovering the natural strengths and weaknesses of each type. We consider types that cause more damage than usual as the effective attacking types, while types that cause less damage than usual as the effective defending types.


