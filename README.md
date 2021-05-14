# MovieLens-Movie-Recommendations
Movie recommendation systems using MovieLens data. 

# Notebook Structure 
* `01_Introduction.ipynb`- Overview of data and content structure
* `02_Initial_Setup.ipynb` - importing libraries and functions; loading dataset and data cleaning process
* `03_EDA.ipynb` - Exploratory Data Analysis (EDA); containing data visualisations
* `04_Data_Preparation.ipynb` - preparing data for modelling purposes
* `05_Modelling.ipynb` - built recommendation systems with the training set
* `06_Evaluation.ipynb` - evaluate recommendation systems models with MAP@k using the test set; and includes conclusive remarks.
* `Movie Recommendation Systems.ipynb` - comprehensive notebook containing all notebooks above in chronological order.

## Purpose

There are so many movies and online information about them to help viewers decide what to watch. With an overwhelming amount of information, it can be challenging for people to decide and watch a movie that they would enjoy. Recommendation systems are developed to help resolve this issue by providing movie recommendations. 

The following notebooks encode various recommendation systems using information provided by the latest and smallest dataset from [MovieLens](https://grouplens.org/datasets/movielens/). The selected content-based and collaborative filtering recommendation systems were chosen with respect to evaluating them appropriately across the same metric, the **mean average precision at k (MAP@k)**. 

Note that the two files from the dataset used were `movies.csv`, which contains each movie's name, ID and list of genres, and `ratings.csv`, which contains each user's rating from movies they have seen by their ID. 

# Data Cleaning

* Duplicate movie titles in `movies.csv` were identified under different movie IDs. Movie IDs of these duplicates were updated to match the unique movie ID across the data files, and the movie ID of these duplicates in `movies.csv` were removed. 
* Found not all movies in `movies.csv` were rated in `ratings.csv`. 
* Timestamps in 'ratings' were irrelevant for our purposes and were dropped.


# Exploratory Data Analysis (EDA)

**Finding 1**
Drama and Comedy are the most common genres found, followed by Thriller, Romance and Action.
![plot1](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/plot1.png)

**Finding 2**
The number of movies have increased at an increasing rate over time. Note that the last dip is due to the incomplete year of 2018 in the data.
![plot2](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/plot2.png)

**Finding 3**
Ratings range from 0.5 to 5 out of 5. It is a bimodal distribution with 3 and 4 out of 5 ratings as the most frequent; and an average rating of 3.5 out of 5.   
![plot3](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/plot3.png)

**Finding 4**
As well as the abundance and growth in Drama and Comedy movies, there has also been significant growth of Thriller and Documentary movies in the last 30 years.
![plot4](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/plot4.png)

**Finding 5**
The boxplot of each genre's distribution of ratings show higher median ratings for Drama, Crime, War, Mystery, Animation, IMAX, Film-Noir and Documentary movies, while Horror and (no genres listed) show a more likely tendency of lower ratings. The average ratings, as shown by the bar plot of average ratings for each genre, reflect these findings also.
![plot5](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/plot5.png)

**Finding 6**
Film-Noir and War movies have the highest average rating; and Horror and '(no genres listed)' genres have the lowest average rating.
![plot6](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/plot6.png)


**Finding 7**
* An upward trend in average ratings is noticed for movies released over the earlier half the twentieth century, and have marginally declined onwards for movies released later.

* Post 1910, there are more noticeable average ratings across genres falling under the overall average ratings, particularly between 1920 and 1960. Post 1960, average ratings across genres have converged with the marginal overall decline in average ratings.
![plot7](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/plot7.png)

# Modelling
Examples of two types of recommendation systems were built here.  

1. **Content-based filtering**: using item attributes to make recommendations - built one from scratch using movie genre information

2. **Collaborative filtering**: two types - memory-based and model-based to understand users' behaviours 
    
    * *Memory-based*: built using the data - both user-based and item-based (memory-based) collaborative filtering were built from scratch 
        * User-based: finds similar users to predict ratings of each users' unseen movies based on historical ratings
        * Item-based: finds similar items based on historical user ratings to predict unseen movie ratings for each user
    * *Model-based*: installed the Surprise package to apply the SVD, KNNBasic and KNNWithMeans algorithms to predict unseen movie ratings for each user.

# Results

*Content-based*
* Using genres only to develop content-based recommendations do not indicate a relatively high MAP@k. There was a bias to recommend movies associated with more genres listed. The recommeder could be improved by adding more attributes, such as movie cast names, directors and movie tags. 

*Recommenders built from scratch*
* The user-based recommender performed better than the item-based recommender. 
* This could be due to various reasons: 
    * There were less users than movies in the data 
    * User similarities were more distinguishable than item similarities; user similarities ranged between -1 and 1, while item similarities ranged between 0 and 1
    * The item-based recommender predicted a significant amount of zero ratings, overlooking a larger variety of movies to recommend

*Model-based recommenders*
* Performed better than recommenders built from scratch. This is likely due to the algorithms selecting only a number of user/item similarities rather than applying all users/items during computation. Given the sparsity in the user-item matrix, it is likely most users/items show insignificant levels of similarities. 
* The MAP@K, where K = 10, shows **item-item collaborative filtering using KNNBasic** performed the best on the test set, and marginally better than the other algorithms. 

![mapk](https://github.com/Bennett-Heung/MovieLens-Movie-Recommendations/blob/main/images/mapk.png)


# Conclusive Remarks

* The Surprise algorithms performed better and had a quicker execution time than the recommenders built from scratch.
* The MAP@K, where K = 10, shows item-item collaborative filtering using KNNBasic performed the best on the test set, and marginally better than the other algorithms.
