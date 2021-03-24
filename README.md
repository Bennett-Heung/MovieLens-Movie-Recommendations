# MovieLens-Movie-Recommendations
Provide movie recommendations using MovieLens data. 

# File Structure 

The **'01_Data_Preparation'** notebook involves: 
* Setting up installations 
* Importing a collection of libraries across all notebooks
* Load data 
* Clean data for other notebooks. 

Notebooks for exploratory data analysis (EDA): 
* **'02_EDA1'**: notebook containing first part of the exploratory data analysis (EDA)
* **'03_EDA2'**: notebook containing second part of the exploratory data analysis (EDA)

All notebooks follow up from the data preparation notebook and require to run this notebook. These include feature engineering and building recommendation systems based on a selection of data, as suggested by their notebook names: 
* **'04_Baseline'**: baseline recommendation system based on the top movies across a selection of genres and overall ratings  
* **'05_Genres'**: content-based filtering recommendation system based on movie genres from the movies dataset 
* **'06_Ratings'**: collaborative filtering recommendation system based on ratings from the ratings dataset
* **'07_Genome'**: content-based filtering recommendation system based on ratings from both Genome datasets
* **'08_Tags'**: collaborative filtering recommendation system based on ratings from the tags dataset.

Appendices are at the end of each notebook for markdown tables as reference. 


# Purpose

There are so many movies and online information about them to help viewers decide what to watch. With an overwhelming amount of information, it can be challenging for people to decide and watch a movie that they would enjoy. Recommendation systems are developed to help resolve this issue by providing movie recommendations. 

