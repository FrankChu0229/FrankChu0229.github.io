title: Keras Siamese Network
date: 2018-09-02 10:11:21
tags: [python, keras, dl]
categories: Machine Learning
description: Keras Siamese Network.
---

## Introduction

孪生网络 (Siamese Network) 常用在matching等任务上，所谓孪生，是指左右两侧共用同一套网络。

## Implementation by Keras

Model Implementation:

```
# -*- coding: utf-8 -*-
# @Author: Frank Chu
# @Date: 2018-06-28

from __future__ import print_function
import numpy as np
from keras.preprocessing.sequence import pad_sequences
from keras.models import Model, load_model
from keras.layers import Dense, Input, Dropout, Embedding, GRU, Bidirectional, \
    Subtract, Multiply, concatenate, BatchNormalization, Add
from utils import preprocess, process_embedding_table
from keras.callbacks import CSVLogger
import tensorflow as tf
import keras.backend.tensorflow_backend as KTF


class SiameseMatcher:
    def __init__(self):
        self._word_vocab_size = 20892
        self._word_embed_size = 300
        self._word_sequence_max_length = 15
        self._char_vocab_size = 3048
        self._char_embed_size = 300
        self._char_sequence_max_length = 30

        self._word_embed_path = '../../dataset/word_embed.txt'
        self._char_embed_path = '../../dataset/char_embed.txt'
        self._word_corpus_path = './data/word_corpus.txt'
        self._char_corpus_path = './data/char_corpus.txt'
        self._train_word_data_path = './data/train_word_data.txt'
        self._test_word_data_path = './data/test_word_data.txt'
        self._train_char_data_path = './data/train_char_data.txt'
        self._test_char_data_path = './data/test_char_data.txt'
        self._train_word_query = None
        self._train_word_answer = None
        self._train_word_label = None
        self._test_word_query = None
        self._test_word_answer = None

        self._word_embed_table = None
        self._char_embed_table = None
        self._word_index = None
        self._char_index = None

        self._batch_size = 256
        self._epoch = 1
        self._validation_split = 0.15
        self._lstm_dim = 256

    def process_train(self):
        self._word_index, self._train_word_query, \
        self._train_word_answer, self._train_word_label = preprocess(
            self._word_corpus_path, self._train_word_data_path,
            self._word_sequence_max_length, mode='train'
        )
        self._word_embed_table = process_embedding_table(self._word_embed_path,
                                                         self._word_index)
        np.save('./data/word_index', self._word_index)
        np.save('./data/train_word_query', self._train_word_query)
        np.save('./data/train_word_answer', self._train_word_answer)
        np.save('./data/train_word_label', self._train_word_label)
        np.save('./data/word_embed_table', self._word_embed_table)
        print("Preprocess Finished.")

    def process_test(self):
        _index, self._test_word_query, \
        self._test_word_answer, _label = preprocess(
            self._word_corpus_path, self._test_word_data_path,
            self._word_sequence_max_length, mode='test'
        )
        np.save('./data/test_word_query', self._test_word_query)
        np.save('./data/test_word_answer', self._test_word_answer)

        print("Test Preprocess Finished.")

    def build_model(self):
        query_word_sequence_input = Input(
            shape=(self._word_sequence_max_length,))
        answer_word_sequence_input = Input(
            shape=(self._word_sequence_max_length,))

        word_embedding_layer = Embedding(self._word_vocab_size,
                                         self._word_embed_size,
                                         weights=[self._word_embed_table],
                                         input_length=self._word_sequence_max_length)
        bi_lstm_layer = Bidirectional(
            GRU(self._lstm_dim, dropout=0.4, recurrent_dropout=0.2,
                return_sequences=True))
        second_lstm_layer = Bidirectional(
            GRU(self._lstm_dim, dropout=0.4, recurrent_dropout=0.2))

        droupour_layer = Dropout(0.5)

        q1 = word_embedding_layer(query_word_sequence_input)
        q1 = droupour_layer(q1)
        q1 = bi_lstm_layer(q1)
        q1 = second_lstm_layer(q1)
        q2 = word_embedding_layer(answer_word_sequence_input)
        q2 = droupour_layer(q2)
        q2 = bi_lstm_layer(q2)
        q2 = second_lstm_layer(q2)
        q_diff = Subtract()([q1, q2])
        q_mul = Multiply()([q1, q2])
        q_add = Add()([q1, q2])
        merged = concatenate([q1, q2, q_add, q_diff, q_mul])

        merged = Dense(256, activation='relu')(merged)
        merged = BatchNormalization()(merged)
        merged = Dense(256, activation='relu')(merged)
        merged = BatchNormalization()(merged)
        merged = Dense(128, activation='relu')(merged)
        merged = BatchNormalization()(merged)
        prediction = Dense(2, activation='softmax')(merged)

        model = Model(
            inputs=[query_word_sequence_input, answer_word_sequence_input],
            outputs=prediction)
        model.compile(loss='categorical_crossentropy', optimizer='adam',
                      metrics=['accuracy'])
        return model

    def train(self):
        self._word_index = np.load('./data/word_index.npy')
        self._train_word_query = np.load('./data/train_word_query.npy')
        self._train_word_answer = np.load('./data/train_word_answer.npy')
        self._train_word_label = np.load('./data/train_word_label.npy')
        self._word_embed_table = np.load('./data/word_embed_table.npy')
        model = self.build_model()
        csv_logger = CSVLogger('log.csv', append=True, separator=';')
        model.fit([self._train_word_query, self._train_word_answer],
                  self._train_word_label,
                  batch_size=self._batch_size, epochs=self._epoch,
                  validation_split=self._validation_split,
                  callbacks=[csv_logger])
        model.save('./model')

    def train_all(self, model_path):
        """
        train the final model using train and dev data.
        :return:
        """
        self._word_index = np.load('./data/word_index.npy')
        self._train_word_query = np.load('./data/train_word_query.npy')
        self._train_word_answer = np.load('./data/train_word_answer.npy')
        self._train_word_label = np.load('./data/train_word_label.npy')
        self._word_embed_table = np.load('./data/word_embed_table.npy')
        model = load_model(model_path)
        model.fit([self._train_word_query, self._train_word_answer],
                  self._train_word_label,
                  batch_size=self._batch_size, epochs=self._epoch + 2)
        model.save('./final-model')

    def predict(self, model_path):
        self._test_word_query = np.load('./data/test_word_query.npy')
        self._test_word_answer = np.load('./data/test_word_answer.npy')
        model = load_model(model_path)
        prediction = model.predict(
            [self._test_word_query, self._test_word_answer],
            batch_size=512)
        print(prediction)
        np.save('./predictions', prediction)


if __name__ == '__main__':
    model = SiameseMatcher()
    # model.process_train()
    # model.process_test()
    # model.predict('/Users/frank/self/paipaiai/first-model-epoch-100')

    config = tf.ConfigProto()
    config.gpu_options.allow_growth = True  # 不全部占满显存, 按需分配
    session = tf.Session(config=config)
    # 设置session
    KTF.set_session(session)

    model.train()


```

