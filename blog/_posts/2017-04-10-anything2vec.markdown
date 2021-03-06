---
layout: post
title:  Anything2Vec, or How Word2Vec Conquered NLP
date:   2017-04-10 12:00:00
tags: NLP AI deep learning word embeddings paragraph topic document tweet2vec med2vec lda2vec
comments: true
---

<p class="first">
Word embeddings are one of the main drivers behind the success of deep learning in Natural Language Processing. Even technical 
people outside of NLP have often heard of word2vec and its uncanny ability to model the semantic relationship between 
a noun and its gender or the names of countries and their capitals. But the success of word2vec extends far beyond the 
word level. Inspired by <a href="https://github.com/MaxwellRebo/awesome-2vec">this list of word2vec-like models</a>, 
I set out to explore embedding methods for a broad variety of linguistic units &mdash; from sentences to tweets and medical concepts.
</p>

<p>
Although the idea of representing words as continuous vectors has been around for a long time, none of the previous approaches 
have been as successful as word2vec. Popularized in a <a href="https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf">series</a>
 <a href="http://www.aclweb.org/anthology/N13-1090">of</a>
 <a href="https://arxiv.org/abs/1301.3781">papers</a> by Mikolov and colleagues, word2vec offers two ways of 
training word embeddings: in the continuous bag-of-word (CBOW) model, the context words are used to predict the current word; 
in the skip-gram model, the current word is used to predict its context words. Because semantically similar words occur in 
similar contexts, the resulting embeddings successfully capture semantic properties of their words. Most famously, high-quality 
embeddings can (sometimes) answer analogy questions, such as “man is to king as woman is to _”. Indeed, the semantic (and syntactic) 
information that is captured in pre-trained word2vec embeddings has helped many deep learning models generalize beyond their small 
data sets. It is not surprising, then, that this framework has been so influential, both at the word level and beyond. While 
competitors such as <a href="https://nlp.stanford.edu/projects/glove/">GloVe</a> offer alternative ways of training word vectors, 
other models have tried to extend the word2vec approach to other linguistic units.
</p> 

<figure class="padded2">
<img width="450" src="https://www.dropbox.com/s/w0faljyphdohs8s/Screenshot%202017-04-08%2020.59.27.png?raw=1">
<figcaption>Sense2Vec operates on the level of word “senses” rather than simple words.</figcaption>
</figure>

<p>
One problem with the original word2vec model is that it maps every word to a single embedding. If a word has several senses, these 
are all encoded in the same vector. To address this problem, Trask and colleagues developed <a href="https://arxiv.org/abs/1511.06388">sense2vec</a>, an adaptation of word2vec 
that uses supervised labels to distinguish between senses. For example, in a corpus that has been labelled with parts of speech, 
<code>bank/noun</code> and <code>bank/verb</code> are treated as distinct tokens. Trask et al. show that downstream NLP tasks such as dependency parsing can 
benefit when word2vec operates on this “sense” level rather than the word level. Of course, depending on the type of information you choose
to model, “sense2vec” may or may not be a fitting name for this approach. Senses are more than just combinations of words and and their
parts of speech, but in the absence of large sense-tagged corpora, POS-tagged tokens can be a valid approximation.  
Either way, it is clear that performance on certain NLP tasks can get a boost from working with relevant, finer-grained units than simple words.
</p>

<p>
Some tasks require even more specific information about a word. Part-of-speech tagging, for instance, can benefit enormously from intra-word information that is encoded in smaller units such as morphemes. The adverbial suffix <em>-ly</em> in English is a good example. 
For this reason, many neural-network approaches operate at least partially on the character level. 
A good example is <a href="http://jmlr.csail.mit.edu/proceedings/papers/v32/santos14.pdf">Dos Santos and Zadrozny’s model for part-of-speech tagging</a>, which uses a convolutional network to extract character-level features.
However, character embeddings are usually trained in a supervised way. This sets them apart from word2vec embeddings, whose training procedure is fully unsupervised.
</p>

<figure class="padded2">
<img width="600" src="https://www.dropbox.com/s/yhelg3l8k43lqum/Screenshot%202017-04-07%2021.47.30.png?raw=1">
<figcaption>Skip-thought vectors are trained by having a sentence predict the previous and next sentences in a paragraph.</figcaption>
</figure>

<p>
Still, not all NLP tasks need such detailed information, and many of them focus on units larger than single words. <a href="https://arxiv.org/abs/1506.06726">Kiros et al.</a> present a direct way of translating the word2vec model to the sentence level: instead of having a word predict its context, they have a sentence predict the sentences around it. Their so-called skip-thought vectors follow the encoder-decoder pattern that is so pervasive in Machine Translation nowadays. First a recursive neural network (RNN) encoder maps the input sentence to a sentence vector, and then another RNN decodes this vector to produce the next (or previous) sentence. Through a series of eight tasks such as paraphrase detection and text classification, this skip-thought framework proves to be a robust way of modelling the semantic content of sentences. 
</p>

<figure class="padded2">
<img width="600" src="https://www.dropbox.com/s/qyjrysvx35ays24/Screenshot%202017-04-07%2022.22.23.png?raw=1">
<figcaption width="500">Tweet2Vec produces a character-based embedding of tweets.</figcaption>
</figure>

<p>
Unfortunately, sentences are not always as well-behaved as those modelled by Kiros et al. Social media content in particular is a hard beast to tame. Tweets, for example, do not combine into paragraphs and are riddled by slang, misspellings and special characters. That’s why <a href="https://arxiv.org/abs/1605.03481">tweet2vec, a model developed by Dhingra and colleagues</a> uses a character-based rather than a word-based network. First, a bidirectional Gated Recurrent Unit does a forward and backward pass over all the characters in the tweet. Then the two final states combine to a tweet embedding, and this embedding is projected to a softmax output layer. Dhingra et al. train their network to predict hashtags. The idea is that tweets with the same hashtag should have more or less semantically similar content. Their results show that tweet2vec indeed outperforms a word-based model significantly on hashtag prediction, particularly when the tweets in question contain many rare words. 
</p>

