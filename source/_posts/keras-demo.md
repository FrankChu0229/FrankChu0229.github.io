title: Keras Demo Linear Regression
date: 2018-06-06 14:36:41
tags: [python, keras, dl]
categories: Machine Learning
description: Keras Linear Regression.
---

## Code

```
import numpy as np

np.random.seed(9999)
import matplotlib.pyplot as plt
from keras.models import Sequential, load_model
from keras.layers import Dense, Activation


def build_model():
    model = Sequential()
    model.add(Dense(1, input_shape=(1,)))
    model.compile(loss='mse', optimizer='sgd')
    return model


def create_data():
    X = np.linspace(-1, 1, 200)
    np.random.shuffle(X)
    noise = np.random.normal(0, 0.5, (200,))
    Y = 0.5 * X + 2 + noise
    # plt.scatter(X, Y)
    # plt.show(block=False)
    # print('finished.')
    return X, Y


def create_train_test():
    X, Y = create_data()
    train_X, train_Y = X[:int(len(X) * 0.8)], Y[:int(len(Y) * 0.8)]
    test_X, test_Y = X[int(len(X) * 0.8):], Y[int(len(Y) * 0.8):]
    return train_X, train_Y, test_X, test_Y


def train_test():
    model = build_model()
    train_X, train_Y, test_X, test_Y = create_train_test()
    model.fit(train_X, train_Y, epochs=50, batch_size=16)
    loss = model.evaluate(test_X, test_Y, batch_size=16)
    print('Loss is {}'.format(loss))
    W, b = model.layers[0].get_weights()
    print("Weight {}, bias {}".format(W, b))
    model.save('./linear_regression')


def predict():
    train_X, train_Y, test_X, test_Y = create_train_test()
    model = load_model('./linear_regression')
    y_pred = model.predict(test_X, batch_size=16)
    print(y_pred)
    plt.scatter(test_X, test_Y)
    plt.plot(test_X, y_pred)
    plt.xlabel("X axis")
    plt.ylabel("Y axis")
    plt.show()


if __name__ == '__main__':
    # train_test()
    predict()

```

## Reference 
- [mofan keras tutorial](https://morvanzhou.github.io/tutorials/machine-learning/keras/2-1-regressor/)
- [keras](https://keras.io/zh/)
