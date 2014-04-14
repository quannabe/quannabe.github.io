---
layout: post
title: Building A Financial Article Text-Sentiment API
category: posts

---

I recently took a course covering some Cloud Computing concepts: [CSCIE-90][cs90]. In the course, we worked with AWS & Azure, RESTful services, as well as working with tools like Hadoop and Hive. For my final project, I decided to train a text-sentiment model using NLTK and NLTK-Trainer to categorize financial articles as 'Positive' or 'Negative' Then, make this model available for querying via a Python Flask RESTful API. 

Assumptions:
Below, I will go through the steps I used to create this model. I assume you have some familiarity with Python (I used 2.7). If you're not working on a Mac, the instructions below will likely be somewhat different from what you need.

Install NLTK & NLTK-Trainer:

I chose to work with the Python package Natural Language Toolkit (NLTK): http://nltk.org/ and NLTK-Trainer: https://github.com/japerk/nltk-trainer. Once we install both, we can now start using the classified corpus of classified documents from above to train our classifier. 

When we installed NLTK and NLTK-Trainer above, a directory should have been created: 
~/nltk_data/corpora/

Inside the 'corpora', create a directory 'fin_articles'. Inside that, add two more directories: 'neg' and 'pos'. 

---

First, we need to hand-classify financial articles (Or, examples of whatever text you would like to classify positive/negative). I chose to find 100 positive & 100 negative to classify. To find these articles, I went to finance.yahoo.com, entered a ticker (ex. ‘AAPL’), then scroll down to view different articles for that equity. This process is admittedly rather imperfect and subjective. 

Our model will be looking for words more commonly found in positive articles than in negative articles and vice-versa. To avoid having a specific company name/ticker be biases positive/negative, I tried to choose the same number of positive and negative articles for each ticker.

Now, copy the plaintext for each article, and then save it in a new .txt file in either the 'neg' or 'pos' directories we created above.

---

Train our classifier

Now, we are looking to train our sentiment model. Inside NLTK-Trainer we have the python script train_classifier.py. We can use train_classifier.py with several different options and our trainerd corpus to generate the classifier using the following command:

python train_classifier.py --algorithm NaiveBayes --instances files --fraction 0.7 --no-pickle --min_score 2 --ngrams 2  --show-most-informative 10 fin_articles

A short explanation of some of the above command options:

--algorithm NaiveBayes: We are using the ‘NaiveBayes’ algorithm to train this model
--fraction 0.7: We are using 0.7 of our files as training, the other 0.3 as validation
--no-pickle: This option means we don’t want to save (pickle) the classifier-- more on this later
 --min_score 2: We want a min score of 2 to include the feature
--ngrams 2: Use 2 grams (bigrams). Or, features are combinations of 2 words 
--show-most-informative 10: Output the 30 most informative features
fin_articles: Is the name of the directory where we stored our training data 

I experimented with several different combinations of training options, and finally settled on the above command to product my classifier. The output of this command:

[ INSERT SCREENSHOT HERE ]

Decent accuracy of: 0.93333!

We can also see the 10 most informative features (bigrams) listed.

Now, if we want to save this classifier, we simply remove the --no-pickle option, and our classifier will be saved in: ~/nltk-data/classifiers/

---

Use Trained Classifier

Now, we have a saved (pickled) trained classifier that we can use to classify articles! We wish to use this to use classifier to work with new articles. So, we create a Python class that uses this classifier: SentimentClassifier.py. Let’s go through the important parts of that script:

Import other modules, and our pattern recognition to split the words into an array:





[cs90]: http://isites.harvard.edu/icb/icb.do?keyword=k98753

---

