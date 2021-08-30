# Recommender Systems
[\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}]

$$\cos^2θ+\sin^2θ=1$$	

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

It is necessary that you come up with a weighted rating that takes into account the average rating and the number of votes it has garnered. Mathematically, it is represented as follows:

Weighted Rating $\Large WR=\frac{v}{v+m}$
(v/v+m.R)+(m/v+m.C)
where,
- v is the number of votes for the movie;
- m is the minimum votes required to be listed in the chart;
- R is the average rating of the movie; And
- C is the mean vote across the whole report
You already have the values to v (vote_count) and R (vote_average) for each movie in the dataset. It is also possible to directly calculate C from this data.

What you need to determine is an appropriate value for m, the minimum votes required to be listed in the chart. There is no right value for m. You can view it as a preliminary negative filter that ignores movies which have less than a certain number of votes. The selectivity of your filter is up to your discretion.

In this case, you will use the 90th percentile as your cutoff. In other words, for a movie to feature in the charts, it must have more votes than at least 90% of the movies in the list. (On the other hand, if you had chosen the 75th percentile, you would have considered the top 25% of the movies in terms of the number of votes garnered. As the percentile decreases, the number of movies considered increases. Feel free to play with this value and observe the changes in your final chart).

### Content-based recommenders

## MovieLens data

### Collaborative filtering

