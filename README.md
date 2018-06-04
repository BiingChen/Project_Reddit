# Summary of the Project:
In this project, I scrapped posts from sub-reddits 'Jokes' and 'Confessions' and used text and auxiliary post features to predict which sub-reddit the post originated from.

The project consisted of 3 primary phases:
1. Data Acquisition
2. Data Cleansing/Preparation and Exploratory Data Analysis
3. Model building and Interpretation of Results

At the end, I had extra time and went back and fit more complex models and used GridSearch across the basic models as well.  The complex models performed better and had the highest accuracy ratings in the low 90s, but Logistic Regression was also able to achieve low 90s accuracy with GridSearch.

# Data Acquisition:
In order to acquire the data, I used the Requests module and pulled posts in JSON format.  I scrapped 1000 posts from each subreddit.  After removing duplicates, I ended up with 1951 total posts.


## Data Cleansing/Preparation and EDA:

### Cleansing of Text:
Since these are real posts pulled from the internet, they are naturally “messy”.  I cleansed the title and selftext by putting it through Beautiful Soup in order to remove all HTML related artifacts.  Using regex, I removed anything that was not text.  Finally, I also put the text through a lemmatizer which returns the ‘lemma’ of a word (base/dictionary form).   I tried stemming the text using the Porter stemmer as well, but decided against it since it did not seem to improve my model performance, and also resulted in the truncated text that was more difficult to understand.

### Preparation of Text:
The cleansed title and post text were combined and then put through a TFIDF Vectorizer since the models require all the inputs to be numeric.  I chose the TFIDF vectorizer over more simple vectorizers (such as count vectorizer) since TFIDF takes the term frequency for each word in a document, but also divides by the overall prevalence of each word in the entire corpus of text.  This usually improves modeling results because words that tend to appear very frequently in the entire corpus of text will tend to have less ability to help distinguish between posts.  During vectorization, I also removed stop words and punctation.  Stop words are common English words such as ‘the, a, he, she’ that are common to all English text and have little discriminatory value.

### Preparation of Auxiliary Post Features:
In addition to the title and text of the post, the data from Reddit also included auxiliary information about each post, including the author, create date, number of comments, over_18, and voting score.  Since I am trying to categorize these posts based on sub-reddit, I chose to ignore the author and create date for the posts as they do not provide much information to help classify the posts into sub-reddits.  I also did some light feature engineering by calculating the total length of the title and post text since its possible that posts from one sub-reddit tended to be longer than the other.

## Exploratory Data Analysis:
I looked at the top key words for each subreddit as well as averages for each of the auxiliary features for the posts.  Some of the key words that stood out for confessions were:  'feel', 'know', 'people', 'friend', 'think', 'love', 'girl', 'guy'.  Some of the key words that stood out for jokes were:  'man', 'say/said', 'woman', 'wife', 'husband','people'.

Some of the auxiliary features varied widely between the two subreddits.  Jokes tended to have signficantly higher score (371), higher percentage of over_18 posts (6%), and shorter selftext length (199).  Confessions tended to have lower score (42), lower percentage of over_18 posts (1%), and longer selftext length (685).

# Results and Key Findings:
I tried a wide variety of models in their base form and with Grid Search.  The best performing model was Adaboost with an accuracy score of 0.91 - 0.92.  Logistic Regression didn't perform as well, but I through grid search I was able to improve the accuracy from 0.87 to 0.89.  GradientBoost with grid search performed second best with 0.91 accuracy.

By looking at the coefficients and feature importance the best features for classifying the subreddits turned out to be unsurprisingly the auxiliary features.  Adaboost ranked text length, number of comments, score, and title length as the top features.  Logistic Regression ranked number of comments high in its coefficients as well as the word 'feel', 'love', 'want', 'life'.  Jokes had high coefficients for 'say', 'doctor', 'wife', 'asks', 'joke'.

Overall, I was not too surprised at the findings.  When people post confessions they often talk about their feelings and the posts are usually about other people in their life, and how they feel about it.  Jokes also often were about people, and usually used as the topic a man, woman, husband or wife.  Due to the relatively large differences in score, over_18, and selftext length those auxiliary factors were also major features for classification.

