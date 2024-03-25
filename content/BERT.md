---
date created: Tuesday, January 9th 2024, 23:42:15
date modified: Monday, March 4th 2024, 23:37:27
tags:
  - transformers
---
> [!tip] BERT is the product of training technique applied to the [[transformers]] architecture.

# Masked LM

- 80% of the tokens are actually replaced with the token `[MASK]`.
- 10% of the time tokens are replaced with a random token.
- 10% of the time tokens are left unchanged.

# Next Sentence Prediction

To predict if the second sentence is indeed connected to the first, the following steps are performed:

1. The entire input sequence goes through the transformers model.
2. The output of the `[CLS]` token is transformed into a 2×1 shaped vector, using a simple classification layer (learned matrices of weights and biases).
3. Calculating the probability of IsNextSequence with `softmax`.

> [!quote] When training the BERT model, **Masked LM** and **Next Sentence Prediction** are trained together, with the goal of minimizing the combined loss function of the two strategies.

> [!todo] BERT combined loss function detail

# Different Tasks

> [!success] 
> BERT can be used for a wide variety of language tasks, while only adding a small layer to the core model.

## Sentiment Analysis

Classification tasks such as sentiment analysis are done similarly to Next Sentence classification, by adding a classification layer on top of the Transformer output for the [CLS] token.

## Question Answering

In Question Answering tasks (e.g. SQuAD v1.1), the software receives a question regarding a text sequence and is required to mark the answer in the sequence. Using BERT, a Q&A model can be trained by learning two extra vectors that mark the beginning and the end of the answer.

## Named Entity Recognition

In Named Entity Recognition (NER), the software receives a text sequence and is required to mark the various types of entities (Person, Organization, Date, etc) that appear in the text. Using BERT, a NER model can be trained by feeding the output vector of each token into a classification layer that predicts the NER label.

# Reference

- [BERT Explained: State of the art language model for NLP](https://towardsdatascience.com/bert-explained-state-of-the-art-language-model-for-nlp-f8b21a9b6270)
- [BERT Explained: A Complete Guide with Theory and Tutorial](https://medium.com/@samia.khalid/bert-explained-a-complete-guide-with-theory-and-tutorial-3ac9ebc8fa7c)