Data Process Implementation:

```
# -*- coding: utf-8 -*-
# @Author: Frank Chu
# @Date: 2018-06-28

from keras.preprocessing.text import Tokenizer
from keras.preprocessing.sequence import pad_sequences
from keras.utils import to_categorical
from numpy import asarray, zeros
import numpy as np


def load_file(path):
    data = list()
    with open(path, 'rt', encoding='utf-8') as fo:
        for line in fo:
            data.append(line.strip())
    return data


def preprocess(corpus_path, data_path, max_length, mode='train'):
    corpus = load_file(corpus_path)
    tokenizer = Tokenizer(lower=False)
    tokenizer.fit_on_texts(corpus)
    print(tokenizer.word_index)
    data = load_file(data_path)
    label, query, answer = process_data_set(data, mode)
    encoded_query = tokenizer.texts_to_sequences(query)
    padded_query = pad_sequences(encoded_query, maxlen=max_length,
                                 padding='post')
    encoded_answer = tokenizer.texts_to_sequences(answer)
    padded_answer = pad_sequences(encoded_answer, maxlen=max_length,
                                  padding='post')
    if mode == 'train':
        label = to_categorical(asarray(label))

    return tokenizer.word_index, padded_query, padded_answer, label


def process_data_set(data, mode='train'):
    label = []
    query = []
    answer = []
    if mode == 'train':
        for line in data:
            splits = line.split("\t")
            label.append(splits[0])
            query.append(splits[1])
            answer.append(splits[2])
    else:
        for line in data:
            splits = line.split("\t")
            query.append(splits[0])
            answer.append(splits[1])
    return label, query, answer


def process_embedding_table(embedding_path, word_index):
    embedding_data = load_file(embedding_path)
    splits = embedding_data[0].split(' ')
    vocab, dim = splits[0], splits[1]
    print("vacab size {} and len of embedding data is {}".format(vocab,
                                                                 len(
                                                                     embedding_data)))
    print('word index dict size is {}'.format(len(word_index)))
    # assert vocab == len(word_index)
    embedding_index = dict()
    for line in embedding_data:
        values = line.split(' ')
        if len(values) == 2:
            continue
        word = values[0]
        vector = asarray(values[1:], dtype='float32')
        embedding_index[word] = vector

    embedding_matrix = zeros((int(vocab) + 1, int(dim)))
    for word, i in word_index.items():
        embedding_vector = embedding_index.get(word)
        if embedding_vector is not None:
            embedding_matrix[i] = embedding_vector
        else:
            print('word is {} and i is {}'.format(word, i))
    return embedding_matrix

```

## Reference 
- [keras](https://keras.io/zh/)
