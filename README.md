<h1><center> Recipe for a SMASH hit? </center></h1> 

As the **Hamilton Brothers** say, *Music makes the world go around*. Music is the driving force that can set a mood, put someone in the mood , and even make someone want to dance. But what is it about music that has the ability to do this? In particular what is it about "Popular Music" that it can transcend through generations and can touch people from all differnet backgrounds.  

I was very intrigued by this thought and decided to do research of my own to find out what features do popular music have in common, and is there a recipe for a Smash hit? In order to achiece this, I decided to collect songs from the *Record Industry Association of America*(RIAA) who is in charge of apointing certifications to 'Singles' & 'Albums' based on certain qualifications. From these songs I used each artist and collected all albums produced, and from the albums I acquired all tracks, audio features , and genres for each track. From this data I could compared the difference between a song that won an award and songs that did not. In order to investigate which features wer most important when determining if a song won a RIAA award. In order to do this I created machine learning models that would be able to predict this. 

I created three models, Logistic Regression, Random Forest Classifier, & Naive Bayes Gaussian. From these models the **Random Forest** had the best score  with .849 ROC AUC score, and  will be my production model. From this model I looked at my feature importance for track featuers and genres. My most important track featuers were respectively. After recieving these features I did analysis on them and how they changed throughout the years, which you can learn more by reading a more detailed summary **below**. 

## Data Collection
### RIAA Web Scraping
#### Who is the RIAA?

> The RIAA (Record Industry Association of America) is a trade organization that has a big influence in the recording Industry and apoints certifications to 'Singles' & 'Albums' based on certain qualifications.

#### Sales
 - Gold: 500,000 units
 - Platinum: 1,000,000 units
 - Multi-Platinum: 2,000,000 units (increments of 1,000,000 thereafter)
 - Diamond: 10,000,000 units

I collected about 4,000 songs that went at least Gold and their respective artist from the years 2000 - 2018

### SpoitPY
> SpotiPY is a Wrapper for the spotify web API that uses python in order to scrape from the Spotify web API

From my RIAA track & artist , I used the artist name for collecting their individual Artist ID's & from there I could collect All of their album names. I want to collect all albums so that I can analyze each track in the album and compare the songs that won awards to the ones that did not. I thought by collecting other songs from the same artist will emphasize the distinct featuers in the audio content that contribute to an Award Winning song. After collecting every track I collected the following audio features from Spotify. 

*All Audio Features are calculated based on Spotify Standards*

|                                                                         Danceability                                                                         |                                                       Energy                                                       |                                                            Key                                                           |                                                                        Liveness                                                                        |                                      Acousticness                                      |                                                                                                              Duration                                                                                                             |                  Time Signature                  |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------:|
|   How suitable a track is for dancing based on a  combination of musical elements including tempo,  rhythm stability, beat strength, and overall regularity. |          Energy is a measure from 0.0 to 1.0 and  represents a perceptual measure of intensity and activity.       |                                        Pitches using standard Pitch Class notation.                                      |   Detects the presence of an audience in the recording.  Higher liveness values represent an increased probability that  the track was performed live. | Acousticness:  A confidence measure from 0.0 to 1.0 of whether  the track is acoustic. |                                                                                                    Length of the track in Seconds                                                                                                 |    How many beats are in each bar (or measure).  |

|                Instruentalness                |                                                 Loudness                                                 |                                                        Mode                                                        |                     Speechiness                    |                                Tempo                                |                                                                                                          Valence                                                                                                          |            Track Number           |
|:---------------------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------:|:-------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------------------:|
|  Predicts whether a track contains no vocals. |  The overall loudness of a track in decibels (dB).  Loudness values are averaged across the entire track |  Indicates the modality (major or minor) of a track,  the type of scale from which its melodic content is derived. |  Detects the presence of spoken words  in a track. |  The overall estimated tempo of a track in beats  per minute (BPM). |   A measure from 0.0 to 1.0 describing the musical  positiveness conveyed by a track. Tracks with high valence  sound more positive (e.g.cheerful), while tracks with low valence sound more negative (e.g. sad , angry). | Position of track on whole album  |

