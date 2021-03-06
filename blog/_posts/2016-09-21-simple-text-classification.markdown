---
layout: post
title:  "Text Classification Made Simple&nbsp;&nbsp;"
date:   2016-09-21 12:00:00
tags: NLP api software text classification monkeylearn fasttext scikit-learn
comments: true
---

<p class="first">
When you need to tackle an NLP task &mdash; say, text classification or sentiment 
analysis &mdash; the sheer number of available software options can be overwhelming. 
Task-specific packages, generic libraries and cloud APIs 
all claim to offer the best solution to your problem, and it can be hard to decide 
which one to use. In this blog post we’ll take a look at some of the available options for a text classification task, and discover their main advantages
and disadvantages.
<p/>

<p> The NLP task in this blog post is a classic instance of text classification: 
we’ll teach the computer to detect the topic in a news article. The task itself is 
pretty straightforward: the topics (sports, finance, entertainment, etc.) are very 
general, and their number is limited to four or five. We’ll use <a 
href="http://goo.gl/JyCnZq">two freely available data sets</a> to train and test 
our classifiers: the AG News dataset and the Sogou News dataset. Our focus is
on three available solutions:
FastText, a command-line tool, Scikit-learn, a 
machine learning library for Python, and MonkeyLearn, a cloud service. We’ll keep 
parameter optimization to a minimum and evaluate the off-the-shelf models that the 
various solutions offer.</p>

<h3>The command-line tool: FastText</h3>

<p>In July, Facebook’s AI lab released <a 
href="https://github.com/facebookresearch/fastText">FastText</a>, a command-line tool for building 
word embeddings and classifying text. The open-source software package has been
available on Github since early August. Reactions to the release were mixed. One news source claimed <a 
href="https://www.inverse.com/article/19878-facebook-fasttext-natural-language-processing-artificial-intelligence">“Facebook’s 
FastText AI is the Tesla of Natural Language Processing”</a>, and “researchers just supercharged 
autonomous artificial intelligence”. <a 
href="https://twitter.com/yoavgo/status/751178795323908096">Other people were less 
enthusiastic</a>, and pointed out FastText is merely a re-implementation of existing techniques. 
</p>

<p>Revolutionary or not, FastText is extremely easy to use. When you’ve prepared your training data correctly (one
piece of text per line, with the class of the text prefixed by “__label__”), you 
can train a classifier with a single command:</p>

<div class="highlight">
> ./fasttext supervised -input data_train -output model
</div>

<p>Testing your trained classifier on a set of held-out test data is also a breeze:</p>

<div class="highlight">
> ./fasttext test model.bin data_test
</div>

<p>FastText surely lives up to its name. On my laptop, it takes 3 to 4 seconds to train a 
classifier on the single words of the 120,000 AG training texts. The 450,000 
training samples in the Sogou corpus keep it busy for about 2 minutes. With one optional 
parameter setting, the model takes both single words and bigrams (2-word sequences) as features. 
This doubles the training time to 7 seconds for the AG corpus and 4 minutes for Sogou. That’s impressive.</p>

<p>The trained classifiers are also pretty accurate. In my experiments, the unigram model with the 
standard settings achieved an accuracy of 91.5% on the AG test set, and 93.2% on the Sogou test 
set. Adding bigrams as features led to an increase in accuracy of 0.2% for AG (91.7%), and 2.3% for 
Sogou (95.5%). These figures are slightly lower than those reported in <a 
href="https://arxiv.org/pdf/1607.01759v2.pdf">the paper that accompanied the release of 
FastText</a>, possibly due to slight differences in tokenization or parameter settings. The paper 
further demonstrates that this accuracy is state-of-the-art, not only on the AG and Sogou corpora, 
but also on other “sentiment” datasets (although, contrary to what the authors suggest, AG and 
Sogou are topical rather than sentiment data).</p>

<p>This state-of-the-art performance may be due to FastText’s reliance on neural networks rather 
than more traditional, linear models, where parameters cannot be shared among features. Its 
networks learn low-dimensional representations for all features in a text, and then average these 
to a low-dimensional representation of the full text. The resulting models are therefore able to represent 
similarities between different features. For example, the model might learn that <em>big</em> and 
<em>large</em> are near-synonyms, and should be treated in a similar manner. That can be really useful, particularly when you’re classifying short texts.</p>

<p>It’s clear FastText is a neat little software package that deals with large
volumes of data easily and produces high-quality classifiers. Let’s find out how it compares 
to the competition.</p>

<h3>The software library: Scikit-learn</h3>

