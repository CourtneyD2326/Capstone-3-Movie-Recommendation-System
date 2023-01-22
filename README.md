# Movie Recommendation System
Build a recommendation system to filter and predict only the movies a user prefers. 

![213908056-005e47b4-9b8b-4c6b-b3db-420e0e10ab9d](https://user-images.githubusercontent.com/85933265/213908562-4a423692-4b52-4a8a-adf4-52d596f34297.png)


# Context of the project

Everyone loves movies irrespective of age, gender, race, colour, or geographical location. We all are connected via this fantastic medium. Yet what is most interesting is how unique our choices and combinations are regarding movie preferences. A Recommendation System is a filtration program whose prime goal is to predict a user's “rating” or “preference” towards a domain-specific item or item. In our case, this domain-specific item is a movie. Recommendation systems also learn your viewing patterns and may suggest content based on the time of the day or your watching patterns. The recommender system helps users quickly find their preferred movies without searching from the extended content catalogue. From a business perspective, the more relevant content, or movies a user finds on any platform, the higher their engagement and, as a result, increased revenue. 

# Problem Statement

How can data from a movie-based platform recommend other movies to users based on their preferences and activities on that platform?

# The Dataset

The dataset contains the metadata (cast, crew, budget, etc..) of movies. Kaggle has removed the original version of this dataset per a DMCA takedown request from IMDB. To minimise the impact, Kaggle replaced it with a similar set of films and data fields from The Movie Database (TMDb) in accordance with their terms of use. There will be two datasets used: credits_data and movies_data. Both are CSV files, and a quick overview of the datasets are below: 

 <img width="850" alt="image" src="https://user-images.githubusercontent.com/85933265/213908203-75eb6404-b755-4e36-8ba7-b96ff02e8b36.png">

 <img width="850" alt="image" src="https://user-images.githubusercontent.com/85933265/213908184-4ad6cd43-37d9-49be-bb1f-98687b050346.png">
 
 # My approach to the problem
 
In this project, I built a recommendation system to filter and predict only the movies a user prefers.

There are three types of recommendation systems. 
•	Demographic Filtering: The recommendations are the same for every user. They are generalised, not personalised. These types of systems are behind sections like “Top Trending”. 
•	Content-based Filtering: These suggest recommendations based on the item metadata (movie, product, song, etc.). Here, the main idea is that if a user likes an item, they will also like items like it. 
•	Collaboration-based Filtering: These systems make recommendations by grouping users with similar interests. For this system, metadata of the item is not required. 

In this project, I built a content-based and demographic recommendation engine for movies. The approach to building the movie recommendation engine consists of the following steps. 
1.	Perform Exploratory Data Analysis (EDA) on the data 
2.	Build the recommendation system 
3.	Get recommendations 

# Data Wrangling and EDA on the dataset

I merged both datasets for easier manipulation of the data. There were no missing values in the datasets. I did remove a column of duplicates of the titles of the movies. I analysed how the data was spread regarding the vote average in the bar plot below.

<img width="850" alt="image" src="https://user-images.githubusercontent.com/85933265/213908306-65d7f750-aa77-40b8-9fea-940a0fc9ae49.png">

Findings: In the line plot below, it is evident that the most popular movies have the most votes even though the average vote range from 5 to 7.5. Movies with a vote average of 9 to 10 are less popular than movies with a vote count in the range of 5 to 7.

<img width="850" alt="image" src="https://user-images.githubusercontent.com/85933265/213908318-3c0338f0-73ab-4b65-8b6d-3f86b26a4961.png">

The accuracy of predictions made by the recommendation system can be adapted using the “plot/description” of the movie. But the quality of suggestions can be further improved using the movie's metadata. Let’s say the query to our movie recommendation system is “The pirates of the Caribbean”. Then the predictions should also include movies directed by the film's director, and it should also have movies with the cast of the given query movie. I utilised the following features to personalise the recommendation: cast, crew, keywords, and genres. The movie data is in the form of lists containing strings, and I would need to convert the data into a safe and usable structure. I applied the literal_eval() function to those features and created a soup of all the metadata information extracted to input into the vectoriser.

# Pre- Processing and Modelling

My original idea was to create a recommendation system focused on Content-based filtering, but I will also include demographic filtering. I will not be including collaboration-based filtering because I do not have a userID, which is necessary for collaborative filtering, and therefore another dataset would have to be used.

# Content-Based Filtering

# Plot description-based Recommender

I converted the word vector of each overview. I used the Term Frequency-Inverse Document Frequency (TF-IDF) vectors for each overview. Frequency is related to the frequency of a word in a document. Inverse Document Frequency is the relative count of documents containing the term given as a log (number of documents/documents with the term). The overall importance of each word to the documents in which they appear is equal to TF * IDF. This will give you a matrix where each column represents a word in the overview vocabulary (all the words that appear in at least one document), and each row represents a movie, as before. This reduces the importance of words frequently occurring in plot overviews and their significance in computing the final similarity score. To save time, TfIdfVectorizer from scikit-learn was used.

With this matrix in hand, we can now compute a similarity score. Many similarity methods could be used, such as the Euclidean, Pearson and cosine similarity scores. I used the cosine similarity to calculate a numeric quantity that denotes the similarity between two movies. I am using the cosine similarity score since it is independent of magnitude, relatively easy, and fast to calculate. I defined a function that takes a movie title as an input and outputs a list of the ten most similar movies. I also needed a mechanism to identify the index of a move in the metadata Data frame, given its title.

<img width="850" alt="image" src="https://user-images.githubusercontent.com/85933265/213908383-2f913ef5-1cde-46fc-9961-32b5d6a32956.png">

This system needs to do a better job with recommendations and returns movies of the same/similar title instead of broadening the range.

Our recommendation system would be improved if there were better metadata. I want to explore and build a recommendation system based on the following metadata: the director, related genres, actors and movie plot keywords. Therefore, developing a credit, genres, and keywords-based recommender was the better option. 

# Credits, Genres and Keywords-Based Recommender
I applied the same/similar functions as the above recommender and then computed the Cosine Similarity matrix based on the count_matrix. I then reused the get_recommendations() function by passing in the new cosine_sim2 matrix as the second argument. 

<img width="850" alt="image" src="https://user-images.githubusercontent.com/85933265/213908413-19a0c242-8ba7-44b4-902b-7a3fb38eb92e.png">

Now, the recommendation system is much more successful in capturing more information due to the metadata and have given better recommendations.

# Demographic Filtering
For this demographic filter, I needed a metric to score or rate a movie, calculate the score for every movie, sort the scores and recommend the best-rated movies to users. To do this, I used IMDB's weighted rating (wr), which is given as follows:

<img width="400" alt="image" src="https://user-images.githubusercontent.com/85933265/213908514-68e44cda-5fad-49a1-b757-e7670be71a82.png">


•   v is the number of votes for the movie;
•   m is the minimum votes required to be listed in the chart;
•   R is the average rating of the movie; And
•   C is the mean vote across the whole report

The mean rating for all the movies is approximately six on a scale of 10. The next step was determining an appropriate value for m, the minimum votes required to be listed in the chart, and I will use the 90th percentile as our cut-off. In other words, for a movie to feature in the charts, it must have more votes than at least 90% of the film in the list.

I defined a function called weighted_rating() and defined a new feature score, which calculates the value by applying this function to the data frame of qualified movies.
After further processing, I was able to create a plot of the most popular movies as below: 

<img width="850" alt="image" src="https://user-images.githubusercontent.com/85933265/213908463-4d49567d-b7d0-42ce-8a6b-fc269979ed57.png">

# Recommendations

Demographic recommenders provide a general chart of recommended movies to all the users, and they are not sensitive to the interests and tastes of a particular user. Therefore, a more intuitive system, like Content-based filtering, would be much better with the use of cosine similarity to calculate a numeric quantity that denotes the similarity between two movies.

# Further research into the topic

I would love to create a collaborative filtering-based recommendation engine, as I could have explored machine learning models for such a recommendation engine. Still, due to the limitations of my dataset that did not contain a UserID I could not have created one. Exploring and building a collaborative filtering-based recommendation system would be another project I would love to focus on next. 















