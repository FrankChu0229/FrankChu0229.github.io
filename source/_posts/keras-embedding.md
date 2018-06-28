title: Keras Embedding Summary
date: 2018-06-28 16:42:08
tags: [Machine Learning, nlp, keras, dl]
categories: Machine Learning
description: Keras preprocessing usage summary and loading pretrained word embeddings summary
---


## Summary

```
import re
from keras.preprocessing.text import text_to_word_sequence, hashing_trick
query = 'W17378 W09158 W03746 W03390'
print(text_to_word_sequence(query))
## ['w17378', 'w09158', 'w03746', 'w03390']

print(hashing_trick(query, 50))
## [23, 19, 32, 12]
print(hashing_trick(query, 20891, hash_function=lambda x: int(re.sub('W|w', '', x))))
## [17379, 9159, 3747, 3391]

```

```
from numpy import array
from keras.preprocessing.text import one_hot
from keras.preprocessing.sequence import pad_sequences
# define documents
docs = ['Well done!',
		'Good work',
		'Great effort',
		'nice work',
		'Excellent!',
		'Weak',
		'Poor effort!',
		'not good',
		'poor work',
		'Could have done better.']
# define class labels
labels = array([1,1,1,1,1,0,0,0,0,0])

vocab_size = 50
encoded_docs = [one_hot(d, vocab_size) for d in docs]
print(encoded_docs)
## [[7, 6], [8, 5], [36, 17], [10, 5], [5], [7], [39, 17], [11, 8], [39, 5], [20, 37, 6, 22]]

max_length = 4
padded_docs = pad_sequences(encoded_docs, maxlen=max_length, padding='post')
print(padded_docs)

## [[ 7  6  0  0]
##  [ 8  5  0  0]
##  [36 17  0  0]
##  [10  5  0  0]
##  [ 5  0  0  0]
##  [ 7  0  0  0]
##  [39 17  0  0]
##  [11  8  0  0]
##  [39  5  0  0]
##  [20 37  6 22]]

```

```
## Use pre-trained embeddings
from numpy import asarray, zeros
from keras.preprocessing.text import Tokenizer
from keras.preprocessing.sequence import pad_sequences
# define documents
docs = ['Well done!',
		'Good work',
		'Great effort',
		'nice work',
		'Excellent!',
		'Weak',
		'Poor effort!',
		'not good',
		'poor work',
		'Could have done better.']
# define class labels
labels = array([1,1,1,1,1,0,0,0,0,0])
t = Tokenizer()
t.fit_on_texts(docs)
print(t)
vocab_size = len(t.word_index) + 1
encoded_docs = t.texts_to_sequences(docs)
print(encoded_docs)
max_length = 4
padded_docs = pad_sequences(encoded_docs, maxlen=max_length, padding='post')
print(padded_docs)
print(t.word_index)

## loading the whold embedding into matrix
embeddings_index = dict()
embeddings_index['Well'] = asarray([1.04661322, -0.86488587, -0.32734334], dtype='float32')
embeddings_index['done'] = asarray([1.04661322, -0.86488587, 0.32734334], dtype='float32')
print(embeddings_index)
## embedding matrix:

embedding_matrix = zeros((vocab_size, 3))
for word, i in t.word_index.items():
    embeddings_vector = embeddings_index.get(word)
    if embeddings_vector is not None:
        embedding_matrix[i] = embeddings_vector
print(embedding_matrix)
```


## Reference
- [keras blog](https://blog.keras.io/using-pre-trained-word-embeddings-in-a-keras-model.html)
- https://machinelearningmastery.com/use-word-embedding-layers-deep-learning-keras/
