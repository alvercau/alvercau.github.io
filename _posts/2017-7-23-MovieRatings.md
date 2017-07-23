---
layout: post
title: Why every movie producer needs a data scientist.
---


Recently I carried out my very first linear regression project. I was going to figure out for a fictitious client what you have to do to make a popular Sci-Fi/Fantasy movie. My client was rich enough not to care about money, all he wanted was to be loved by the audience. I thus had to determine what influences movie ratings in a good way.  

I [scraped]( https://github.com/alvercau/project-luther/blob/master/Scraping_rotten_tomatoes.ipynb) movie reviews and ratings from Rotten Tomatoes, [cleaned]( https://github.com/alvercau/project-luther/blob/master/Project_Luther_data_cleaning.ipynb) the data and put it in a nice data frame. The two target variables were average rating by critics and average rating by the audience. Making a unified rating variable based on these two ratings was not a wise idea, since the standard deviation of the difference between the two was bigger than the average of the difference between the two. I would have to tell my client what he could do to be loved by the audience (a lot of people) and what he could do to be loved by the critics (not that many people).  

For [building my model]( https://github.com/alvercau/project-luther/blob/master/Regression_the_neat_way.ipynb), I only considered potentially relevant features that can be decided on before a movie is made: runtime, rating, director, release month. The thing is, linear regression is a powerful model for predicting continuous variables based on continuous, or at least numerical, variables. It isn’t fit for dealing with categorical variables, and most of my data was categorical. There is however a trick you can do to include categorical predictive variables in the model: you turn them into dummy variables, turning something like this:

|movie|director|
|---|---|
|Beauty and the Beast|Christophe Gans|
| Fantastic beasts and where to find them|David Yates|
| The Hobbit: The Battle of the Five Armies|Peter Jackson|
|The Hobbit: The Desolation of Smaug|Peter Jackson|

into something like this:

|movie|director[David Yates]|director[Peter Jackson]|
|---|---|---|
|Beauty and the Beast|0|0|
| Fantastic beasts and where to find them|1|0|
| The Hobbit: The Battle of the Five Armies|0|1|
|The Hobbit: The Desolation of Smaug|0|1|

where 1 means yes and 0 means no. A smart trick to make non-numerical things numerical, but a dangerous trick, because applying the trick introduces massive multicollinearity. Multicollinearity is a situation in which several of your predictive variables vary with each other, but not necessarily with the dependent variable. In the case of the movies above, if a movie has a 0 in the David Yates column, it will probably have a 1 in the Peter Jackson column. All of the zero-and-one columns vary with each other. Who cares? Linear regression does. Linear regression does not like the formation of groups where the dependent variable is not welcome, because these groups do not help finding relevant relations with the dependent variable.
Again, some smart people came up with techniques to reduce the influence of multicollinearity: regularization. This technique assigns weights to all of the predictive variables in such a way that in the end, only the relevant ones will be included in the model. It is a powerful technique, but not as powerful as 1000 movie directors.  
Although my model was terrible (for obvious reasons), it was pretty clear that the ‘director’ variable is very relevant when it comes to movie ratings. For the training data, only about 20% of the variation in the ratings could be explained by runtime, rating and release month, but 80% of the variation could be explained if we also took into account the director, indicating that some directors make loved movies and some of them make hated movies. The problem was that the was model over fit: it could not deal with unseen data and hence it was unable to predict which directors make movies with a good rating. The reason why was pretty obvious: if one of the directors in the unseen data was not in the training set, the model would have no clue of what to do with it because it did not know anything about the relation between this particular director and the rating. Most of the directors in my dataset only made one movie, so the model did not know what to do in most of the cases. More radical strategies were needed to get something out of the data.  
Since the problem was clearly the fact that most of the directors didn’t make enough movies to know whether they were good at it or not, I reduced my dataset to include only those movies that were made by a director that made at least five movies. If they had good ratings for all or most of their movies, or bad ratings for all or most of their movies, we could be sure that there effectively was a correlation between director and movie ratings. This did seem to be the case: about 45% of the variation in the ratings in the training data could be explained by runtime and director, and the same applied to the test data. I finally had a model that could deal with unseen data, and that could tell us which (productive) directors were good at their jobs and which directors weren’t.  
I went back to my fictitious client and gave him the following advice:  
Don’t worry too much about which choices to make, most of the factors that are in your hands are absolutely irrelevant. Just remember to work with a director that has proven to be successful. The same goes for the data scientist. I am pretty sure that anyone could have given the same advice without building a linear regression model.