The following notebooks encode various recommendation systems using information provided by [MovieLens](http://movielens.org). Firstly, a baseline recommendation system is built on the basis of top movies from a selection of genres and their overall ratings. This recommendation system can be effective to recommend movies, although it is not designed to tailor recommendations from a user's preferences and tastes. Thus, content-based (genres and Genome tags) and collaborative (user ratings and user tags) filtering recommendation systems were developed in the concept to cater more specific movie recommendations to each viewer. Note that there are pros (for example, more tailored to a user's preferences than the baseline) and cons (for example, sparse matrices) of each type of recommendation system, and with the amount of information here, play as a key factor of the quality in movie recommendations for a viewer. 


# Quick summary about the data
The raw data was not uploaded to Github following the Usage License quote: "The user may not redistribute the data without separate permission."

The dataset describes 5-star rating and free-text tagging activity from [MovieLens](http://movielens.org), a movie recommendation service. It contains 27753444 ratings and 1108997 tag applications across 58098 movies. These data were created by 283228 users between January 09, 1995 and September 26, 2018. This dataset was generated on September 26, 2018.

The following five data files from this dataset were used for the movie recommendations: 
* `genome-scores.csv`: from Tag Genome, which is a data structure containing tag relevance scores for movies. There are 1,128 tags (tagId), with a relevance tag value (relevance) between 0 and 1 in terms of relative relevance of each tag for each movie by ID (movieId). 
* `genome-tags.csv`: from Tag Genome, which only includes references to the tag descriptions (tag) in correspondence to the tag IDs (tagId) in `genome-scores.csv`. 
* `movies.csv`: contains movie IDs (movieId), movie titles (title) and movie genres (genres) for each movie. 
* `ratings.csv`: contains ratings (rating) for each movie (movieId) each user (userId) has provided, as well as timestamps (timestamp) on the time ratings were made. 
* `tags.csv`: contains all tags (tag) for each movie (movieId) each user (userId) has provided, as well as timestamps (timestamp) on the time each tag was made.


# Data Cleaning
The following process the clean data are from the **'01_Data_Preparation'** notebook. The data cleaning process is to prepare the feature engineering and building of each recommendations system. 

* Duplicate movie titles in `movies.csv` were identified under different movie IDs. Movie IDs of these duplicates were updated to match the unique movie ID across the other data files, and the movie ID of these duplicates in `movies.csv` were removed. 
* Found not all movies in `movies.csv` were found in the other datasets - no actions were taken, rather this as an observation that some recommendation systems made here do not consider the full list of movies in `movies.csv` due to their reliance on the other data files.
* Title namnes in `movies.csv`  contained year numbers in them so a separate column was created to separate movie titles and the movie release year. 
* Some movies have missing tags in `tags.csv`, which were replaced with ''. 
* Timestamps in `ratings.csv` and `tags.csv` were dropped as they were not found to be relevant for the next steps. 


# Exploratory Data Analysis (EDA)
The EDA is focused on the `movies.csv`, `ratings.csv` and `genome-scores.csv` data.

![1EDA_genre_count](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/1EDA_genre_count.png)

**Finding 1**
Drama and Comedy are the most common genres found, followed by Thriller, Romance and Action.

![2EDA_year_count](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/2EDA_year_count.png)

**Finding 2**
The number of movies have increased at an increasing rate over time. Note that the last dip is due to the incomplete year of 2018 in the data.

![3EDA_genre_year_count](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/3EDA_genre_year_count.png)

**Finding 3**
As well as the abundance and growth in Drama and Comedy movies, there has also been significant growth of Thriller and Documentary movies in the last 30 years.

![4EDA_year_genre](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/4EDA_year_genre.png)

**Finding 4**
The boxplot of each genre's distribution of ratings show higher median ratings for Drama, Crime, War, Mystery, Animation, IMAX, Film-Noir and Documentary movies, while Horror and (no genres listed) show a more likely tendency of lower ratings. The average ratings, as shown by the bar plot of average ratings for each genre, reflect these findings also.

![5EDA_rating_genre](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/5EDA_rating_genre.png)

**Finding 5**
Film-Noir and War movies have the highest average rating; and Horror and '(no genres listed)' genres have the lowest average rating.

![6EDA_rating_year_genre](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/6EDA_rating_year_genre.png)

**Finding 6**
* An upward trend in average ratings is noticed for movies released over the earlier half the twentieth century, and have marginally declined onwards for movies released later.

* Post 1910, there are more noticeable average ratings across genres falling under the overall average ratings, particularly between 1920 and 1960. Post 1960, average ratings across genres have converged with the marginal overall decline in average ratings.

![7EDA_genre_genome_corr](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/7EDA_genre_genome_corr.png)

|    | genre              | most_corr_genre    |   corr_value |
|---:|:-------------------|:-------------------|-------------:|
|  0 | (no genres listed) | Drama              |     0.964824 |
|  1 | Action             | Adventure          |     0.945753 |
|  2 | Adventure          | Fantasy            |     0.961199 |
|  3 | Animation          | Children           |     0.939709 |
|  4 | Children           | Animation          |     0.939709 |
|  5 | Comedy             | Romance            |     0.938233 |
|  6 | Crime              | Thriller           |     0.959181 |
|  7 | Documentary        | Drama              |     0.917415 |
|  8 | Drama              | (no genres listed) |     0.964824 |
|  9 | Fantasy            | Adventure          |     0.961199 |
| 10 | Film-Noir          | Crime              |     0.873139 |
| 11 | Horror             | Thriller           |     0.887839 |
| 12 | IMAX               | Adventure          |     0.942178 |
| 13 | Musical            | Comedy             |     0.885201 |
| 14 | Mystery            | Thriller           |     0.96821  |
| 15 | Romance            | Drama              |     0.946158 |
| 16 | Sci-Fi             | Adventure          |     0.900086 |
| 17 | Thriller           | Mystery            |     0.96821  |
| 18 | War                | Drama              |     0.868225 |
| 19 | Western            | Drama              |     0.874093 |

**Finding 7**
* The most correlated genre pairs (in order) are: Mystery and Thriller, Drama and (no genre listed), Fantasy and Adventure, and Crime and Thriller.
* The most correlated genres are Drama, Adventure and Thriller.

![8EDA_genre_relevance](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/8EDA_genre_relevance.png)

**Finding 8**
Relevances do not vary significantly across each genre - relevance of tags are approximately spread evenly across genres.

![9EDA_relevance_rank](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/9EDA_relevance_rank.png)

**Finding 9**
The level of relevance for higher ranks of a tag is significantly higher than tags in the lower end of relevance, which are likely to not provide much information about movies.


# Baseline Recommendations 
The (content-based) baseline recommendation system returns the most and highly rated movies for each genre, with the option to set a release year range on the movie recommendations. 

The feature engineering process involved: 
* Collecting the total number of ratings and average rating of each movie in `ratings.csv` 
* Converting movie year of release (year) from string into numbers 
* Sorting the number of ratings, followed by each movie's average rating for the dataframe of recommendations. 

Two examples of the baseline recommendation results are below. 

## Example 1. Top 15 recommendations for all genres based on the number of ratings and average rating for each movie.  

|      |   movieId | title                                                                   | genres                                                      |   year |   no_of_ratings |   avg_ratings |
|-----:|----------:|:------------------------------------------------------------------------|:------------------------------------------------------------|-------:|----------------:|--------------:|
|  315 |       318 | Shawshank Redemption, The                                               | ['Crime', 'Drama']                                          |   1994 |           97999 |       4.42419 |
|  352 |       356 | Forrest Gump                                                            | ['Comedy', 'Drama', 'Romance', 'War']                       |   1994 |           97040 |       4.05658 |
|  293 |       296 | Pulp Fiction                                                            | ['Comedy', 'Crime', 'Drama', 'Thriller']                    |   1994 |           92406 |       4.17397 |
|  587 |       593 | Silence of the Lambs, The                                               | ['Crime', 'Horror', 'Thriller']                             |   1991 |           87899 |       4.15141 |
| 2487 |      2571 | Matrix, The                                                             | ['Action', 'Sci-Fi', 'Thriller']                            |   1999 |           84545 |       4.1497  |
|  257 |       260 | Star Wars: Episode IV - A New Hope                                      | ['Action', 'Adventure', 'Sci-Fi']                           |   1977 |           81815 |       4.12045 |
|  476 |       480 | Jurassic Park                                                           | ['Action', 'Adventure', 'Sci-Fi', 'Thriller']               |   1993 |           76451 |       3.66503 |
|  523 |       527 | Schindler's List                                                        | ['Drama', 'War']                                            |   1993 |           71516 |       4.2575  |
|  108 |       110 | Braveheart                                                              | ['Action', 'Drama', 'War']                                  |   1995 |           68803 |       4.00848 |
|    0 |         1 | Toy Story                                                               | ['Adventure', 'Animation', 'Children', 'Comedy', 'Fantasy'] |   1995 |           68469 |       3.88665 |
| 1184 |      1210 | Star Wars: Episode VI - Return of the Jedi                              | ['Action', 'Adventure', 'Sci-Fi']                           |   1983 |           66023 |       3.98604 |
| 1171 |      1196 | Star Wars: Episode V - The Empire Strikes Back                          | ['Action', 'Adventure', 'Sci-Fi']                           |   1980 |           65822 |       4.13348 |
| 2874 |      2959 | Fight Club                                                              | ['Action', 'Crime', 'Drama', 'Thriller']                    |   1999 |           65678 |       4.23066 |
|  583 |       589 | Terminator 2: Judgment Day                                              | ['Action', 'Sci-Fi']                                        |   1991 |           64258 |       3.9415  |
| 1173 |      1198 | Raiders of the Lost Ark (Indiana Jones and the Raiders of the Lost Ark) | ['Action', 'Adventure']                                     |   1981 |           63505 |       4.12046 |

## Example 2. Top 15 comedy and/or romance films between 2005 and 2018 

|       |   movieId | title                                                                               | genres                                                               |   year |   no_of_ratings |   avg_ratings |
|------:|----------:|:------------------------------------------------------------------------------------|:---------------------------------------------------------------------|-------:|----------------:|--------------:|
| 12780 |     60069 | WALL·E                                                                              | ['Adventure', 'Animation', 'Children', 'Romance', 'Sci-Fi']          |   2008 |           28116 |       4.00734 |
| 11140 |     46578 | Little Miss Sunshine                                                                | ['Adventure', 'Comedy', 'Drama']                                     |   2006 |           20038 |       3.87342 |
| 13139 |     63082 | Slumdog Millionaire                                                                 | ['Crime', 'Drama', 'Romance']                                        |   2008 |           20006 |       3.84787 |
| 12304 |     56367 | Juno                                                                                | ['Comedy', 'Drama', 'Romance']                                       |   2007 |           19131 |       3.74999 |
| 13829 |     69122 | Hangover, The                                                                       | ['Comedy', 'Crime']                                                  |   2009 |           16717 |       3.62063 |
| 10363 |     35836 | 40-Year-Old Virgin, The                                                             | ['Comedy', 'Romance']                                                |   2005 |           15332 |       3.46181 |
| 15466 |     78499 | Toy Story 3                                                                         | ['Adventure', 'Animation', 'Children', 'Comedy', 'Fantasy', 'IMAX']  |   2010 |           14841 |       3.87009 |
| 22455 |    106782 | Wolf of Wall Street, The                                                            | ['Comedy', 'Crime', 'Drama']                                         |   2013 |           14748 |       3.86913 |
| 11686 |     51255 | Hot Fuzz                                                                            | ['Action', 'Comedy', 'Crime', 'Mystery']                             |   2007 |           14379 |       3.84696 |
| 13978 |     69844 | Harry Potter and the Half-Blood Prince                                              | ['Adventure', 'Fantasy', 'Mystery', 'Romance', 'IMAX']               |   2009 |           14115 |       3.84867 |
| 11374 |     48385 | Borat: Cultural Learnings of America for Make Benefit Glorious Nation of Kazakhstan | ['Comedy']                                                           |   2006 |           14093 |       3.35837 |
| 32274 |    134853 | Inside Out                                                                          | ['Adventure', 'Animation', 'Children', 'Comedy', 'Drama', 'Fantasy'] |   2015 |           13659 |       3.96043 |
| 18651 |     92259 | Intouchables                                                                        | ['Comedy', 'Drama']                                                  |   2011 |           13573 |       4.12753 |
| 14328 |     71535 | Zombieland                                                                          | ['Action', 'Comedy', 'Horror']                                       |   2009 |           13553 |       3.76651 |
| 10166 |     33679 | Mr. & Mrs. Smith                                                                    | ['Action', 'Adventure', 'Comedy', 'Romance']                         |   2005 |           13324 |       3.24058 |


# Recommendations based on movie genres
This content-based filtering recommendation system tailors movie recommendations with similar genres based on a user's list of movie titles, ratings and genres of movies that the user has seen. 

The feature engineering process included: 
* One-hot-encoding each genre from a list of genres for each movie in the `movies.csv` 
* Create a user profile based on the user inputs of user ratings for each movie the user has seen. 

For example, if a user has provided the following inputs: 
* 'White Chicks': 5 out of 5 rating
* 'Proposal, The': 5 out of 5 rating
* 'Mean Girls': 5 out of 5 rating 

The user profile is based on the dot product of these ratings with the one-hot-encoded genres that each movie is categorised under. 

|                    |   User_Profile |
|:-------------------|---------------:|
| Adventure          |              0 |
| Animation          |              0 |
| Children           |              0 |
| Comedy             |             15 |
| Fantasy            |              0 |
| Romance            |              5 |
| Drama              |              0 |
| Action             |              5 |
| Crime              |              5 |
| Thriller           |              0 |
| Horror             |              0 |
| Mystery            |              0 |
| Sci-Fi             |              0 |
| IMAX               |              0 |
| Documentary        |              0 |
| War                |              0 |
| Musical            |              0 |
| Western            |              0 |
| Film-Noir          |              0 |
| (no genres listed) |              0 |


The recommendation scores are weighted sum of ratings, where the movies with the highest weighted sum of ratings recommended. The weighted sum of ratings are calculated by: i) the dot product of the user profile (see example above) and the genres that are one-hot-encoded; and ii) dividing by the sum of the user profile (from the example above, 30). In detail, the **recommendation scores** for each movie are sorted from 1 - movies that are to be most recommended based on the user inputs to 0 - the movies to least recommend to the user. 

