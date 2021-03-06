---
layout: post
title: Perplexed by Game of Thrones. A Song of N-Grams and Language Models  
shorttitle: A Song of N-Grams and Language Models
date:   2017-07-05 12:00:00
tags: NLP AI deep learning word embeddings n-grams language models neural networks
comments: true
---

<p class="first">
N-grams have long been part of the arsenal of every NLPer. These fixed-length word sequences are not only ubiquitous 
as features in NLP tasks such as text classification, but also formed the basis of the language models underlying machine translation 
and speech recognition. However, with the advent of recurrent neural networks and the diminishing role of feature selection in 
deep learning, their omnipresence could quickly become a thing of the past. Are we witnessing the end of n-grams in Natural Language Processing?
</p>

<p>
N-grams are sequences of <em>n</em> linguistic items, usually words. Depending on the length of these sequences, we call them 
unigrams, bigrams, trigrams, four-grams, five-grams, etc. N-grams have often been essential in developing high-quality 
NLP applications. Traditional models for sentiment analysis, 
for example, benefit immensely from n-gram features. To classify a sentence such 
as <em>I did not like this movie</em> as negative, it’s not sufficient to know that it contains the words <em>not</em> and <em>like</em> 
&mdash; after all, so does the positive sentence <em>What’s not to like about this movie</em>?
A sentiment model will have a much better chance of guessing the positive or negative nature of a sentence correctly when it relies on
 the bigrams or trigrams, such as <em>did not like</em>, in addition to the individual words. 
</p> 

<p>This benefit of n-grams is apparent in many other examples of text classification as well.
To illustrate this, let’s do a fun experiment. Let’s take a look at the television series <em>A Game of Thrones</em>, 
and investigate whether we can use the dialogues in the individual episodes to determine which book from George
R. R. Martin’s book series <em>A Song of Ice and Fire</em> they are based on. 
According to Wikipedia, the episodes in series 1 are all based on the first novel, <em>A Game of Thrones</em>,
series 2 covers the events in <em>A Clash of Kings</em>, series 3 and 4 are adapted from A <em>Storm of Swords</em>, and series 5 and
6 mix events from <em>A Feast for Crows</em> and <em>A Dance of Dragons</em>. It turns out we can induce this mapping 
pretty well on the basis of the dialogues in the episodes.</p>

<figure class="padded2">
<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<div id="sankey_basic" style="width: 900px; height: 300px;"></div>
<script async type="text/javascript">
      google.charts.load('current', {
        'packages': ['sankey']
      });
      google.charts.setOnLoadCallback(drawChart);

      function drawChart() {
        var data = new google.visualization.DataTable();
        data.addColumn('string', 'From');
        data.addColumn('string', 'To');
        data.addColumn('number', 'Weight');
        data.addRows([
          ['Series 1', 'A Game of Thrones', 10],
          ['Series 2', 'A Clash of Kings', 10],
          ['Series 3', 'A Storm of Swords', 1],
          ['Series 3', 'A Storm of Swords', 9],
          ['Series 4', 'A Storm of Swords', 9],
          ['Series 4', 'A Feast for Crows', 0],
          ['Series 4', 'A Dance with Dragons', 1],
          ['Series 5', 'A Storm of Swords', 2],
          ['Series 5', 'A Feast for Crows', 3],
          ['Series 5', 'A Dance with Dragons', 5],
          ['Series 6', 'A Feast for Crows', 2],
          ['Series 6', 'A Clash of Kings', 1],
          ['Series 6', 'A Storm of Swords', 3],
          ['Series 6', 'A Dance with Dragons', 4]
        ]);

        // Sets chart options.
        var options = {
          width: 600,
          sankey: {
            iterations: 0,
          }
        };

        // Instantiates and draws our chart, passing in some options.
        var chart = new google.visualization.Sankey(document.getElementById('sankey_basic'));
        chart.draw(data, options);
      }
</script>
</figure>

<p>For each episode and book, I collected the n-grams in a feature vector. I used Tf-Idf to weight the n-gram
frequencies and applied L2-normalization. For each episode, I then picked the book whose feature vector had the
highest cosine similarity with the episode’s vector. If we look at just the unigrams, 46 of the 60 episodes (76.67%)
are assigned to the correct book, according to the Wikipedia mapping. If we also take bigrams into account,
this figure climbs to 50 (83.33%). Adding trigrams takes us to 52 correct episodes (86.67%). This indicates
that the n-grams capture the similarities between the books and the episodes very well: random guessing would have only given
us a success ratio of 22%. The diagram above, which plots the mappings of the model with unigrams, bigrams and trigrams,
also suggests another interesting pattern: the later the television series, the more difficult it becomes to connect its episodes
to the correct book. While this is obviously just a toy example, this trend is in line with
<a href="https://www.pastemagazine.com/articles/2015/05/20-big-differences-between-the-game-of-thrones-tv.html">observations</a>
that with each <em>Game of Thrones</em> series, the screen adaptation has become <a href="http://www.vanityfair.com/hollywood/2016/06/game-of-thrones-season-6-debate">
less faithful to the books</a>.
</p>

