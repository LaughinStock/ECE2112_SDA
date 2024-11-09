# ECE2112_SDA
**John Felipe M. Domingo
2ECE-A**

# Introduction

This is a *preliminary comprehensive assignment* that deals with understanding and analyzing 
the dataset "Most Streamed Spotify Songs 2023." At the end of the assignment, the student is
expected to explore, analyze, visualize, and interpret the data to extract meaningful insights.

Done by an undergraduate student of Electronics Engineering at the University of Santo Tomas.

## Links, Files, and Libraries used

The link for the 2023 Spotify dataset is attached below:

https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023/data

This Programming Assignment deals with the use of previous libraries:
- Numpy (np)
- Pandas (pd)
- Matplotlib (plt)
- Seaborn (sns)
  
In order to provide and investigate summary statistics, visualizations, correlations, and
insights and recommendations regarding the different metrics found in the music scene.

# Objectives

1. Overview of the Dataset
2. Basic Descriptive Statistics
3. Finding the **Top _performer_** and the **Highest _Streams._**
4. Analyzing the music **Trends** within 2023.
5. Examining different **Music _Characteristics_** and their **correlation** to _streams._
6. Checking the **Popularity** between different _streaming platforms_.
7. Performing **Advanced Analysis** among the tracks.

# Extraction of the dataset and importing the _".csv"_ file into Jupyter Notebook

Before starting any analysis, first **download the dataset** from the link attached in #introcution 
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

To start with this objective, first **import** the file, *spotify-2023.csv* into the **Jupyter Notebook.**
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

Skipping into the lower portion of the dataframe, the *dataset* contains **953 rows and 24 columns.**