<figure class="padded2">
<img width="450" src="https://www.dropbox.com/s/1iig0ptbzuona1w/Screenshot%202017-04-08%2008.59.11.png?raw=1">
<figcaption width="500">Doc2Vec combines document and word embeddings in a single model.</figcaption>
</figure>

<p>
Whether they are word-based or character-based, the sentence-modelling methods above require all input sentences to have the same length (in words or characters). This becomes impractical when you’re dealing with longer paragraphs or documents that vary in length considerably. For this type of data, there is <a href="https://cs.stanford.edu/~quocle/paragraph_vector.pdf">doc2vec, an approach by Le and Mikolov</a> that models variable-length text sequences such as sentences, paragraphs, or full documents. It builds embeddings for both documents and words, and concatenates these embeddings to predict the next word in a sliding-window context. For example, in the sliding window <em>the cat sat on</em>, the document vector would be concatenated with the word vectors for <em>the cat sat</em> to predict the next word <em>on</em>. The document vector is unique to each document; the word vectors are shared between documents. Compared to its competitors, doc2vec has some unique advantages: it takes word order into account, generalizes to longer documents, and can learn from unlabelled data. When the resulting document vectors are used in downstream tasks such as sentiment analysis, they prove very competitive indeed. 
</p>

<p>
But why stop at paragraphs or documents? The next victim that has fallen prey to the word2vec framework is topic modelling. Traditional topic models such as Latent Dirichlet Allocation do not take advantage of distributed word representations, which could help them model semantic similarity between words. <a href="https://arxiv.org/abs/1605.02019">Moody’s lda2vec</a> aims to cure this by embedding word, topic and document vectors into the same space. His method owes a lot to word2vec, but in lda2vec the vector for a context word is obtained by summing the word vector with the vector of the document in which the word occurs. In order to obtain LDA-like topic distributions, these document vectors are defined as a weighted sum of topic vectors, where the weights are sparse, non-negative and sum to one. Moody’s experiments show that lda2vec produces semantically coherent topics, but unfortunately his paper does not offer an explicit comparison with LDA topics. 
</p>

<figure class="padded2">
<img width="550" src="https://www.dropbox.com/s/36awrn287o6hz15/Screenshot%202017-04-07%2022.04.20.png?raw=1">
<figcaption width="500">Topic2Vec produces both word and topic embeddings, based on LDA topic assignments.</figcaption>
</figure>

<p>
<a href="https://arxiv.org/abs/1506.08422">Niu and Dai's topic2vec</a> is even more similar to word2vec. In the CBOW setting, the context words are used to predict both a word and topic vector; in the skip-gram setting, these two vectors themselves predict the context words. Niu and Dai argue that their topics are more semantically coherent than those produced by LDA, but because they only give a few examples, their argument feels rather anecdotic. Moreover, topic2vec still depends on LDA, as the topic assignments for each word in the corpus are required to train the topic vectors. When it comes to word2vec-like topic embeddings, I’m still to be convinced.
</p>


<p>
After their conquest of classic NLP tasks, it’s no surprise embedding methods have also found their way to specialized disciplines where large volumes of text abound. One good example is <a href="https://arxiv.org/abs/1602.05568">med2vec</a>, an adaptation of word2vec to the medical domain. Choi and colleagues present a neural network that learns embeddings for medical codes (diagnoses, medications and procedures) and patient visits. Their method differs from word2vec in two crucial respects: it is explicitly modified to produce interpretable dimensions, and it is sensitive to the order of the patient visits. While the quality of the code embeddings in the evaluation is mixed, the embedding dimensions are clearly correlated with specific medical conditions. The visit embeddings in their turn prove quite effective at predicting the severity of the clinical risk and future medical codes. Med2vec is not the only example of embeddings in the medical domain: <a href="http://www.nature.com/articles/srep26094">Deep Patient</a> learns unsupervised representations of patients to predict their medical future on the basis of their electronic health records.
</p>

<p>
And the list doesn’t end there. In the domain of scientific literature, <a href="https://researchweb.iiit.ac.in/~soumyajit.ganguly/papers/A2v_1.pdf">author2vec</a> learns 
representations for authors by capturing both paper content and co-authorship, while <a href="https://arxiv.org/pdf/1703.06587.pdf">citation2vec</a> embeds papers 
by looking at their citations. And if we leave language for a moment, word2vec-like approaches have been applied to 
<a href="https://www.scribd.com/document/334578710/Person-2-Vec">people in a social network</a>, <a href="https://github.com/mattdennewitz/playlist-to-vec">playlists on Spotify</a>, 
<a href="https://github.com/warchildmd/game2vec">video games</a> and <a href="https://github.com/airalcorn2/batter-pitcher-2vec">Major League Baseball Players</a>. 
Not all of these applications are equally serious, but they all attest to the success of the word2vec paradigm.
</p>

<p>It’s clear word2vec embeddings are here to stay. Whether the framework is used to embed words or other linguistic units, the resulting vectors have played a huge role in the success of deep learning methods. At the same time, they still have clear limitations. Simple tokens may be relatively easy to embed, but word senses and topics are a different matter. How do we make the embedding dimensions more interpretable? And what can we gain from adding explicit linguistic information beyond word order? In these respects, embeddings are still in their infancy.</p>

