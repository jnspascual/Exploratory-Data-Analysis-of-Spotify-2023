# Exploratory-Data-Analysis-of-Spotify-2023
Pascual, Jasmine Nicole S.

2ECE-B

In this activity, exploratory data analysis (EDA) was performed on a dataset containing popular tracks on the Most Streamed Songs in 2023. This task aims to analyze, visualize, and interpret the data to extract meaningful insights.

# Access the Dataset through this link 

https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023

# About the Dataset

1. Overview of the Dataset
   * How many rows and columns does that dataset contain?
   * What are the data types of each column? Are there any missing values?

2. Basic Descriptive Statistics
   * What are the mean, median, and standard deviation of the streams column?
   * What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?
     
3. Top Performers
   * Which track has the highest number of streams? Display the top 5 most streamed tracks.
   * Who are the top 5 most frequent artists based on the number of tracks in the dataset?
    
4. Temporal Trends
   * Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
   * Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?
    
5. Genre and Music Characteristics
   * Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
   * Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

6. Platform Popularity
    * How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?
      
7. Advanced Analysis
   * Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
   * Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.
     
# Codes and Analysis
1. In order to access the dataset, download the file through the website
2. Upload the file in the same folder as your Jupyter notebook to load the dataframe. You will see the downloaded file named as "spotify-2023".
3. If there is an instance where you couldn't load the file, open the file and save it as "CSV UTF-8". You will be able to open and load the file in your Jupyter notebook.

## Access the libraries

    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import seaborn as sns 

## Load the Spotify data, read as UTF-8 to handle encoding

    spotify_data = pd.read_csv('spotify-2023.csv', encoding='utf-8')

## Overview of Dataset 

    print("Dataset Dimensions:", spotify_data.shape)
    print("\nData Types and Missing Values:\n")
    print(spotify_data.info())
    print("\nMissing Values per Column:\n", spotify_data.isnull().sum())

## Basic Descriptive Statistics

##### Converting 'streams' column to numeric, ignoring non-numeric values

    spotify_data['streams'] = pd.to_numeric(spotify_data['streams'], errors='coerce')

##### Calculating mean, median, and standard deviation for 'streams' column

    mean_streams = spotify_data['streams'].mean()
    median_streams = spotify_data['streams'].median()
    std_dev_streams = spotify_data['streams'].std()

##### Displaying the calculated statistics

    print("Streams Column - Basic Statistics:")
    print(f"Mean Value: {mean_streams}")
    print(f"Median Value: {median_streams}")
    print(f"Standard Deviation: {std_dev_streams}")

##### Distribution of 'released_year' and 'artist_count'

    plt.figure(figsize=(14, 6))
    sns.histplot(spotify_data['released_year'], bins=15, kde=True, color='teal')
    plt.title("Distribution of Track Release Year")
    plt.xlabel("Release Year")
    plt.ylabel("Frequency")
    plt.show()

## Top Performers

##### Top Performers

    top_track = spotify_data.sort_values(by='streams', ascending=False).iloc[0]
    print("\nTrack with Highest Streams:", top_track[['track_name', 'streams']])
    
    top_tracks = spotify_data.sort_values(by='streams', ascending=False).head(5)
    print("\nTop 5 Most Streamed Tracks:\n", top_tracks[['track_name', 'streams']])
    
    top_artists = spotify_data['artist(s)_name'].value_counts().head(5)
    print("\nTop 5 Most Frequent Artists:\n", top_artists)


## Temporal Trends

##### Temporal Trends

    tracks_by_year = spotify_data['released_year'].value_counts().sort_index().reset_index()
    tracks_by_year.columns = ['Year', 'Number_of_Tracks']
    plt.figure(figsize=(14, 6))
    sns.lineplot(data=tracks_by_year, x='Year', y='Number_of_Tracks', marker='o', color='purple')
    plt.title("Tracks Released per Year")
    plt.xlabel("Year")
    plt.ylabel("Number of Tracks")
    plt.show()

##### Plotting distribution of 'released_year' with red bars

    sns.histplot(spotify_data['released_year'], kde=False, color='#FF5733')

##### Setting title and labels

    plt.title('Yearly Release Distribution')
    plt.xlabel('Year of Release')
    plt.ylabel('Track Count')
    plt.show()

