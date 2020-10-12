# 1. Project Summary and Problem Statement

Get Bent! takes a Twitter username and classifies that user as either conservative or liberal.

Initially, I intended this project to explore the hashtag #wrongtrump.   As the project evolved, I realized that I was attempting to classify users as either conservatives or liberals, and that became the focus.  The following are a specific and general problem statement for this project.

● Looking at a politically charged Twitter hashtag, how do I know which users are conservatives and which are liberals?

● Given any polarizing topic, can I classify which conflicting ideology an individual represents?

# 2. File Descriptions

### [Data](http://localhost:8888/lab/tree/data) : Folder containing all scraped twitter data and clean CSVs.  

**[blue](http://localhost:8888/lab/tree/data/blue), [blue2](http://localhost:8888/lab/tree/data/blue2), [red](http://localhost:8888/lab/tree/data/red), [red2](http://localhost:8888/lab/tree/data/red2), [3am](http://localhost:8888/lab/tree/data/3am), [4am](http://localhost:8888/lab/tree/data/4am)**:  Folders containing files that correspond to individual Twitter users.  blue and red contain the training users.  3am and 4am are folders containing all scraped tweets from users who participated in "wrong trump" until 4am UTC, the night of Robert Trump's death.

**[data/wt_corpus.csv](data/wt_corpus.csv)**:  CSV file containing the text of every tweet participating in "wrong trump" in the day following Robert Trump's death.

**[data/wrong_trump_final_final.csv](data/wrong_trump_final_final.csv)**:  CSV file containing usernames, predicted labels, and probability of each label.  This file only contains ~1,000 users who I was able to scrape while testing **get_bent**.

## Models

**[final_doc2vec](http://localhost:8888/lab/tree/final_doc2vec)**: Fully trained Doc2Vec model to infer vectors on unseen data.

**[svc_final_model.pkl](http://localhost:8888/lab/tree/svc_final_model.pkl)**: Train Support Vector Classifier model for classifying inferred vectors.

## Notebooks

1. [Early Webscraping](http://localhost:8888/lab/tree/1.InitialScraping.ipynb): Initial scrape of the #wrongtrump hashtag using GetOldTweets3
2. [Early EDA](http://localhost:8888/lab/tree/2.EDA.ipynb): Some early EDA to find out a little more about who is actually tweeting about #wrongtrump.
3. [Training Scrape](http://localhost:8888/lab/tree/3.Training%20Scrape.ipynb): Scraping every tweet for the 100+ training users.
4. [Update Training Documents:](http://localhost:8888/lab/tree/4.Update%20Training%20Docs.ipynb)  A function that allows me to input a list of users, scrape all tweets and join them into one document, and then add them to the training corpus.
5. [Doc2Vec Model](http://localhost:8888/lab/tree/5.Doc2Vec%20Model.ipynb): Function that trains a new Doc2Vec model on a dataframe of training documents
6. [Model Selection](http://localhost:8888/lab/tree/6.Model%20Selection.ipynb): This notebook splits my training data and runs it through multiple classification methods in order to choose a final classifier.
7. [Prediction Function](http://localhost:8888/lab/tree/7.Prediction%20Function.ipynb):  Takes as input a series of usernames and attempts to make predictions.
8. [get_bent.py](http://localhost:8888/lab/tree/8.get_bent.ipynb): A function that takes in a Twitter username and predicts their political ideology

# 3. Technologies Used

- [GetOldTweets3](https://github.com/Mottl/GetOldTweets3): Scraper that allowed you to go beyond Twitter API limitations but lost its utility when Twitter changed how they set html tags.
- [snscrape](https://github.com/JustAnotherArchivist/snscrape):  Scraper for most social networking platforms, was able to get around the new Twitter API.
- [Gensim Doc2Vec](https://radimrehurek.com/gensim/models/doc2vec.html):  Training model to output document embeddings.

# 4. Executive Summary

### 1. Webscraping, Cleaning, EDA

Initially while using, GetOldTweets3, I was scraping all the tweets from a single hashtag with the intention of classifying each individual tweet.  When Twitter's API changed, so did my project, and with snscrape I began scraping every tweet from every user on that topic.  I didn't know this at the time but the volume of this data would be important.  Cleaning and EDA was minimal and I essentially only used Gensim's simple preprocess function to tokenize the data.  Once I knew I would be feeding the information to a neural network (Doc2Vec), I knew I would not need to do much cleaning.

### 2. Modelling and Hyperparameter Tuning

I use two ML models in this project.  Doc2Vec is neural network that represents a single document in an array of vectors that are intended to pull out the context of the document's words.  I used a high number of vectors per document and a high number of minimum appearances per word for some simple dimensionality reduction.  The second model was a simple support vector classifier that I performed no parameter tuning on.  SVC and logistic regression were so accurate, I was not able to tune my model.

### 3. Evaluation

My training and testing set gave me nearly perfect accuracy.  In general, when reviewing predicted labels on random Twitter users I found my model to be as accurate as my training set.  

When my model makes an incorrect prediction it is generally easy to decipher why the model was wrong.  For example, rare political tweets that use positive words around President Trump might not necessarily belong to a conservative user.

### 4. Conclusions and Next Steps

I believe that I have found a way to classify Twitter users as belonging to one ideological group or another.  In a contentious conversation, where everyone is angry and talking about the same topic, if you have load the correct training set and only predict users that are a part of that conversation, classification using my function is a breeze.

# 5. References

Press, Associated. “Hashtag #Wrongtrump Trends on Twitter after Death of President's Brother.” *Fox News*, FOX News Network, 16 Aug. 2020, www.foxnews.com/politics/hashtag-wrongtrump-trends-on-twitter-after-death-of-presidents-brother. 

“My Pipeline of Text Classification Using Gensim's Doc2Vec and Logistic Regression.” *Notes on My Journey of Learning*, 14 Jan. 2018, fzr72725.github.io/2018/01/14/genism-guide.html. 

“Small Share of U.S. Adults Produce Majority of Political Tweets.” *Pew Research Center - U.S. Politics & Policy*, Pew Research Center, 17 Aug. 2020, www.pewresearch.org/politics/2019/10/23/national-politics-on-twitter-small-share-of-u-s-adults-produce-majority-of-tweets/. 

<u>Classifying Political Orientation on Twitter: It’s Not Easy!</u>, Raviv Cohen and Derek Ruths

