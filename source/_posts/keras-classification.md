title: Keras Classification
date: 2018-06-20 19:27:18
tags: [python, keras, dl]
categories: Machine Learning
description: Keras Classification.
---

## Code 

```
import numpy as np

from keras.datasets import mnist
from keras.utils import np_utils
from keras.models import Sequential
from keras.optimizers import RMSprop
from keras.layers import Dense, Activation
from keras.models import load_model


def create_dataset():
    (train_X, train_Y), (test_X, test_Y) = mnist.load_data()
    train_X = train_X.reshape(train_X.shape[0], -1) / 255  ## -1: 自动推断出维度
    test_X = test_X.reshape(test_X.shape[0], -1) / 255
    train_Y = np_utils.to_categorical(train_Y, num_classes=10)
    test_Y = np_utils.to_categorical(test_Y, num_classes=10)
    print(train_X[0].shape)
    print(train_Y[:3])
    return train_X, train_Y, test_X, test_Y


def build_model():
    model = Sequential()
    model.add(Dense(32, input_shape=(784,)))
    model.add(Activation('relu'))
    model.add(Dense(10))
    model.add(Activation('softmax'))
    rmsprop = RMSprop()  # can set learning rate 等参数值
    model.compile(optimizer=rmsprop, loss='categorical_crossentropy',
                  metrics=['accuracy'])
    return model


def train_test():
    print("------Training------")
    model = build_model()
    train_X, train_Y, test_X, test_Y = create_dataset()
    model.fit(train_X, train_Y, epochs=10, batch_size=32)
    loss, acc = model.evaluate(test_X, test_Y, batch_size=32)
    print("Test loss {} and acc {}".format(loss, acc))
    model.save('./classification.model')


def predict():
    model = load_model('./classification.model')
    train_X, train_Y, test_X, test_Y = create_dataset()
    pred_y = model.predict(test_X)
    print(pred_y)


if __name__ == '__main__':
    predict()


```


## Reference

- [keras manual](https://keras.io/zh/getting-started/sequential-model-guide/)
- [mofan keras tutorial](https://morvanzhou.github.io/tutorials/machine-learning/keras/2-2-classifier/)