From the example above, the top 20 movies recommendations based on the user inputs (shown earlier) are below: 

|    |   movieId | title                                                        |   recommendation_scores |
|---:|----------:|:-------------------------------------------------------------|------------------------:|
|  0 |    172197 | Under New Management                                         |                       1 |
|  1 |    135893 | Rough Cut                                                    |                       1 |
|  2 |      4719 | Osmosis Jones                                                |                       1 |
|  3 |    144324 | Once Upon a Time                                             |                       1 |
|  4 |     75408 | Lupin III: Sweet Lost Night (Rupan Sansei: Sweet Lost Night) |                       1 |
|  5 |    150268 | Dilwale                                                      |                       1 |
|  6 |    117630 | Double Trouble                                               |                       1 |
|  7 |     31367 | Chase, The                                                   |                       1 |
|  8 |      3868 | Naked Gun: From the Files of Police Squad!, The              |                       1 |
|  9 |    144338 | Holiday                                                      |                       1 |
| 10 |    106138 | You May Not Kiss the Bride                                   |                       1 |
| 11 |    121370 | Happy New Year                                               |                       1 |
| 12 |    121436 | The Squeeze                                                  |                       1 |
| 13 |    138806 | The Blue Lamp                                                |                       1 |
| 14 |    153490 | The Clan - Tale of the Frogs                                 |                       1 |
| 15 |    151673 | Hustle & Heat                                                |                       1 |
| 16 |     76153 | Lupin III: First Contact (Rupan Sansei: Faasuto Kontakuto)   |                       1 |
| 17 |     53022 | Wheels on Meals (Kuai can che)                               |                       1 |
| 18 |    154542 | Too Fat Too Furious                                          |                       1 |
| 19 |    127341 | Longshot                                                     |                       1 |