<p>Apart from text classification, n-grams are most often used in the context of language models. Language models are models that can assign a probability
to a sequence of words, such as a sentence. This ability underpins many successful NLP applications, such as speech recognition, 
machine translation and text generation. In speech recognition, say, it is useful to know that the sentence <em>there is a hair in my soup</em>
is much more probable than <em>there is a air in my soup</em>. Traditionally, language models estimated these probabilities on the 
basis of the frequencies of the n-grams they had observed in a large corpus. A bigram model would 
look up the frequencies of the relevant bigrams, such as <em>a hair</em> and <em>a air</em>, in 
its training corpus, while a trigram model would rely on the frequencies of the three-word sequences,
such as <em>is a hair</em>. As the figures from the Google Books N-gram corpus below illustrate, 
these frequencies should normally
point towards a large preference for <em>there is a hair in my soup</em>. Even when the speaker does not
pronounce the first letter in <em>hair</em>, the language model will help ensure that the speech recognition
system gets the sentence right.
</p>

<iframe src="https://books.google.com/ngrams/interactive_chart?content=is+a+hair%2C+is+a+air&year_start=1800&year_end=2000&corpus=15&smoothing=3&share=&direct_url=t1%3B%2Cis%20a%20hair%3B%2Cc0%3B.t1%3B%2Cis%20a%20air%3B%2Cc0" width="600" height="275" marginwidth="0" marginheight="0" hspace="0" vspace="0" frameborder="0" scrolling="no"></iframe>

<p>
The predictions of language models are usually expressed in terms of <em>perplexity</em>. The perplexity of a particular model on a
sentence is the inverse of the probability it assigns to that sentence, normalized for sentence length. Intuitively, the perplexity expresses
how confused the model is by the words in the sentence: a perplexity of 50 means that the model behaves
as if it were randomly picking a word from 50 candidates at each step. 
The lower the perplexity, the better the language model is at predicting the text.</p>

<p>Let’s return for a minute to the increasing difficulty of linking the <em>Game of Thrones</em> episodes in the later 
series to the books.
If these newer episodes are indeed less faithful to their source material than earlier ones, this should be reflected in a rising perplexity of a language model trained
on the books. To verify this, I trained a bunch of language models on all books in the <em>Song of Ice and Fire</em> series, using the KenLM toolkit.
<a href="https://kheafield.com/papers/avenue/kenlm.pdf">KenLM</a> implements n-gram language models that are equipped with some advanced smoothing
techniques to deal with word sequences they have never seen in the training corpus. 
The results in the figure below confirm our expectations: the median perplexity of the four-gram 
language model on the sentences in the episodes climbs with every series. While on average,
the median sentence perplexity in the first series is 38, it increases to 48 in the second and
third series, 52 in the fourth series, 56 in the fifth and 57 in the sixth. This
pattern is repeated by the other language models I trained (from bigram to 5-gram models). Clearly, n-gram language models
have a harder time predicting the dialogues in the later episodes from the language in the books.
</p>

<iframe width="700" height="370" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1Tir_tqVJLQIr9nY868y1dYAwPgM33qSRQbMzR6jFkgU/pubchart?oid=1201461159&amp;format=interactive"></iframe>

<p>
As with so many traditional NLP approaches, in recent years n-gram based language models have been outshone by their neural 
counterparts. Because neural networks rely on distributed representations that capture the meaning of words 
(the so-called word embeddings), they are much better at dealing 
with unseen word sequences. For example, an n-gram model may not be able to predict that <em>pitcher of beer</em> is more likely 
than <em>pitcher of cats</em> if it has never seen these trigrams in the training corpus. This is true even when the training corpus 
contains many examples of <em>glass of beer</em> or <em>bottle of beer</em>, but no examples of <em>glass of cats</em> or <em>bottle of cats</em>. 
A neural language model, by contrast, will typically have learnt that <em>pitcher</em>, <em>glass</em> and <em>bottle</em> have similar meanings, 
and it will use this information to make the right prediction. These dense word embeddings also have a convenient side effect: neural language models 
take up much less memory than n-gram models.
</p>

