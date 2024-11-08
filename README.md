# Exploratory-Data-Analysis-of-Spotify-2023
Pascual, Jasmine Nicole S.

2ECE-B

In this activity, exploratory data analysis (EDA) was performed on a dataset containing popular tracks on the Most Streamed Songs in 2023. This task aims to analyze, visualize, and interpret the data to extract meaningful insights.

## Access the Dataset through this link 

https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023

## About the Dataset

1. Overview of the Dataset
   * How many rows and columns do the dataset contain?
   * Are there any missing values in the dataset?
   * What are the data types of each column?
  
2. Top Performances
   * What are the top 5 most streamed tracks throughout the years?
   * Who are the top 5 most favored artist/s?

3. Trends and Patterns
   * How many tracks were released per year?

4. Musical Attributes
   * What is the correlation between the track's number of streams and musical attributes such as bpm, danceability, and energy?
   * What is the relationship of the following musical attributes: danceability, energy, valence and acousticness?

## Codes and Analysis
1. In order to access the dataset, download the file through the website
2. Upload the file in the same folder as your Jupyter notebook to load the dataframe. You will see the downloaded file named as "spotify-2023".
3. If there is an instance where you couldn't load the file, open the file and save it as "CSV UTF-8". You will be able to open and load the file in your Jupyter notebook.

Access the libraries needed to perform the program

    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import seaborn as sns 

Load the downloaded file

    spotify = pd.read_csv('spotify-2023.csv')
    spotify

Getting the number of rows and columns in the dataset

    spotify.shape

Find the missing values in the dataset

    pd.isnull(spotify).sum()

Find the data types in the dataset

    spotify.info()

Top 5 most streamed tracks throughout the years

    top_tracks = spotify.sort_values(by = 'streams', ascending = False).head()
    print("TOP 5 MOST STREAMED TRACKS:")
    top_tracks

Top 5 most favored artists

    top_artists = spotify['artist(s)_name'].value_counts().head() 
    print("TOP 5 MOST FAVORED ARTISTS:")
    top_artists

Number of tracks released per year

    tracks_peryear = spotify['released_year'].value_counts().reset_index()
    tracks_peryear.columns = ['Released_year', 'Tracks']
    print ("Number of tracks released per year")
    print (tracks_peryear)
    
    plt.figure(figsize = (14,6))
    sns.barplot(x = 'Released_year', y = 'Tracks', data = tracks_inyear, color = 'blue')
    plt.xlabel("Years")
    plt.xticks(rotation = 60)
    plt.title("Number of tracks released per year")
    plt.show()

Correlation between the track's number of streams and musical attributes

    atr = spotify[['streams', 'danceability_%', 'energy_%', 'valence_%', 'acousticness_%', 'instrumentalness_%', 'liveness_%', 'speechiness_%']].apply(pd.to_numeric, errors='coerce') 
    clean = atr.dropna() 
    cor = clean.corr()
    correl_streams = cor['streams'].sort_values(ascending = False)
    print (correl_streams)

Relationship of the following musical attributes: danceability & energy, and valence & acousticness

    dance_energy = spotify[['danceability_%', 'energy_%']].apply(pd.to_numeric, errors='coerce').corr()
    val_acous = spotify[['valence_%', 'acousticness_%']].apply(pd.to_numeric, errors='coerce').corr()
    print ('The Correlation bewteen danceability_% and energy_%:')
    print (dance_energy)
    print ('The Correlation bewteen valence_% and acousticness_%:')
    print (val_acous) 

## Insights and Reflection

From my understanding of the patterns and trends that was shown from the visualizations and personal liking for music, I believe that what makes a track popular is how the listener relates to the lyrics of the song as well as the catchiness of the vocals, lyrics and instrumentals. 

While doing this activity, it made me realize how helpful Python is in interpreting, analyzing, and visualizing data. I admit that this task was quite hard for me to finish because I still need improvement in my understanding and application of the lessons about Python. This activity actually helped me in reflecting on how much I need to improve in programming because I am planning to pursue the language for my career. 

## References:

1. https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023
2. ChatGPT.com
