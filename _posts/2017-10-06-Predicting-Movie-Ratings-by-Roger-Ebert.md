---  
layout: post  
title: Predicting Movie Ratings by Roger Ebert  
---  

From entertaining B-movies to Hollywood summer blockbusters, I've always enjoyed watching and discussing movies with friends, breaking down the good, the bad, and the ugly. When it comes to critiquing movies professionally, no one had a more talked about review than Roger Ebert. The second project at Metis focused on using web scraping techniques and building a linear regression model to learn more about a topic of our choice and see if we could predict certain outcomes. For this project, I wanted to analyze how Ebert reviewed and rated movies and asked the question, *Can* *we* *predict* *how* *Roger* *Ebert* *would* *rate* *movies* *if* *he* *were* *alive* *today*?       

![Ebert Thumbs Up](https://zachheick.github.io/images/Project_Luther_images/ebert_thumbs_up.jpg){: .center-image }  

## Data Collection    

I used Python's BeautifulSoup and Selenium libraries to scrape data about the movies Ebert reviewed and rated from his [website](https://www.rogerebert.com). Once the data was scraped and cleaned, I realized that there might not be enough features to create an accurate model. I wanted other ratings to compare his to, so I scraped user ratings from IMDb. I also downloaded a dataset of user ratings from another large movie rating site, MovieLens. IMDb scores were on a scale of 0.0 to 10.0 and MovieLens scores were on a scale of 0.0 to 5.0. The IMDb and MovieLens data was cleaned and merged with Ebert's data into one dataset of 4749 total ratings.

![Final Dataset](https://zachheick.github.io/images/Project_Luther_images/ebert_final_df.png){: .center-image }    

## Exploratory Analysis and Feature Selection   

I took a look at Roger Ebert's rating distribution first. Ebert rates movies on a scale of zero to four stars, incrementing by halves. Looking at the bar plot, he gave almost half of the movies a 3 to 3.5 rating. Ebert also does not give too many movies a terrible rating of 0 to 0.5 stars.  

![Ebert Rating Distribution](https://zachheick.github.io/images/Project_Luther_images/ebert_rating_distribution.png){: .center-image }  

When thinking about what features to include in the model, a good practice is to start with a single feature and build from there. Features that came to mind were the IMDb and MovieLens user ratings. I wanted to see if there was any correlation between Ebert's ratings and either of the user ratings from other sources. I plotted each feature separately, but did not see a good linear relationship. I had also tried to re-engineer the IMDb and MovieLens features, but could not get a better result.  

![Single Feature Graphs](https://zachHeick.github.io/images/Project_Luther_images/ebert_single_feature_graphs.png){: .center-image }  

I took a look at the pair plot of the rest of the features to see if there was something I had missed, but once again did not notice any patterns, specifically in how the features were correlated with Ebert's `Star_Score`. The other ratings, however, do seem to have some correlation with each other.

![Feature Pair Plot](https://zachheick.github.io/images/Project_Luther_images/ebert_pair_plot.png){: .center-image }  

Because of the difficulty finding correlations to Star_Score when looking at the pairplot, I looked at the correlation scores to get a better understanding.  

## The Linear Regression Model  

Some of my features were categorical. To prepare my dataframe for modeling, I needed to "dummy" these features. Features with multiple categories had their categories assigned as individual columns. A boolean value of 0 or 1 was used to indicate if a row fell into some category. I removed the Title column and the dataframe now only has numeric values. Next, I split the data up into training and test sets.  
  
```python
X = df.drop('Star_Score', axis=1)
y = df['Star_Score']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=22)
```  

The features I start with were `MovieLens_Score` and `Imdb_Score`. I slowly added features that had either stronger positively correlation or stronger negatively correlation until I got to an Adjusted R-squared value that was best. For example, the `Genre_Drama` had a correlation score of 0.19 and `Sub_Genre_Comedy` had a correlation score of -0.12.  

The summary of my final model with 19 features that include `Runtime`, `MovieLens_Score`, `IMDb_Score`, and multiple genres and sub-genres:  

![Model Summary](https://zachheick.github.io/images/Project_Luther_images/ebert_model_summary.png){: .center-image }  

I couldn't just rely on the adjusted R-squared value for measuring the model. I also needed to look at the distribution of residuals to make sure the model was capturing all trends across the whole set of training data. A normal distribution of the model's residuals indicated this.  

![Model Residuals Distribution](https://zachheick.github.io/images/Project_Luther_images/ebert_residual_distribution.png){: .center-image }  

Another check to see if the model was performing consistently across the whole set of data was using cross validation.  

```python
# in-sample
in_lr = linear_model.LinearRegression()
in_lr.fit(X_1, y_train)
y_pred_in = in_lr.predict(X_1)
np.sqrt(mean_squared_error(y_train, y_pred_in))
>>> 0.71815221525694273

# out-of-sample
out_lr = linear_model.LinearRegression()
out_scores = cross_val_score(out_lr, X_1, y_train, scoring='neg_mean_squared_error', cv = 5) * -1
np.sqrt(out_scores.mean())
>>> 0.72441388004974472
```  

I got a slightly higher root mean squared error on the out-of-sample data than the in-sample data. This meant that the model generalized the data well. I test my model on the test set and get a final root mean squared error of 0.73, meaning the model's predictions were off by slightly more than a half star rating. I plotted the actual Ebert ratings vs the model's predicted ratings.  

![Model Predicted Ratings](https://zachheick.github.io/images/Project_Luther_images/ebert_model_predictions.png){: .center-image }  
Looking at the model visualization, this model loses some accuracy as Ebert's rating decreases. Even at the higher ratings, the model did not perform terribly, but there's definately room for improvement.

## Predict Recent Movies  

With a finished model, I got some recent movies and plugged their values into the model to get a predicted rating out of four stars.  

```python
Dunkirk = np.array([106,3.98,8.4,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0])
Emoji_Movie = np.array([86,1.08,2.2,1,0,0,0,0,0,0,0,0,1,0,0,0,0,0])
La_La_Land = np.array([128,3.80,8.2,0,1,0,0,0,0,0,0,0,0,0,0,0,0,1])
Mad_Max_Fury_Road = np.array([120,3.83,8.1,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0])
print('Dunkirk:', model_fit.predict(Dunkirk)[0])
print('The Emoji Movie:', model_fit.predict(Emoji_Movie)[0])
print('La La Land:', model_fit.predict(La_La_Land)[0])
print('Mad Max Fury Road:', model_fit.predict(Mad_Max_Fury_Road)[0])
>>> Dunkirk: 3.60695422058
>>> The Emoji Movie: 1.31972335311
>>> La La Land: 3.12637559205
>>> Mad Max Fury Road: 3.75669926337
```  

## Thoughts about the Model, What I Learned, and Future Work  

Overall, I'm happy with how my model turned out. The root mean squared error of 0.73 stars is a bit high, but when testing some movies that came out recently, the ratings did not seem unbelievable. Something significant that I learned was that even though a model could have good characteristics, such as a high adjusted R-squared or normally distributed residuals, it does not guarantee good performance when it comes to cross validation.  I also could have explored regularization techniques such as Lasso, Ridge, and Elastic Net in order to improve feature selection.     

For the future, I could improve this model by including data from other well known film critics. This model only had internet user ratings to compare to, but I think there could be some stronger correlations between Roger Ebert and other professional film critics. 

## Project source  
The source can be found on my [github](https://github.com/ZachHeick/Project_Luther).
