# Recommender Systems
Recommender systems are among the most popular applications of data science today. They are used to predict the "rating" or "preference" that a user would give to an item. Almost every major tech company has applied them in some form or the other: Amazon uses it to suggest products to customers, YouTube uses it to decide which video to play next on autoplay, and Facebook uses it to recommend pages to like and people to follow.

Recommender systems can be classified into 3 types:
- Simple recommenders: offer generalized recommendations to every user, based on popularity and/or genre.

- Content-based recommenders: suggest similar items based on a particular item. This system uses item metadata, such as genre, author, etc. to make 
these recommendations.

- Collaborative filtering engines: these systems try to predict the rating or preference that a user would give an item-based on past ratings and preferences of 
other users.

## Movies metadata
In this section, you will build a simplified clone of IMDB Top 250 Movies using metadata collected from IMDB.

### Simple recommenders
As described in the previous section, simple recommenders are basic systems that recommends the top items based on a certain metric or score. 
The following are the steps involved:
- Decide on the metric or score to rate movies on.
- Calculate the score for every movie.
- Sort the movies based on the score and output the top results.

#### Metric
One of the most basic metrics you can think of is the rating. However, using this metric has a few caveats. For one, it doesn't take into consideration the popularity of a movie.

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

Then I built my recommender as follows:
- Get the index of the movie given its title
- Get the list of cosine similarity scores for that particular movie with all movies
- Sort the list based on the similarity scores
- Get the top 10 elements of this list
- Return the titles corresponding to the indices of the top elements

Result:

We see that, while my system has done a decent job of finding movies with similar plot descriptions, the quality of recommendations is not that great. "The Dark Knight Rises" returns all Batman movies while it more likely that the people who liked that movie are more inclined to enjoy other Christopher Nolan movies. This is something that cannot be captured by your present system.

#### Credits, Genres and Keywords based recommender
Then, I built a recommender based on the following metadata: the 3 top actors, the director, related genres and the movie plot keywords.

The next step would be to convert the names and keyword instances into lowercase and strip all the spaces between them. This is done so that your vectorizer doesn't count the Johnny of "Johnny Depp" and "Johnny Galecki" as the same. After this processing step, the aforementioned actors will be represented as "johnnydepp" and "johnnygalecki" and will be distinct to your vectorizer.

I created a "metadata soup", which is a string that contains all the metadata that we want to feed to our vectorizer: actors, director and keywords.

The next steps are the same as what I did with your plot description based recommender. One important difference is that I use the CountVectorizer() instead of TF-IDF. This is because we don't want to down-weight the presence of an actor/director if he or she has acted or directed in relatively more movies. It doesn't make much intuitive sense.

Result:

We see that your recommender has been successful in capturing more information thanks to more metadata and has given you better recommendations.

## MovieLens data

### Collaborative filtering
Collaborative filters can further be classified into two types:
- User-based Filtering: these systems recommend products to a user that similar users have liked
- Item-based Filtering: these systems are almost similar to the content recommendation engine that you built. These systems identify similar items based on how people have rated it in the past.