# Recommendations based on Genome data 

This content-based filtering movie recommendaiton system is based on the relevance of the Genome tags assigned to each movie. Movie recommendations are given for a user providing a movie they watched, rather than a list of movies and ratings they provided. 

The movie recommendations are based on movies from the Genome data files - `genome-scores.csv` and `genome-tags.csv`. Note that not all movies are recommended as these data files make up 23% of movies in the `movies.csv` data file, which contains the full comprehensive list of movies.

The feature engineering process involved differentiating movies by a selection of top-ranked relevant tags. To determine the number of tags to consider, a similar to the elbow method on the median relevances were applied to indicate that the 20 most relevant tags were a sensible choice to characterise each movie. The figure of the top 100 relevance-ranked tags and the table of median relevances below reflect the results that determined these steps.  

![Genome_subset_tags](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/Genome_subset_tags.png)


|   relevance_rank |   median_relevance |       diff |
|-----------------:|-------------------:|-----------:|
|                1 |           0.9815   | nan        |
|                2 |           0.95625  |  -0.02525  |
|                3 |           0.929    |  -0.02725  |
|                4 |           0.90325  |  -0.02575  |
|                5 |           0.878    |  -0.02525  |
|                6 |           0.854875 |  -0.023125 |
|                7 |           0.83375  |  -0.021125 |
|                8 |           0.81325  |  -0.0205   |
|                9 |           0.794875 |  -0.018375 |
|               10 |           0.778    |  -0.016875 |
|               11 |           0.76125  |  -0.01675  |
|               12 |           0.7455   |  -0.01575  |
|               13 |           0.731625 |  -0.013875 |
|               14 |           0.71775  |  -0.013875 |
|               15 |           0.704    |  -0.01375  |
|               16 |           0.69175  |  -0.01225  |
|               17 |           0.679125 |  -0.012625 |
|               18 |           0.6675   |  -0.011625 |
|               19 |           0.65575  |  -0.01175  |
|               20 |           0.64525  |  -0.0105   |
|               21 |           0.63475  |  -0.0105   |
|               22 |           0.62525  |  -0.0095   |
|               23 |           0.61575  |  -0.0095   |
|               24 |           0.60575  |  -0.01     |
|               25 |           0.5965   |  -0.00925  |


