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

### Datatypes

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
11. in_apple_charts
12. in_deezer_playlists
13. in_deezer_charts
14. in_shazam_charts
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

### Missing Values

After running the code below:
```
null_counts = df.isnull().sum()
print(null_counts)
```
This code detects if there are missing values in the dataset and results show that **there are** missing values
in the data set. According to the image attaced below, the datatypes that include missing values are "_in_shazam_charts_" 
and "_key._" The probable reasons for these missing values is that:
1. The track isn't available on the platform (Shazam).
2. The track's details are not given by the artist to the platform(s).
3. The artist probably did not mind what "key" they were using.
4. The data collection process had some issues.
5. The data was lost for some reason.

![image](https://github.com/user-attachments/assets/a1a84bee-bddd-4237-9ee0-195f7d7e778e)

# 2. Basic Descriptive Statistics

This objective is more focused on the following: 
- Double-checking for the datatypes
- Finding and Coverting missing values to "NaN"
- Indexing, Slicing, and Subsetting dataframes to gather the required data.

## Checking for the datatypes 

Before starting any computation for the mean, median, and mode, sometimes there are tendencies where the numerical data
contained in the column may be *non-numeric*. Because there may be a time where the error message, "n1, n2,..., nx cannot convert 
to numeric," shows up when running the code. 
To troubleshoot, use the code below:
```
print(df["streams"].dtype)
```
In this current case, the datatype shows is "object" and it may contain non-numeric values. To convert and turn any non-numeric
values into "NaN," you can use the code:
```
df["streams"] = pd.to_numeric(df["streams"], errors="coerce")
df["streams"].fillna(df["streams"].median(), inplace=True)
```

## What are **mean, median, and mode** of the *'streams'* column?

To solve for all three at one time, use the code:
```
mean = df["streams"].mean()
median = df["streams"].median()
mode = df["streams"].mode()[0]  

print("Mean:", mean)
print("Median:", median)
print("Mode:", mode)
```
This would result in the following answer:

![image](https://github.com/user-attachments/assets/474fdaea-3846-488f-8041-6b6879c40b3d)


## What is the distribution of the released_year and artist_count?

### To solve for the distribution of the released year 
using the code below:
```
year_bins = np.arange(1940, 2028, 4)  # Decade bins from 1940 to 2023

# Group by the defined year bins
out_year = df.groupby(pd.cut(df['released_year'], bins=year_bins, right=False))['released_year'] \
    .agg(**{
        'Min': 'min', 
        'Max': 'max', 
        'Count': 'count'
    })

print("Year Release Analysis:")
print(out_year)
```
This gives the result:

![image](https://github.com/user-attachments/assets/ed45f836-fcd8-4ba6-8d23-0abe00d922cd)

There seems to be a trend in regards to which tracks were mostly played on the different streaming platforms.
Per usual, the tracks released recently (2020-2024) were streamed a lot while the older ones were not but was
included in the list for the most streamed songs in Spotify. There are a few tracks included in the list
that made it but it is believed to be the generational songs that also made it into the current generation's 
ears. The popularity for older songs is also boosted because of vines, memes, short videos (tiktok, reels, shorts)
and it makes them appreciate the older songs as well and be able to relate to their parents' music tastes.

### To solve for the distribution of the artist count, 
using the code below:
```
artist_bins = np.arange(1, 8 + 1, 1)
out_artist = df.groupby(pd.cut(df['artist_count'], artist_bins))['artist_count'] \
    .agg(**{
        'Min': 'min', 
        'Max': 'max', 
        'Count': 'count',
    })

print("Artist Count Analysis:")
print(out_artist)
```
This gives the result:

![image](https://github.com/user-attachments/assets/cee0b544-1642-43b7-a082-a301219bb8a1)

Based on the table given by Jupyter Notebook, there are quite a lot of solo and duo artists in a single track.
Tracks with 1 artist are the highest and as the number of artists featuring increases, there are fewer tracks 
that has a lot of artists. With the highest count being 8.

# 3. Top Performers

Before starting any slicing, indexing, and subsetting any data, why use a "_for_" loop? The for loop is used to **iterate and check
the items** through the top 5 most streamed tracks and the top 5 most frequent artists. By doing this, it allows the programmer to
to access each tracks' information and **output** the **correct and specific details** neatly.

## Which track has the **highest** number of *'streams'*? Display the **top 5 most** streamed *tracks*.
using the code below:
```
most_streamed_tracks = df.sort_values(by='streams', ascending = False).head(5)
print("Top 5 most streamed tracks:")
for idx, row in most_streamed_tracks.iterrows():
    print(f"Track: {row['track_name']}, Streams: {row['streams']}")
```
The results are:

![image](https://github.com/user-attachments/assets/d1be8c60-d80f-4308-9114-313fed585d47)

## Who are the **top 5** most frequent artists based on the number of tracks in the dataset?
using the code below:
```
top_5_artists = df['artist(s)_name'].value_counts().head(5)
print("Top 5 most frequent artists:")
for artist, count in top_5_artists.items():
    print(f"Artist: {artist}, Frequency: {count}")
```
The results are:

![image](https://github.com/user-attachments/assets/f4936d84-cda9-440d-bd38-68b0864633eb)

# 4. Temporal trends

Before starting analysis in regards to the number of tracks released over years and months, first know that it is needed
to group the data in terms of their release year or month and count the number of tracks released. In doing so, it would
save the time needed to analyze the data.

For annual trends:
```
# Group by release year and count the number of tracks released each year
annual_trends = df.groupby('released_year').size()  # This will give the count of tracks per year
```

For monthly trends:
```
# Group by release month and count the number of tracks released per month
monthly_trends = df.groupby('released_month').size()  # This will give the count of tracks per month
```

## Number of tracks released over time

After using the code below:
```
# Plotting the number of tracks released over time
annual_trends.plot(kind='line', title='Number of Tracks Released Over Time')
plt.xlabel('Year')
plt.ylabel('Number of Tracks Released')
plt.show()
```

We are given this graph:

![image](https://github.com/user-attachments/assets/81ba2369-a497-43f4-8799-d57eff89c04e)

Given how the nature of this dataset goes, it was expected that the recent releases were mostly topping the charts. Since this is a dataset covering
the top 2023 spotify songs, the older songs were shadowed by the huge spike in graph. But like the explanation from earlier in obective number 2, the reason
why some of the older songs are featured in the dataset is due to the other listeners coming from the older generations, memes, tiktoks, and etc.

## Number of tracks released per Month

After slightly editing the code earlier:
```
# Plotting the number of tracks released over time
monthly_trends.plot(kind='bar', title='Number of Tracks Released per Month')
plt.xlabel('Month')
plt.ylabel('Number of Tracks Released')
plt.xticks(rotation=0)
plt.show()
```

We are given this graph:

![image](https://github.com/user-attachments/assets/9f64ebe2-b76d-4c6a-9f4a-511fd7823aab)

Suprisingly, January tops the chart and is followed by May. A quick Google search explains why. 
Generally, the first two months of the year is a good time to release music because the market isn't that much
saturated compared to the releases within the year. And to the consumer's perspective, it would make sense since 
after the exciting end to the year, people tend to look at the newer and hottest hits that would greatly symbolize their
start of the year. 

For May, its simply explained as "It's summer! We got time to entertain ourselves!" And the average consumer would listen to hundreds and
maybe even thousands of music to indulge themselves after their exhausting assignments or workloads at school or at work.

# 5. Genre and Music characteristics

To start with this obective, lets go straight to the heat map itself. If the map is hard to read in the "readme" file, its probably best for the 
reader to go straight to the code itself in Jupyter Notebook. Do note that positive (red/warm) values correlate well while negative values (blue/cold)
do not correlate that much.

The used code for this obective:
```
# Select columns for correlation, including streams and musical characteristics
music_characteristics = df[['streams', 'bpm', 'danceability_%', 'energy_%', 'acousticness_%', 'instrumentalness_%', 'liveness_%', 'speechiness_%']] 

# Compute the correlation matrix
correlation_matrix = music_characteristics.corr()

# Display the correlation matrix
plt.figure(figsize=(9, 6))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', center=0)
plt.title("Correlation between Music Characteristics")
plt.show()
```

![image](https://github.com/user-attachments/assets/9aee87b3-c776-4b56-ab29-32b1bee6bcde)

Why put every characteristics to the map? It's also good to see the different music characteristics correlate with each other to not only the 
streams, but also to each other.

## Examine the correlation... 
between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?

For the comparison between bpm, danceability_%, and energy_%, the energy_% influences streams the most since at heart, people tend to 
listen to music to feel something maybe because of their emotional and physical reactions, maybe because of their social connections, rhythmic influence,
and maybe to stimulate themselves. And sometimes the energy music brings to them gives them these feelings.

As it is because of that, it influences the streams the most since it has the closest value to zero and it is followed by acousticness_% and intrumentalness_%.

# 6. Platform Popularity (temporary)

## How do the numbers in spotify_playlists, spotify_charts, and apple_playlists compare?

## Which platform seems to favor the most popular tracks?

# 7. Advanced Analysis