![image](https://github.com/user-attachments/assets/478bd79c-d5eb-499c-841d-c5c6e2858b34)


## What are the datatypes of each column? Are there any missing values?

### Datatypes

Each *column* corresponds to a **datatype**, since there are *24 columns*, there are **24** different datatypes found in the dataset.
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


#### After running the code below:

```
null_counts = df.isnull().sum()
print(null_counts)
```

#### Why this code?

This code **detects** if there are *missing values* in the dataset and results show that **there are** *missing values*
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

### To solve for all three at one time, use the code:

```
mean = df["streams"].mean()
median = df["streams"].median()
mode = df["streams"].mode()[0]  

print("Mean:", mean)
print("Median:", median)
print("Mode:", mode)
```

### This would result in the following answer:

![image](https://github.com/user-attachments/assets/474fdaea-3846-488f-8041-6b6879c40b3d)

## What is the distribution of the released_year and artist_count?

### The code used (year_release):

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

### Here are the results in a bargraph (year_release):

![image](https://github.com/user-attachments/assets/ed45f836-fcd8-4ba6-8d23-0abe00d922cd)

### Discussion of results (year_release):

There seems to be a *trend* in regards to which **tracks were mostly played** on the different streaming platforms.
Per usual, the *tracks* released **_recently_** (2020-2024) were **streamed a lot** while the **_older_** ones were not but was
included in the list for the most streamed songs in Spotify.
There are a few tracks included in the list that made it but it is believed to be the generational songs that also made it into 
the current generation's ears. The popularity for older songs is also boosted because of vines, memes, 
short videos (tiktok, reels, shorts) and it makes them appreciate the older songs as well and be able to relate 
to their parents' music tastes.

### Here is the code used (artist_count):
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

### This gives the result (artist_count):

![image](https://github.com/user-attachments/assets/cee0b544-1642-43b7-a082-a301219bb8a1)

### Discussion of results (artist_count):

Based on the table given by Jupyter Notebook, there are quite **a lot** of *solo and duo artists* in a single track.
Tracks with **1** artist were the *highest* and as the number of artists featuring *increases*, there are **fewer** tracks 
that has **a lot of artists**. With the *highest count* being **8**.

# 3. Top Performers

Before starting any slicing, indexing, and subsetting any data, why use a "_for_" loop? The for loop is used to **iterate and check
the items** through the top 5 most streamed tracks and the top 5 most frequent artists. By doing this, it allows the programmer to
to access each tracks' information and **output** the **correct and specific details** neatly.

## Which track has the **highest** number of *'streams'*? Display the **top 5 most** streamed *tracks*.
### using the code below:
```
most_streamed_tracks = df.sort_values(by='streams', ascending = False).head(5)
print("Top 5 most streamed tracks:")
for idx, row in most_streamed_tracks.iterrows():
    print(f"Track: {row['track_name']}, Streams: {row['streams']}")
```
### The results computed are:

![image](https://github.com/user-attachments/assets/d1be8c60-d80f-4308-9114-313fed585d47)

### Discussion of results:

Despite the song being released in 2020, the *track* with the **highest** number of *'streams'* is "Blinding Lights" 
from The Weeknd having 3.7 billion streams, followed by "Shape of You" by Ed Sheeran with 3.56 billion and 
"Someone You Loved" by Lewis Capaldi with 2.88 billion streams. 

## Who are the **top 5** most frequent artists based on the number of tracks in the dataset?

### using the code below:
```
top_5_artists = df['artist(s)_name'].value_counts().head(5)
print("Top 5 most frequent artists:")
for artist, count in top_5_artists.items():
    print(f"Artist: {artist}, Frequency: {count}")
```
### The results computed are:

![image](https://github.com/user-attachments/assets/f4936d84-cda9-440d-bd38-68b0864633eb)

### Discussion of results:

The top three *artist(s)* who were the **most frequent** in the data set were Taylor Swift with 34, The Weeknd with 22, and Bad Bunny with 19. 
Taylor Swift was extremely popular in 2023 due to having released and re-released albums that skyrocketed her fame and having "The Eras Tour"
concert that made her even popular. Along with her, The Weeknd also gotten pretty popular after his hit after hit releases of music. But the people 
are not only coming back just for one song, they are also looking back at his older hits to learn and listen more about hit music. Bad Bunny is a
Puerto Rican rapper that has found success after his hit song "Diles" in 2016. His charisma and music style resonated with people and that has helped 
him to gain popularity.

# 4. Temporal trends

Before starting analysis in regards to the number of tracks released over years and months, first know that it is needed
to group the data in terms of their release year or month and count the number of tracks released. In doing so, it would
save the time needed to analyze the data.

a. For annual trends:

```
# Group by release year and count the number of tracks released each year
annual_trends = df.groupby('released_year').size()  # This will give the count of tracks per year
```

### Discussion of results:

b. For monthly trends:

```
# Group by release month and count the number of tracks released per month
monthly_trends = df.groupby('released_month').size()  # This will give the count of tracks per month
```

## Number of tracks released over time

### Here is the code used:
```
# Plotting the number of tracks released over time
annual_trends.plot(kind='line', title='Number of Tracks Released Over Time')
plt.xlabel('Year')
plt.ylabel('Number of Tracks Released')
plt.show()
```

### Here are the results in a linegraph:

![image](https://github.com/user-attachments/assets/81ba2369-a497-43f4-8799-d57eff89c04e)

### Discussion of results

Given how the nature of this dataset goes, it was expected that the *recent releases* were mostly **topping** the charts. Since this is a dataset covering
the top 2023 spotify songs, the *older songs* were **shadowed** by the huge spike in graph. But like the explanation from earlier in obective number 2, the reason
why some of the older songs are featured in the dataset is due to the other listeners coming from the older generations, memes, tiktoks, and etc.

## Number of tracks released per Month

### Here is the code used:

```
# Plotting the number of tracks released over time
monthly_trends.plot(kind='bar', title='Number of Tracks Released per Month')
plt.xlabel('Month')
plt.ylabel('Number of Tracks Released')
plt.xticks(rotation=0)
plt.show()
```

### Here are the results in a bargraph:

![image](https://github.com/user-attachments/assets/9f64ebe2-b76d-4c6a-9f4a-511fd7823aab)

### Discussion of results:

Suprisingly, **January tops the chart** and is **followed by May**. A quick Google search explains why. 
Generally, the *first two months of the year* is a **good time to release music** because the market isn't that much
saturated compared to the releases within the year. And to the consumer's perspective, it would make sense since 
after the exciting end to the year, people tend to look at the newer and hottest hits that would greatly symbolize their
start of the year. 

For May, its simply explained as "It's summer! We got time to entertain ourselves!" And the average consumer would listen to hundreds and
maybe even thousands of music to indulge themselves after their exhausting assignments or workloads at school or at work.

# 5. Genre and Music characteristics

To start with this obective, lets go straight to the heat map itself. If the map is hard to read in the "readme" file, its probably best for the 
reader to go straight to the code itself in Jupyter Notebook. Do note that positive (red/warm) values correlate well while negative values (blue/cold)
do not correlate that much.

## The code used:

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

## Here are the results in a heatmap:

![image](https://github.com/user-attachments/assets/9aee87b3-c776-4b56-ab29-32b1bee6bcde)


## Examine the correlation... 

between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?

For the comparison between bpm, danceability_%, and energy_%. **Energy_% influences streams the most** since at heart, people tend to 
listen to music to feel something maybe because of their emotional and physical reactions, maybe because of their social connections, rhythmic influence,
and maybe to stimulate themselves. And sometimes the energy music brings to them gives them these feelings.

But the music characteristic that influences *streams* the **most** is **acousticness_%** since it has the closest value to zero and is followed by 
energy_%, and instrumentalness_%.

# 6. Platform Popularity 

This obective is divided into two parts, first is the *comparison between spotify_playlists, spotify_charts, and apple_playlists.* 
While the second part comprises of *every platform given in the dataset and compressed into one category to easily compare* the final values.

The codes in regards to these parts are not placed in the repository due to the length of the lines of the code probably being too much for 
the viewer. So instead, the important lines of code are presented to show how the process goes. Because in the second part, there are four different
platforms being compared and added on. The platforms and their respective sections in the dataset were:

- Spotify (Playlists and Charts)
- Apple Music (Playlists and Charts)
- Deezer (Playlists and Charts)
- Shazam (Charts only)

## How do the numbers in spotify_playlists, spotify_charts, and apple_playlists compare?

### Here is a snippet of the code:

```
# Calculate total occurrences for each platform
spotify_playlists_total = df['in_spotify_playlists'].sum()
spotify_charts_total = df['in_spotify_charts'].sum()
apple_playlists_total = df['in_apple_playlists'].sum()

# Display results in the console
print("Comparison of Track Appearances in Different Platforms:")
print(f"Spotify Playlists: {spotify_playlists_total}")
print(f"Spotify Charts: {spotify_charts_total}")
print(f"Apple Playlists: {apple_playlists_total}")

# Prepare data for plotting
platform_counts = {
    'Spotify Playlists': spotify_playlists_total,
    'Spotify Charts': spotify_charts_total,
    'Apple Playlists': apple_playlists_total
}
```

### Here are the results in a graph ("Total track apperances * 10^6):

![image](https://github.com/user-attachments/assets/4da6e2c7-b1da-4883-9e38-61eb22d47d9f)

### Discussion of results

For the three, it was heavily **dominated by spotify_playlists** followed by apple_playlists and the spotify_charts. Setting aside the dominance
of spotify_playlists, look at the comparison between charts and playlists. The track appearances are mostly tied to the consumer's playlists rather
than the weekly or monthly ranking of the platforms' new top performing songs. It generally reflects the user's use of the app since most people nowadays
would use these streaming apps to look, and download their favourite songs. Its very rare for a user to find and listen to the newer released songs in
these platoforms since they would rather stick to what is trending, popular, or comfortable in their ears. Though the platforms do generate the charts, 
the consumers would prefer to stick to their own created playlists.

## Which platform seems to favor the most popular tracks?

### Here is a snippet of the code:

```
# 1. Define Popular Tracks - Top 100 tracks by stream count
top_tracks = df.nlargest(100, 'streams')

# 2. Ensure columns are numeric and handle non-numeric values by filling them with 0
columns_to_convert = ['in_spotify_playlists', 'in_spotify_charts', 
                      'in_apple_playlists', 'in_apple_charts', 
                      'in_deezer_playlists', 'in_deezer_charts', 
                      'in_shazam_charts']

for column in columns_to_convert:
    top_tracks[column] = pd.to_numeric(top_tracks[column], errors='coerce').fillna(0).astype(int)

# 3. Calculate occurrences of top tracks in each platform
top_tracks_spotify_playlists = top_tracks['in_spotify_playlists'].sum()
top_tracks_spotify_charts = top_tracks['in_spotify_charts'].sum()
top_tracks_apple_playlists = top_tracks['in_apple_playlists'].sum()
top_tracks_apple_charts = top_tracks['in_apple_charts'].sum()
top_tracks_deezer_playlists = top_tracks['in_deezer_playlists'].sum()
top_tracks_deezer_charts = top_tracks['in_deezer_charts'].sum()
top_tracks_shazam_charts = top_tracks['in_shazam_charts'].sum()

# 4. Combine counts for playlists and charts per platform
spotify_total = top_tracks_spotify_playlists + top_tracks_spotify_charts
apple_total = top_tracks_apple_playlists + top_tracks_apple_charts
deezer_total = top_tracks_deezer_playlists + top_tracks_deezer_charts
shazam_total = top_tracks_shazam_charts  # Shazam only has charts, not playlists
```
### Here are the results in a graph:

![image](https://github.com/user-attachments/assets/42bbc168-7f33-4d5a-b020-eb802b398b05)

### Discussion of results

Its no suprise that **Spotify continued to dominate in this graph** since its currently and probably for a long time, the 
*largest and most popular* music streaming platform globally, with users reaching the hundred millions all around the world.
With Apple Music coming in second followed by Deezer and Shazam. Currently, the top two music streaming platforms are 
generally known to a wider audience but since Apple Music is limited to ios and or iMusic softwares and Deezer and Shazam not being
well-known to the average consumer and maybe even the artist, they would most likely prefer to publish or listen to songs 
that is on a well-known platform that has large numbers of users on a daily basis. 

# 7. Advanced Analysis

The final objective is also split into two parts. The first part mainly focusing on identifying on the patterns among the tracks with the same key or mode, 
and the second part is about performing an analysis about the most frequently appearing artists in playlists or charts. 

Also in this part, do note that snippets of the code are shown since these are also very long and its probably better to only show the important
parts of the code.

## Average streams by key or mode

### Here is a snippet of the code:

```
# Group by 'key' and 'mode' and calculate average streams
key_mode_streams = df.groupby(['key', 'mode'])['streams'].mean().unstack()
```

### Here is the results in a graph:

![image](https://github.com/user-attachments/assets/8c608d01-f4a5-4750-9dda-2d44851b520c)

### Discussion of results

The key findings in the graph is that the consumers would **_prefer_** to listen to songs that has an average *key* from **F to C#** and they 
would prefer to have a *mode* that is a **Major**. The reason why consumers would prefer to listen to songs that mostly have a mode of 
Major is because of its songs generally being happy while Minor modes tend to be intensive or melancholic.
For the key signatures, the consumers gravitate towards the keys that are F, E, D#, D, and C#. The reason also ties with the 
preference of songs that are generally happy, warm, and have common instruments that accompanies the vocalists very well.

## Top Artists in Playlists and Charts

### Here is a snippet of the code:

```
# Ensure playlist/chart columns are boolean or numeric (0/1) so we can sum them correctly
playlist_columns = ['in_spotify_playlists', 'in_spotify_charts', 
                    'in_apple_playlists', 'in_apple_charts', 
                    'in_deezer_playlists', 'in_deezer_charts', 
                    'in_shazam_charts']

# Convert playlist/chart columns to numeric, setting errors='coerce' to handle non-numeric values
# and filling non-numeric values (NaN) with 0
for col in playlist_columns:
    df[col] = pd.to_numeric(df[col], errors='coerce').fillna(0).astype(int)

# Summing up occurrences in playlists/charts for each artist
artist_popularity = df.groupby('artist(s)_name')[playlist_columns].sum()
artist_popularity['Total'] = artist_popularity.sum(axis=1)
top_artists = artist_popularity['Total'].sort_values(ascending=False).head(10)
```

### Here is the results in a graph:

![image](https://github.com/user-attachments/assets/1ee33ac8-4149-4b24-867c-6a6d6ddc57cc)

### Discussion of results

The *top three* is **The Weeknd, Taylor Swift, and Ed Sheeran.** 2023 was a blast for The Weeknd since he has had a lot of 
major breakthroughs in regards to his music career in 2023. He has had major hits from his newer ("Blinding Lights") and older songs and was
probably the most well-known singer in 2023. The second spot is from Taylor Swift whom in 2023 had a breakthrough in her career.
from re-releasing her older albums (1989), to having the highest-grossing concert of all time, it was no suprise that she made it to second. 
And finally completing the top three is Ed Sheeran which is suprising since lately he hasn't been releasing new albums or songs but he is still here.
Though not well-known around here in the Philippines, he has done quite a lot of concerts (large and one-to-one) that gained traction around the internet 
and that made people love the guy. 

Oh and do note that since the dataset also **includes numbers from playlists**, it is also expected that some of the **older
artists can make it into the graph.**

# 8. Comments and recommendations by the author (not a professional artist)

As much as possible that I want to compress everything into a single paragraph, I would like to use bullet points for the reccomendations
and comments because it would probably be the best option for this.

## If you are an artist that is aspiring to be big in the future, based on the analysis you should:

- It is best to release songs along the start (first two months) of the first and second quarters of the year since these are the times when people do have the time to listen to the songs and they would like to start the year or summer with a blast.
- Make a song that focuses on bringing the energy you produce from music towards the audience since this would have a great impact towards their emotions, focus on bringing the instruments that bring the acousticness in music.
- It is also best that you should publish your songs on Spotify since its the largest streaming platform currently and capitalizing on its recent rise is also a good thing.
- Focus on having a song that has fluctuating key signatures from "F" to "C#" and a mode that is "Major" as they're mostly warm, happy, and it tends to blend to the instruments and vocalists very well.

 ## If you are also doing the same dataset analysis:

 - Take extra care of the missing values within the datatypes since this would cause a lot of trouble if you're analyzing data and is not sure if the code is incorrect or the ".csv" file download and import caused issues.
 - If you're doing this for an assignment, its best to give the whole assignment a whole week since there is a lot of code, graphs, and analysis to be covered and cramming everything in one day is not recommended.
 - Research some of the trends and fact check them to confirm that the analysis was correct.

 ## Final remarks

 Some of the comments here is just opinions from the author, as everyone has their own ways in dealing data or making music. These are just the results and trends seen from the analysis and it may be useful towards the artist or programmer who is doing their passions in life. 
  