For demonstration purposes, a snippet of the 20 most relevant tags for the first five movies in the data file are below:

|    |   movieId | title                       | tag                                                                                                                                                                                                                                                 |
|---:|----------:|:----------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  0 |         1 | Toy Story                   | toys, computer animation, pixar animation, animation, kids and family, kids, pixar, cartoon, animated, children, friendship, imdb top 250, story, adventure, childhood, great movie, light, original, unlikely friendships, disney animated feature |
|  1 |         2 | Jumanji                     | adventure, children, fantasy, kids, jungle, special effects, animals, fantasy world, family, fun movie, lions, childhood, video game, big budget, fun, based on a book, good, original, computer game, scary                                        |
|  2 |         3 | Grumpier Old Men            | sequel, good sequel, sequels, comedy, original, romance, gunfight, destiny, great, good, wedding, family, minnesota, pg-13, funny, romantic comedy, crappy sequel, very funny, fun movie, mentor                                                    |
|  3 |         4 | Waiting to Exhale           | women, chick flick, girlie movie, romantic, adultery, original, unlikely friendships, relationships, stereotypes, cheating, divorce, feel-good, touching, shallow, love, feel good movie, adaptation, based on a book, friendship, mentor           |
|  4 |         5 | Father of the Bride Part II | good sequel, sequel, sequels, pregnancy, father daughter relationship, family, comedy, feel-good, original, midlife crisis, parenthood, feel good movie, destiny, funny, touching, too short, wedding, sentimental, predictable, girlie movie       |