<p>While FastText only has neural networks to learn a classifier, it can often be worthwhile to 
explore some alternative approaches. That’s where more generic machine learning software libraries 
come in. One great example of such a library is <a 
href="http://scikit-learn.org/stable/">Scikit-learn</a> for Python. Scikit-learn offers a wealth of 
machine learning approaches and makes it really easy to experiment with various models and parameter 
settings. When you’re doing text classification, its Multinomial Naive Bayes classifier is a simple 
baseline to try out, while its Support Vector Machines can help you achieve state-of-the-art 
accuracy.</p>

<p>Training and testing a model with Scikit-learn is more involved than with FastText, but not very 
much so. The tutorial <a 
href="http://scikit-learn.org/stable/tutorial/text_analytics/working_with_text_data.html">Working 
with Text Data</a> summarizes the most important steps. Basically, what we need is a pipeline with 
three components: a vectorizer that extracts the features from our texts, a transformer that 
weights these features correctly, and finally, our classifier. In my code below, the 
<code>CountVectorizer</code> tokenizes our texts and models each text as a vector with the 
frequencies of its tokens. The <code>TfidfTransformer</code> converts these frequencies to the more 
informative tf-idf weights, before the classifier builds a classification model. Training an SVM 
instead of a Multinomial Naive Bayes model is as simple as replacing <code>MultinomialNB</code> with 
<code>LinearSVC</code> on line three; extending the features with bigrams can be done by setting 
the <code>CountVectorizer</code>'s <code>ngram_range</code> to <code>(1, 2)</code>. The 
<code>fit</code> command on line four trains a model by sending the training data through the 
pipeline; the <code>predict</code> command uses the trained model to classify the test data. Finally, we measure 
the accuracy by checking how often the predicted label is the same as the test label.</p>

<div class="highlight">
text_clf = Pipeline([('vectorizer', CountVectorizer()),<br>
                   &nbsp;&nbsp;&nbsp;&nbsp;('transformer', TfidfTransformer()),<br>
                   &nbsp;&nbsp;&nbsp;&nbsp;('classifier', MultinomialNB())])<br/>
text_clf.fit(data["train"]["texts"],data["train"]["labels"])<br/>
predicted_labels = text_clf.predict(data["test"]["texts"])<br/>
print(np.mean(predicted_labels == data["test"]["labels"]))<br/>
</div>

<p>Compared to FastText, Scikit-learn is painfully slow. For example, training an SVM on the 
unigrams and bigrams of the Sogou corpus takes about 17 minutes on my laptop, compared to 4 minutes 
for FastText. It also requires several times more memory. The models can be made significantly 
smaller and faster by setting a minimum frequency for their features, but when you’re dealing with 
lots of data, speed and memory usage can become a concern.</p>

<p>In terms of accuracy, however, there is much less difference between the two solutions. In fact, 
I obtained a slightly higher accuracy with Scikit-learn than with FastText: when it is trained on 
unigrams and bigrams, its SVM classifies 92.8% of the AG test examples correctly (FastText: 91.7%, 
92.5% in the paper), and 97.1% of the Sogou examples (FastText: 95.5%, 96.8% in the paper). As 
expected, the Naive Bayes classifier does less well, with an accuracy of 90.8% for AG, and 90.2% 
for Sogou.</p>

<p>In summary, Scikit-learn is a really versatile library that allows you to quickly experiment 
with many different models and parameter settings. The more advanced of these often obtain a very 
good performance. However, out of the box it’s less suitable for modelling large data sets than 
FastText.</p>

<h3>The cloud solution: MonkeyLearn</h3>

<p>A third simple approach to text classification is to use an online API. While 
most available services have pre-trained models for traditional NLP tasks such as named
entity recognition, sentiment analysis, and general text classification, some of them
allow people to train their own models and hopefully achieve a better accuracy on
domain-specific data. One of these is <a 
href="http://www.monkeylearn.com/">MonkeyLearn</a>.</p>

<p>One of the main selling points of MonkeyLearn is its user-friendliness. Three dialogue windows 
help us set up a text classification model. During this process, we tell the system we would like to 
train a classifier, that this classifier should categorize texts by topic, and that our texts are 
news articles in English (or Chinese for Sogou). These settings help Monkeylearn choose the best 
pre-processing steps (filtering, emoticon normalization, etc.) and select the best combination of 
parameters for its models. When this is done, it creates an environment where we can 
manipulate the parameters of our model, train it and explore its performance.</p>

<img class="padded" src="https://www.dropbox.com/s/ydxzd31qjc30i8e/Screenshot%202016-08-23%2010.10.38.png?raw=1">

