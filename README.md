# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 3: Natural Language Processing on cooking subreddits

#### by Karthik Nambiar

## Executive Summary

In the attached project notebooks, we analyze the r/Cooking and r/CookingforBeginners subreddits over the past 4 years using Natural Language Processing. We use a couple features, like domain and the time it was created, but the majority of analysis is on the post titles and the self-text. Our focus was trying to classify post titles and post content text based on what subreddit they came from. To that end, we used multiple classifiers and multiple techniques on both the titles of the post and post content text. 

This work has major implications for industry and reddit at large. In general, it can be hard to manage 2 similar subreddits. By looking at what factors make a 'Cookingforbeginners' post, you can move more people to that subreddit and even be able to predict using an automoderator that certain posts should be flagged as potentially belonging there. In a more general sense, if companies can find how beginner chefs talk or what kind of words they tend to use, they can use this information to either sell them knives or pots or training services or otherwise use that data to improve their own services and how they should market their goods.

Through our process, we were able to accurately predict 64.5% of posts based on their post text and 63.3% of posts based on their title. This is compared to the baseline of 50%. This is actually an impressive improvement given the limitations of the data nd the similarity between the subreddits. We have also gained useful information about what features were important in this model and why. Our process started with some methods. One was using a simple CountVectorizer to drop the titles into an understandable bag of words and a LogisticRegression. This was the base of our model and hit around 60% accuracy on post text and title. Then, we tried various techniques, such as a tfidfvectorizer in lieu of a countvectorizer, or Random Forest Classifiers, Extra Trees Classifiers, Adaboost, Gaussian Naive Bayes, and Multinomial Bayes in lieu of the LogisticRegression. Then, we took all those models, and searched over relevant hyperparameters and tried to maximize our accuracy score. Along the way, we also tried lemmatizing the data (converting it to a base dictionary form) using spacy and testing if that improved anything. In both self-text and titles, we found the best model was a mixture of models that would cover the blind spots of the other models- in particular, Multinomial Bayes, ExtraTrees, and RandomTreesClassifier, one using lemmatized data, turned out to yield the largest scores that were mentioned in the beginning. What we found to be the most important features were often helper verbs and joiner words, but furthermore, it seemed that users of 'r/CookingforBeginners' tended to use words like 'recipe', 'to help', 'make', and 'chicken' in their posts more. These are important- and in particular, the 'chicken' example might say something interest about what a starter cook might use as their go-to for meal-prep.

This work is promising and there are multiple ways we can improve on it, given support. We believe we can get more predictive results by combining all features together- in particular adding comments as a feature to help prediction and understanding their usage. Sentiment analysis could be interesting in its usage to determine how in general, users of these subreddits may differ in emotional valence. we also think that there would be a benefit at looking to see how the subreddit and its community changed over the past 4 years to see if perhaps a more restricted dataset could contribute more information and avoid issues where a subreddit culture has profoundly changed over time to the point that it's hard to find a commonality (as would be needed in our machine learning models). Beyond that, more complicated transformers (such as BERT) are available to provide more information than the methods we have used so far or better lemmitization (using a more thorough version of spacy). We might also be interested in carefully comparing which posts were classified incorrectly and thinking carefully about how to rectify that problem. Beyond that, more compute time to optimize better could be useful, or even creating a preliminary bot that identifies and flags potential sub posts that wouldn't fit in the sub guidelines and would perhaps be better off in the other subreddit.

## Coda- Brief discussion of data sources

As mentioned before, in this dataset, we used PushShift API and reddit to grab data. We'll describe briefly the variables used, and then explain the various pipelines/gridsearches. Please keep in mind the conventions used in this data. In short, with a notebook, any number is used to tie all estimators using the same series of estimators (ex: CountVectorizer->LogReg might be pipe1, gridsearch1, randomsearchcv1, paramlist1, etc). Furthermore, subscription of numbers (ex: 4_1) refers to a further modification by testing different hyperparameters, but still the same series of estimators. Similarly, subscripted *alt* refers to using a lemmatized dataset.

|Feature|Notebook|Description|
|---|---|---|
|**created_utc**|*all*|Time at which post was created (UTC seconds)|
|**domain**|*all*|Host domain of post-self posts are the subreddit, videos from where the video came from|
|**selftext**|*all*|Post text|
|**title**|*all*|Title of post|
|**subreddit_subscribers**|*all*|Number of subscribers to subreddit at time of posting|
|**body**|*all*|Body of the comment|
|**permalink**|*all*|Link to comment in question|
|**sub**|*all*|Classification sub name (r/Cooking or r/C4B)|
|**X_tr/telemma**|*p3*|Lemmatized X_train/test using spacy en_core_web_sm|
|**pipe1**|*p2*|(CountVectorizer,LogisticRegression)|
|**pipe2**|*p2*|(CountVectorizer,RandomForestClassifier)|
|**pipe3**|*p2*|(CountVectorizer,ExtraTreesClassifier)|
|**pipe4**|*p2*|(CountVectorizer,AdaBoostclassifier)|
|**vc**|*p2*|VotingClassifier(pipe1,rsc2,rsc3)|
|**pipe1**|*p3*|(CountVectorizer,LogisticRegression)|
|**pipe3**|*p3*|(CountVectorizer,AdaBoostClassifier)|
|**pipe4**|*p3*|(CountVectorizer,RandomForestClassifier)|
|**pipe5**|*p3*|(CountVectorizer,MultinomialNb)|
|**pipe6**|*p3*|(CountVectorizer,ExtraTreesClassifer)|
|**pipe7**|*p3*|(TfidfVectorizer,ft.todense(),GaussianNB)|
|**vc**|*p3*|VotingClassifier(gs5,rsc6,rsc5_alt)|
|**vc2**|*p3*|VotingClassifier(rsc4_1,rsc6,rsc5alt)|