The recommendation system is built on assigning these tags to each movie and applying the TF-IDF Vectorizer to build a cosine similarity matrix. This matrix allocate similarity scores for each movie in the Genome data, where the recommendation system reads a movie and the most similar movies, based on the similarity scores, to recommend. 

In detail, the top 20 relevant tags for each movie are used in a count Vectorizer, rather than a TF-IDF Vectorizer which down-weights some tags. The cosine similarity matrix, which contains similarity scores between 1 and -1, where 1 is the most positive correlation and -1 is the most negative correlation, for each movie. 1,125 of the 1,128 Genome tags were involved in this process to develop a 13,176 by 13,176 cosime similarity matrix, where 13,176 movies were identified in the Genome data files. The recommendation system orders the highest cosine similarity score as the top recommendation, followed by movies that have a smaller cosine similarity score. 

For example, the top 20 recommendations for movie 'Up' are shown below. 

|       |   movieId | title                             |   similarity |
|------:|----------:|:----------------------------------|-------------:|
| 12357 |    134853 | Inside Out                        |     0.767772 |
|     0 |         1 | Toy Story                         |     0.748331 |
| 10235 |     78499 | Toy Story 3                       |     0.742857 |
| 12387 |    136016 | The Good Dinosaur                 |     0.721605 |
| 11421 |    103141 | Monsters University               |     0.714286 |
|  8742 |     50872 | Ratatouille                       |     0.713506 |
|  5611 |      6377 | Finding Nemo                      |     0.690359 |
|  4406 |      4886 | Monsters, Inc.                    |     0.676665 |
|  8455 |     45517 | Cars                              |     0.666924 |
| 10638 |     87876 | Cars 2                            |     0.666737 |
|  8214 |     40339 | Chicken Little                    |     0.665703 |
|  2809 |      3114 | Toy Story 2                       |     0.659955 |
| 12369 |    135436 | The Secret Life of Pets           |     0.657143 |
|  9884 |     71264 | Cloudy with a Chance of Meatballs |     0.652051 |
|  4499 |      4990 | Jimmy Neutron: Boy Genius         |     0.647978 |
|  2097 |      2355 | Bug's Life, A                     |     0.647952 |
| 10257 |     79091 | Despicable Me                     |     0.647952 |
|  7069 |      8907 | Shark Tale                        |     0.637059 |
| 11699 |    109425 | Dug's Special Mission             |     0.633556 |


