---
layout: post
title: Named Entity Recognition and the Road to Deep Learning
shorttitle: NER and the Road to Deep Learning
date:   2017-09-12 12:00:00
tags: NLP AI deep learning word embeddings named entity recognition
comments: true
author: Yves Peirsman
image: ner_model.png
---

<p class="first">
Not so very long ago, Natural Language Processing looked very different. In sequence labelling tasks such as 
Named Entity Recognition, Conditional Random Fields were the go-to model. The main challenge for NLP
engineers consisted in finding good features that captured their data well. Today, deep learning has replaced
 CRFs at the forefront of sequence labelling, and the focus has shifted from feature engineering to designing and implementing effective neural network
architectures. Still, the old and the new-style NLP are not diametrically opposed: just as it is possible
(and useful!) to incorporate neural-network features into a CRF, CRFs have influenced some of the best 
deep learning models for sequence labelling.
</p>

<p>
One of the most popular sequence labelling tasks is <a href="http://nlp.town/blog/nlp-api-ner/">Named Entity Recognition</a>, 
where the goal is to identify the names of entities in a sentence.
Named entities can be generic proper nouns that refer to locations, people or organizations,
but they can also be much more domain-specific, such as diseases or genes in biomedical NLP. A seminal task for Named Entity
Recognition was the <a href="https://www.clips.uantwerpen.be/conll2003/ner/">CoNLL-2003 shared task</a>, 
whose training, development and testing data are still often used
to compare the performance of different NER systems. In this blog post, we’ll rely on this data to help us answer a few questions
about how the standard approach to NER has evolved in the past few years: are neural networks really better than CRFs?
What components make up an effective deep learning architecture for NER? And in what way can CRFs and neural networks be combined?
</p> 

<p>
Read the rest of this article on the <a href="http://nlp.town/blog/ner-and-the-road-to-deep-learning/">NLP Town blog</a>.
</p>