### Complete Data 
Once I acquired all the audio features, I merged the audio features, genres , along with track name , artist , & release date.

My final Data Frame was:
- 651 Columns x 85267 Rows

## Exploratory Data Analysis

From Exploring my data, I noticed that a significantly increase in the amount of awards being given each year from 2000-2018. 
![download](https://user-images.githubusercontent.com/41606483/47175841-ff1a3580-d2c8-11e8-9d31-3a426b5fa73e.png)

- I thought maybe that because of the Ipod, Iphone , & the vast other products that make music more avaliable was the reason for this.
- At the same time I couldn't help to think that this wasn't the case, but that music producers were beginning to figure out the recipe that would make a song popular, thus the reason for the increasing number of awards given.
*Reason 2018 is so Low is because this data was collected in October 2018 , & most awards for 2018 have no been awarded yet*

## Models

### Logistic Regression
- Logistic Regression is the perfect predicting model that will take in Continuous variables and then will classify data into descrete groups. You can have 2 or more classification groups, but in this case I am grouping into two groups, Award Winning & Not Award Winning. A Pro about Logistic Regression is the betas are easy to interpret compared to other models because they are the log of odds. Therefore, in order to properly interpret the coefficient of this model, we need to exponentiate them.

### Random Forest
- Random Forest is an ensemble learning method that is flexible and easy to use. It is one of the most used algorithms, because of its simplicity and the fact that it can be used for both classification and regression tasks. It is a meta estimator that fits a number of decision tree classifiers on various sub-samples of the dataset. It combats high variance by adding additional randomness to the model, while growing the trees. Instead of searching for the most important feature while splitting a node, it searches for the best feature among a random subset of features. This results in a wide diversity that generally results in a better model.

### Naive Bayes
- Naive Bayes model is based off of the Bayes therorm which talks about the relationship between the probability of an event happening & the observations of that same event happening previously in recorded data. 
 ## Model Evaluation
 
##### Recall 
> The fraction of relevant instances that have been retrieved over the total amount of relevant instances.
 
##### Precision
> The fraction of relevant instances among the retrieved instances

 |         Model            | Recall | Precision | ROC AUC Test Score |
|---------------------|--------|-----------|----------------|
| Logistic Regression | 4.77%  | 59.2%     | .841         |
| Random Forest       | 5.51%  | 56.5%     | .849           |
| Naive Bayes Guassian  | 97.4%    | 4.31%    | .571           |



## Analysis
###  Most Important Track Featuers

| Feature:           | Track Number  | Followers | Loudness | Duration | Danceability | Liveness | Accousticness |
|--------------------|---------------|-----------|----------|----------|--------------|----------|---------------|
| Award Average: | 4.77          | 4,373,117 | -5.742   | 232.3    | 0.6243       | 0.1858   | 0.1546        |
| Non Award Average:     | 8.61          | 1,880,963 | -7.632   | 230      | 0.5739       | 0.2484   | 0.2325        |
| Difference (abs):    | 3.84         | 2,492,154 | 1.89   | 2.3     | 0.0504     | 0.0626   | 0.0779       |

- This table shows the differnece in the averages of the features from years 2000-2018
- Looking at the Graphs in the Analysis notebook can give a better picture as to the many convergences of non award features to those of award winning features. Which may insinuate one of my hypothesis that the reason for the increasing number of Awards is due to people figuring out how to make popular music.


### Genre
- Extensive overall increase in tracks being produced in thewe Genres
- Pop & Rap being the larget genres along with the biggest growth

![download-1](https://user-images.githubusercontent.com/41606483/47180167-c2543b80-d2d4-11e8-8a3b-0cf4bef61431.png)




# Executive Summary



My Random Forest Model, which is my production model, had the best roc auc aocre with a very average recall and precision score. For this model, it was shown that the following track features, *track number, followers, loudness, duration, danceability, liveness,and  accousticness* were the most important in identify award winning songs; and *pop ,dirty south rap, post-teen pop, contemporary country , dance pop , rap , and gangster rap* were the most important genres in determining award winning songs. Although these audio features are made by spotify and the general public cannot be directly access the actual musical substances. I belive that optimizing for the following feautres should help when trying to make a smash hit. 




