# Recommendations based on user ratings 
This collaborative filtering recommendation system is based on finding similar users from `ratings.csv` based on user inputs that include a user's movie titles and ratings of movies seen. 

The feature engineering process involved finding the subset of users who have seen at least one of the movies from the user inputs. 

The following steps were undertaken to build this recommendation system: 
1. Calculate the similarities of users with Pearson's coefficient 
2. Select positively correlated (similar) users were found 
3. Calculate weighted ratings by multiplying each user's Pearson's coefficient (weights) with their movie ratings 
4. Calculate weighted average recommendation scores for each movie by dividing total sum of weighted ratings with the total sum of weights
5. The top recommendations were sorted with the highest weighted average recommendation scores, followed by the number of ratings. 

## Example 
The user inputs include:
* 'Breakfast Club, The', 4 out of 5 rating
*  'Finding Nemo', 5 out of 5 rating
*  'Toy Story', 4.5 out of 5 rating
*  'Up', 4.5 out of 5 rating

Weighted ratings were calculated by multiplying Pearson coefficients (weights) by ratings for each movie from each similar user (below shows the first five similar users only):


|    |   userId |   pearson_coeff |   movieId |   rating |   weighted_rating |
|---:|---------:|----------------:|----------:|---------:|------------------:|
|  0 |      134 |        0.852803 |         1 |        5 |           4.26401 |
|  1 |      134 |        0.852803 |         2 |        4 |           3.41121 |
|  2 |      134 |        0.852803 |        10 |        4 |           3.41121 |
|  3 |      134 |        0.852803 |        12 |        3 |           2.55841 |
|  4 |      134 |        0.852803 |        18 |        4 |           3.41121 |


Calcuate the weighted average recommendation score using the sum total of weights (Pearson coefficients) and sum total of weighted ratings. 
From this example, the first five similar users only are below. 


|    |   movieId |   sum_of_coefficients |   sum_of_weighted_ratings |   weighted_avg_recommendation_score |
|---:|----------:|----------------------:|--------------------------:|------------------------------------:|
|  0 |         1 |             1128.21   |                 4649.53   |                             4.12115 |
|  1 |         2 |              643.579  |                 2057.16   |                             3.19643 |
|  2 |         3 |              166.579  |                  476.101  |                             2.85811 |
|  3 |         4 |               31.6491 |                   81.6372 |                             2.57945 |
|  4 |         5 |              198.361  |                  560.094  |                             2.82361 |


The top 20 recommendations based on the example user's inputs are below. 


|    |   movieId | title                                                                    |   weighted_avg_recommendation_score |   rating |
|---:|----------:|:-------------------------------------------------------------------------|------------------------------------:|---------:|
| 14 |      1324 | Amityville: Dollhouse                                                    |                                   5 |      253 |
| 35 |     59129 | Outpost                                                                  |                                   5 |       99 |
| 64 |      1843 | Slappy and the Stinkers                                                  |                                   5 |       46 |
| 32 |    133653 | Roald Dahl's Esio Trot                                                   |                                   5 |       25 |
| 28 |    171519 | Nasha Russia: Yaytsa sudby                                               |                                   5 |       25 |
| 54 |    152579 | Regular Show: The Movie                                                  |                                   5 |       22 |
| 87 |    140441 | Anne Of Green Gables: The Continuing Story                               |                                   5 |       22 |
| 11 |    101003 | Look, Up in the Sky! The Amazing Story of Superman                       |                                   5 |       21 |
| 20 |    122292 | Merlin's Apprentice                                                      |                                   5 |       20 |
| 74 |    141982 | Mrs. Miracle                                                             |                                   5 |       18 |
| 10 |    134524 | Turtle Power: The Definitive History of the Teenage Mutant Ninja Turtles |                                   5 |       17 |
| 43 |    159779 | A Midsummer Night's Dream                                                |                                   5 |       14 |
| 65 |    179747 | Crimea                                                                   |                                   5 |       14 |
|  4 |     52427 | Welcome Back, Mr. McDonald (Rajio no jikan)                              |                                   5 |       13 |
| 72 |    175639 | Futurama: The Lost Adventure                                             |                                   5 |       12 |
| 60 |     98556 | Elevator Girl                                                            |                                   5 |       12 |
| 62 |     98275 | Octopus, The (Le poulpe)                                                 |                                   5 |       12 |
| 68 |    148478 | Monkey King: Hero Is Back                                                |                                   5 |       12 |
| 79 |    141036 | 4 Minute Mile                                                            |                                   5 |       10 |
| 24 |     57778 | Future by Design                                                         |                                   5 |        9 |