<p>
The first neural language models were still heavily indebted to their predecessors, as they applied neural networks to n-grams.
<a href="http://www.jmlr.org/papers/volume3/bengio03a/bengio03a.pdf">Bengio et al.’s (2003)</a> seminal approach learnt word embeddings for all words in the n-gram, along with the probability of the word sequence. Later models, 
such as those by <a href="http://www.fit.vutbr.cz/research/groups/speech/publi/2010/mikolov_interspeech2010_IS100722.pdf">Mikolov et al. (2010)</a> 
and <a href="http://www.quaero.org/media/files/bibliographie/sundermeyer_lstm_neural_interspeech2012.pdf">Sundermeyer et al. (2013)</a> 
did away with the n-grams altogether and introduced recurrent neural networks (RNNs) to the task of language modelling. These RNNs, and Long Short-Term Memories in particular,
not only learn word embeddings, but are also able to model dependencies that exceed the boundaries of traditional n-grams.
Even more recently, other neural architectures have been applied to language modelling, such as 
<a href="https://arxiv.org/pdf/1612.08083.pdf">convolutional networks</a> and 
<a href="https://arxiv.org/pdf/1702.04521.pdf">RNNs with attention</a>. Teams at 
<a href="https://arxiv.org/pdf/1602.02410.pdf">Google</a>, 
<a href="https://research.fb.com/building-an-efficient-neural-language-model-over-a-billion-words/">Facebook</a> 
and other places seem to have got 
caught up in a race for ever-lower perplexities. 
</p>

<figure>
<img width="500" src="https://www.dropbox.com/s/0z4jq6jo20io24g/Screenshot%202017-07-04%2023.38.09.png?raw=1">
<figcaption>RNN-based language models learn word embeddings to make predictions. Adapted from the <a href="http://web.stanford.edu/class/cs224n/lectures/cs224n-2017-lecture8.pdf">Stanford CS224n course slides</a>.</figcaption>
</figure>

<p>There are quite a few toolkits for training and testing neural language models. <a href="https://nlg.isi.edu/software/nplm/">NPLM</a>
implements Bengio’s original feed-forward neural language models, <a href="https://www-i6.informatik.rwth-aachen.de/web/Software/rwthlm.php">RWTHLM</a>
adds the later RNN and LSTM architectures, while <a href="https://github.com/senarvi/theanolm">TheanoLM</a> lets you customize the networks more easily.
It’s also fairly straightforward to build your own language model in more generic deep learning libraries such as
<a href="https://www.tensorflow.org/tutorials/recurrent">Tensorflow</a>, which is what I did for this blog post. 
There’s a simple example of an LSTM language
model in the <a href="https://github.com/tensorflow/models/blob/master/tutorials/rnn/ptb/ptb_word_lm.py">Github Tensorflow repo.</a></p>

<p>
I repeated the experiment above with an LSTM-based language model that I trained for ten epochs, using a dimensionality of 256 for the word embeddings
and the LSTM cell. Ideally, we’d like to have more data than just the sentences in George R. R. Martin’s books to train such a model, but 
the results are nevertheless interesting. First of all, the median sentence
perplexities of each episode confirm our earlier findings: with each series, the dialogues in <em>Game of Thrones</em> get more difficult to predict. Second, the perplexities of our neural language model are generally lower than the ones of the four-gram language model.
This is true in particular for the later series: while the median sentence perplexities of both models average around 38 in the first series,
the last series shows more diverging results: the four-gram language model has a median sentence perplexity of 58 on average, while the LSTM 
model has only 49. This difference showcases the strength of neural language models &mdash; a result of the semantic knowledge they store in their
word embeddings and their ability to look beyond n-gram boundaries.
</p>

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1Tir_tqVJLQIr9nY868y1dYAwPgM33qSRQbMzR6jFkgU/pubchart?oid=1561749522&amp;format=interactive"></iframe>

<p>N-grams will not disappear from NLP any time soon. For simple applications, it’s often more convenient to train an n-gram model
rather than a neural network. Doing so will also save you a lot of time: training an n-gram model on the data above is a matter of seconds, while an 
LSTM can easily need a few hours. Still, when you have the data and the time, neural network models offer some compelling advantages
over their n-gram competitors, such as
their ability to model semantic similarity and long-distance dependencies. 
These days both types of model should be in your toolkit when you’re doing text classification, need to predict the most likely
word in a sentence, or are just having a bit of fun with books and television series.
</p>