<p>Before we can train our model, we need to upload the training data, either as a csv or Excel 
file, or as text data, through a simple API call. Unfortunately, there’s a pretty strict limit on the 
number of training data MonkeyLearn accepts. The free plan allows for 3,000 training examples, the 
Ultra Gorilla for 40,000, and the Enterprise plan is made to measure. These limits may not be an issue when 
there is little tagged data for your problem, but in this age of big data they do feel a bit 
restrictive. Luckily the folks behind MonkeyLearn were friendly enough to give me access to the Ultra 
Gorilla plan for a few weeks. As a result, I chose to work with 40,000 training examples for the AG corpus,
and 20,000 for Sogou.</p>

<p>While the MonkeyLearn user interface is particularly attractive for newcomers to NLP, more 
experienced users still have the possibility to tweak the main parameters of their classifier. 
These include the type of model (Naive Bayes or SVM), the type of features (unigrams, bigrams, 
trigrams, or a combination of these), the maximum number of features (the default is 10,000), 
the stopwords, etc. There’s also a reference page that explains what these 
parameters mean.</p>

<img class="padded" src="https://www.dropbox.com/s/45p69ndljdfea3t/Screenshot%202016-09-18%2010.47.17.png?raw=1">

<p>Building a classifier takes a considerable amount of time. For example, MonkeyLearn needs around
20 minutes to train an SVM on the unigrams and bigrams in 20,000 Sogou news texts. Scikit-learn does
that in just 41 seconds on my laptop. However, our patience is rewarded with a
nice overview of our model and its performance: its accuracy, the keywords in our text data, and a 
really useful confusion matrix that helps us analyze what mistakes the classifier tends to make. 
These statistics are based on 4-fold cross-validation on the training data, which explains the longer 
training times at least partially.</p>

<img class="padded" src="https://www.dropbox.com/s/1ardbntka29w46l/Screenshot%202016-08-27%2015.42.19.png?raw=1">

<p>In order to test the trained model on our test set, we can make use of another of 
MonkeyLearn’s distinguishing features: all trained classifiers are immediately available for 
production, through an API. As with Scikit-learn, we experimented with a Naive Bayes classifier 
and an SVM, both with and without bigrams. On the AG corpus, there is very little difference 
between these settings: all models achieve an accuracy between 87.7% and 88.0%. With the same 
training examples, our Scikit-learn models gave an accuracy of 89.3% (SVM, unigrams only), while
FastText achieved 88.6% (unigrams only). The Sogou corpus shows bigger differences. The 
best-performing MonkeyLearn model is the SVM with unigrams and bigrams, with an accuracy of 
94.2%. This is about the same as a Scikit-learn SVM trained on the same examples (94.8%), and 
considerably better than FastText, which scores 89.1% with unigrams, and 86.8% with ungirams and 
bigrams.</p>

<p>MonkeyLearn takes away some of the pains of developing a classifier: it assists users in the 
training process, helps them evaluate their models, and immediately exposes their classifiers through 
an API. In terms of accuracy, it’s in the same ballpark as the other approaches we’ve tested
here, but training takes a while and using large data sets can become expensive.</p>

<h3>Conclusion</h3>

<p>As machine learning and AI grow in popularity, tackling basic NLP tasks is getting easier and 
easier. The wealth of available options means NLP practitioners are now often spoilt for choice. 
While comparing the accuracy of the models may be a logical first step, it did not reveal a clear 
winner among the three approaches to text classification I tested here. This is true in particular because 
I didn’t do any parameter optimization, and some approaches may have performed better with other 
parameter settings.</p>

<p>This means other factors will be decisive when you weigh up the different software options. If 
you’re working with big data sets, and speed and low memory usage are crucial, FastText looks 
like a good choice. The figures I obtained with 20,000 examples do suggest it works less well with small data sets, however. If 
you need a flexible framework that allows you to compare a wide range of machine learning models 
quickly, Scikit-learn will help you out. Its best classifiers can give great performance on both 
small and large data sets, but training large models can take some time. If you’re relatively new 
to NLP and looking for a text classifier that’s easy to build and integrate, MonkeyLearn is worth 
checking out. Keep in mind it’s less suitable for big-data problems and you give up some control.</p>

<p>There’s no doubt there are many more possibilities than the three 
frameworks I’ve explored here. As new options come along frequently,
we’ll be faced with the agony of choice for quite some time to come.	 
Still, a few simple experiments can go a long way to help you take your 
pick.</p>