# Recommendations based on Tags 

This content-based filtering recommendation system is based on the content in the tags for each movie provided by users in `tags.csv`. These tags were not as polished as tags in the Genome data files.

The feature engineering process involved: 
* Removing punctuations 
* Detecting and translating foreign languages based on the possibility that it uncovers valuable information
* Leaving purely digit tags, as they had a chance of holding valuable information
* Removing one-character tags - they were interpreted as highly unlikely to hold valuable information 
* Names are converted into one word for building the recommendation system.

The recommendation system was built similarly to the recommendation system using the Genome tags:  
* Apply the TF-IDF Vectorizer, rather than the Count Vectorizer, as some tags contain words that frequently appear across a large amount of tags and contain insignifcant value. 
* Building a cosine similarity matrix from the TF-IDF Vectorizer. Note that the cosine similarity scores can be calculated directly using the dot product, given the use of the TF-IDF vectorizer. As a result, *Sklearn's linear_kernel()* was used instead of cosine_similarities() because it is faster. The cosine similarity matrix is a 45,935 by 45,935 matrix, derived from 43,623 movies in the `tags.csv` data file. 
* The recommendation system is based on inputting a movie and providing the number of top recommendations based on the most similar movies (from the tags), measured by the cosine similaity score (from the cosine similarity matrix). 

For example, the top 20 recommendations for the movie 'Dumbo' from this recommendation system is tabluated below.

|       |   movieId | title                                           |   similarity |
|------:|----------:|:------------------------------------------------|-------------:|
|  1809 |      2018 | Bambi                                           |     0.624167 |
|  1875 |      2085 | 101 Dalmatians (One Hundred and One Dalmatians) |     0.590958 |
|   348 |       364 | Lion King, The                                  |     0.543169 |
|  1870 |      2080 | Lady and the Tramp                              |     0.535034 |
|  1868 |      2078 | Jungle Book, The                                |     0.51457  |
|   572 |       596 | Pinocchio                                       |     0.506159 |
|  2788 |      3034 | Robin Hood                                      |     0.491714 |
|   590 |       616 | Aristocats, The                                 |     0.48891  |
|  1871 |      2081 | Little Mermaid, The                             |     0.484659 |
|  3028 |      3287 | Tigger Movie, The                               |     0.47875  |
|  6456 |      6889 | Brother Bear                                    |     0.476047 |
| 26346 |    126921 | The Fox and the Hound 2                         |     0.472828 |
|  3491 |      3775 | Make Mine Music                                 |     0.470414 |
| 25613 |    124356 | The Missing Lynx                                |     0.469558 |
|  6923 |      7374 | Home on the Range                               |     0.464741 |
|  3492 |      3776 | Melody Time                                     |     0.45254  |
| 32837 |    148624 | Top Cat The Movie                               |     0.4388   |
|  1880 |      2090 | Rescuers, The                                   |     0.433549 |
|  1839 |      2049 | Happiest Millionaire, The                       |     0.430089 |


# Results

**The recommendation system based on `tags.csv` provides the most appropriate results after testing**. These tags strike the most balance between having the least drawback in terms of limilations in the amount of data and tailors recommendations based on a user's inputted preference or taste (rather than a baseline set). The quality of recommendations comes at the cost of requiring a significant amount of memory to run. If running out of memory becomes an issue for deployment, recommendation systems using the **Genome Tags** and the **baseline recommendation system** are also preferable altneratives. 