##### Plotting distribution of 'artist_count' with blue bars

    sns.histplot(spotify_data['artist_count'], kde=False, color='steelblue')

##### Adding title and axis labels
    plt.title('Artist Track Distribution')
    plt.xlabel('Number of Artists')
    plt.ylabel('Frequency')
    plt.show()

##### Scatter plot to show relationship between 'released_year' and 'artist_count'

    plt.figure(figsize=(7, 7))
    sns.scatterplot(data=spotify_data, x='released_year', y='artist_count', color='darkgreen')

##### Scatter plot title and labels
    plt.title('Year vs Artist Count')
    plt.xlabel('Release Year')
    plt.ylabel('Artist Count')
    plt.show()

##### Monthly Release Patterns - Assuming 'released_month' is provided as a separate column
    if 'released_month' in spotify_data.columns:
        monthly_counts = spotify_data['released_month'].value_counts().sort_index()
        print("\nTrack Counts by Month:\n", monthly_counts)

  ##### Bar plot to show monthly release patterns
        plt.figure(figsize=(10, 5))
        sns.barplot(x=monthly_counts.index, y=monthly_counts.values, color='coral')
        plt.xlabel("Month")
        plt.ylabel("Track Count")
        plt.title("Tracks Released by Month")
        plt.show()
    else:
        print("Monthly release data not available.")


## Genre and Music Characteristics

##### Genre and Music Characteristics
    correlation_data = spotify_data[['streams', 'bpm', 'danceability_%', 'energy_%', 'valence_%', 'acousticness_%']].apply(pd.to_numeric, errors='coerce')
    correlation_matrix = correlation_data.corr()
    print("\nCorrelation of Streams with Music Attributes:\n", correlation_matrix['streams'].sort_values(ascending=False))

##### Correlation between selected attributes
    dance_energy_corr = correlation_data[['danceability_%', 'energy_%']].corr()
    valence_acoustic_corr = correlation_data[['valence_%', 'acousticness_%']].corr()

    print("\nCorrelation between Danceability and Energy:\n", dance_energy_corr)
    print("\nCorrelation between Valence and Acousticness:\n", valence_acoustic_corr)

## Platform Popularity

##### Summing track counts across each platform column, ignoring non-numeric values
    spotify_track_total = spotify_data['in_spotify_playlists'].apply(pd.to_numeric, errors='coerce').sum()
    deezer_track_total = spotify_data['in_deezer_playlists'].apply(pd.to_numeric, errors='coerce').sum()
    apple_track_total = spotify_data['in_apple_playlists'].apply(pd.to_numeric, errors='coerce').sum()

##### Displaying the results
    print("Track Counts per Platform:")
    print(f"Total tracks in Spotify playlists: {spotify_track_total}")
    print(f"Total tracks in Deezer playlists: {deezer_track_total}")
    print(f"Total tracks in Apple playlists: {apple_track_total}")


## Advanced Analysis

##### Advanced Analysis

##### Track patterns by key and mode
    key_mode_pattern = spotify_data.groupby(['key', 'mode'])['streams'].mean().unstack()
    print("\nAverage Streams by Key and Mode:\n", key_mode_pattern)

##### Genre or artist playlist appearance frequency
    artist_playlist_counts = spotify_data.groupby('artist(s)_name')[['in_spotify_playlists', 'in_apple_playlists']].sum()
    top_playlist_artists = artist_playlist_counts.sort_values(by=['in_spotify_playlists', 'in_apple_playlists'], ascending=False).head(5)
    print("\nArtists with Highest Playlist Appearances:\n", top_playlist_artists)

# Insights and Reflection

From my understanding of the patterns and trends that was shown from the visualizations and personal liking for music, I believe that what makes a track popular is how the listener relates to the lyrics of the song as well as the catchiness of the vocals, lyrics and instrumentals. 

While doing this activity, it made me realize how helpful Python is in interpreting, analyzing, and visualizing data. I admit that this task was quite hard for me to finish because I still need improvement in my understanding and application of the lessons about Python. This activity actually helped me in reflecting on how much I need to improve in programming because I am planning to pursue the language for my career. 

# References:

1. https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023
2. ChatGPT.com
