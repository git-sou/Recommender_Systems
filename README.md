# Recommender Systems
Recommender systems are among the most popular applications of data science today. They are used to predict the "rating" or "preference" that a user would give to an item. Almost every major tech company has applied them in some form or the other: Amazon uses it to suggest products to customers, YouTube uses it to decide which video to play next on autoplay, and Facebook uses it to recommend pages to like and people to follow.

Recommender systems can be classified into 3 types:
- Simple recommenders: offer generalized recommendations to every user, based on popularity and/or genre.

- Content-based recommenders: suggest similar items based on a particular item. This system uses item metadata, such as genre, author, description, etc. to make 
these recommendations. The general idea behind these recommender systems is that if a person liked a particular item, he or she will also like an item that is 
similar to it.

- Collaborative filtering engines: these systems try to predict the rating or preference that a user would give an item-based on past ratings and preferences of 
other users. Collaborative filters don't require item metadata. 

## Movies metadata
In this section, you will build a simplified clone of IMDB Top 250 Movies using metadata collected from IMDB.

### Simple recommenders
As described in the previous section, simple recommenders are basic systems that recommends the top items based on a certain metric or score. 
The following are the steps involved:
- Decide on the metric or score to rate movies on.
- Calculate the score for every movie.
- Sort the movies based on the score and output the top results.

#### Metric
One of the most basic metrics you can think of is the rating. However, using this metric has a few caveats. For one, it doesn't take into consideration the popularity of a movie. Therefore, a movie with a rating of 9 from 10 voters will be considered 'better' than a movie with a rating of 8.9 from 10,000 voters. So it is more difficult to discern the quality of a movie with extremely few voters.

It is necessary that you come up with a weighted rating (WR) that takes into account the average rating and the number of votes it has garnered. Mathematically, it is represented as follows:

<p align="center">
<img src="https://user-images.githubusercontent.com/83417933/131492903-e34e0d25-1feb-48b9-aead-b21566eb96f8.png" />
</p>

where,
- v is the number of votes for the movie
- m is the minimum votes required to be listed in the chart
- R is the average rating of the movie
- C is the mean vote across the whole report

We can get all the values except m whose we need to choose an appropriate value. There is no right value for m but it could be a preliminary negative filter. I chose the 90th percentile as your cutoff. In other words, the movie must have more votes than at least 90% of the movies in the list. As the percentile decreases, the number of movies considered increases.

Result:

### Content-based recommenders

#### Plot description based recommender
In this section, I built a system that recommends movies that are similar to a particular movie. More specifically, I computed pairwise similarity scores for all movies based on their plot descriptions and recommend movies based on that similarity score.

To do this, I need to compute the word vectors of each overview or document.

I computed Term Frequency-Inverse Document Frequency (TF-IDF) vectors for each document. The TF-IDF score is the frequency of a word occurring in a document, down-weighted by the number of documents in which it occurs. This is done to reduce the importance of words that occur frequently in plot overviews and therefore, their significance in computing the final similarity score.

This gave me a matrix where each column represents a word in the overview vocabulary and each column represents a movie. Then I computed a similarity score. There are several candidates:
- Euclidean
- Pearson
- Cosine similarity 

Again, there is no right answer to which score is the best. Different scores work well in different scenarios and it is often a good idea to experiment with different metrics.

I used the cosine similarity to calculate the similarity between two movies. Cosine similarity score is independent of magnitude and is relatively easy and fast to calculate:
<p align="center">
<img src="https://user-images.githubusercontent.com/83417933/131520810-5a43ab61-ce91-474d-b626-23d3d7f13796.png" />
</p>

Then I built my recommender as follow:
- Get the index of the movie given its title
- Get the list of cosine similarity scores for that particular movie with all movies
- Sort the list based on the similarity scores
- Get the top 10 elements of this list
- Return the titles corresponding to the indices of the top elements

Result:

We see that, while my system has done a decent job of finding movies with similar plot descriptions, the quality of recommendations is not that great. "The Dark Knight Rises" returns all Batman movies while it more likely that the people who liked that movie are more inclined to enjoy other Christopher Nolan movies. This is something that cannot be captured by your present system.

#### Credits, Genres and Keywords based recommender

## MovieLens data

### Collaborative filtering

