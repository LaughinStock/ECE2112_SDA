# ECE2112_SDA_Incentive
John Felipe M. Domingo
2ECE-A

# Introduction

This is a *preliminary comprehensive assignment* and *incentive* that deals with understanding and analyzing 
the dataset "Most Streamed Spotify Songs 2023." At the end of the assignment, the student is
expected to explore, analyze, visualize, and interpret the data to extract meaningful insights.
Done by an undergraduate student of Electronics Engineering at the University of Santo Tomas.

## Links, Files, and Libraries used

The link for the 2023 Spotify dataset is attached below:

https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023/data

This Programming Assignment deals with the use of previous libraries:
- Numpy
- Pandas
- Matplotlib
- Seaborn
In order to provide and investigate summary statistics, visualizations, correlations, and
insights and recommendations regarding the different metrics found in the music scene.

# Objectives

1. Overview of the Dataset
2. Basic Descriptive Statistics
3. Finding the **Top _performer_** and the **Highest _Streams._**
4. Analyzing the music **Trends** within 2023.
5. Examining different **Music _Characteristics_** and their **correlation** to **_streams_**
6. Performing Advanced Analysis among the tracks

# Extraction of the dataset and importing the _".csv"_ file into Jupyter Notebook

Before starting any analysis, first download the dataset from the link attached in #introcution 
and then under the *download* tab, download the dataset as *.zip* and then extract the file named
"*spotify-2023.csv*" into your folder for Jupyter Notebook.

![image](https://github.com/user-attachments/assets/b899080e-77c1-48e1-a712-f0ea5232c2a5)

![image](https://github.com/user-attachments/assets/900f607f-aaca-4e97-a1a7-7af03ee4538e)

After extracting the ".*csv*" file, head into your Jupyter Notebook and import the four main libraries
mentioned in #links,-files,-and-libraries-used. After which, we can start with the analysis of the dataset.

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

# 1. Overview of the Dataset

To start with this objective, first import the file, **spotify-2023.csv** into the Jupyter Notebook
using *"pd.read_csv"*. At first you are greeted with the warning, "UnicodeDecodeError: '*utf-8*' can't decode bytes in position ..."

![image](https://github.com/user-attachments/assets/4320f383-ce4e-48b3-9f0f-5bc392567f7f)

This error message occurs when trying to read a file that containts *non-UTF-8* encoded characters. **UTF-8** is an encoding system 
for *unicode*, a matching unique binary string (of 0s and 1s). When a file is not saved in UTF-* encoding, some characters may cause issues during reading
In order to fix this issue, extend the code by inputting **encode = 'ISO-8859-1'** since this file might've been saved in that specific encoding, 
just to ensure that **non-UTF-8 characters** are **interpreted** correctly.

```
df = pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1')
df
```

## How many rows and columns does the dataset contain?

Skipping into the lower portion of the dataframe, the dataset contains 953 rows and 24 columns.

![image](https://github.com/user-attachments/assets/478bd79c-d5eb-499c-841d-c5c6e2858b34)


## What are the datatypes of each column? Are there any missing values?

Each column corresponds to a datatype, since there are 24 columns, there are 24 different datatypes found in the dataset.
The datatypes found in order are:
1. track_name
2. artist(s)_name
3. artist_count
4. released_year
5. released_month
6. released_day
7. in_spotify_playlists
8. in_spotify_charts
9. streams
10. in_apple_playlists
11. ..
12. ..
13. ..
14. ..
15. bpm
16. key
17. mode
18. danceability_%
19. valence_%
20. energy_%
21. acousticness_%
22. instrumentalness_%
23. liveness_%
24. speechiness_%

But one of the major findings from the overview of the dataset is that there are **indeed** missing values. This is mainly due to the sheer size of the dataset
itself and the notebook wouldn't be able to display each track and it's corresponding information. **_Column-wise_**, the datatype numbers from 11 to 14 are missing 
and **_Row-wise_**, tracks number 5 to 947 are missing from the provided table in Jupyter Notebook. 

![image](https://github.com/user-attachments/assets/f419d290-334a-4702-ba70-fee6dc1dfe9a)

