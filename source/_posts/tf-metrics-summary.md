title: TF Metrics Summary
date: 2019-07-28 11:48:07
tags: [tensorflow, metrics, bert, summary]
categories: tensorflow
description: Tensorflow Metrics Summary. 
---

最近尝试将bert应用在生产环境中的分类任务上，由于bert中只带了`accuracy`的指标，因此使用`tf.metrics.precision` 和`tf.metrics.recall`将precision和recall指标加进来，但是这里算出来的precision和recall和sklearn中算出来的无论是micro还是macro都不一致，在这里被坑了一段时间，为此记录一下。


最开始尝试取看实现代码，发现代码看起来比较麻烦，因此还是自己动手造了一个例子，然后针对这个例子，通过

1. 手动计算micro-precision, micro-recall, micro-f1等指标
2. 借助sklearn api计算相关指标
3. 使用tf metrics计算相关指标

结果发现，手动计算指标和sklearn计算指标是一致的，最后将tf.metrics 代码拿出来运行发现，precision和recall的计算会首先将结果cast 到true和false，在取统计TP， TP和FP，FN等，因此这里只适合二分类任务的precision和recall计算，但是文档中没提及过这一点。

除了这个坑之外，tf.metrics 在使用时还要注意：

1. 初始化LOCAL_VARIABLES
2. 要先session.run(precision_OP)更新TP等LOCAL变量, 在run(precision)才会得到precision的值，具体可见reference中的链接



## 代码

以下是代码，tf版本1.14.0

``` python
#!/usr/bin/env python3
# encoding=utf8

from sklearn.metrics import f1_score
from sklearn.metrics import precision_score
from sklearn.metrics import recall_score
from sklearn.metrics import accuracy_score
from sklearn.metrics import accuracy_score
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix

import tensorflow as tf
from tensorflow.python.ops import math_ops
from tensorflow.python.framework import dtypes



def calculate_performance(predictions, label_ids):
    true_micro_precision = precision_score(label_ids, predictions,
                                           average='micro')
    true_macro_precision = precision_score(label_ids, predictions,
                                           average='macro')

    true_micro_recall = recall_score(label_ids, predictions, average='micro')
    true_macro_recall = recall_score(label_ids, predictions, average='macro')

    true_micro_f1 = f1_score(label_ids, predictions, average='micro')
    true_macro_f1 = f1_score(label_ids, predictions, average='macro')

    true_acc = accuracy_score(label_ids, predictions)

    confusion_matrix_report = confusion_matrix(label_ids, predictions)
    report = classification_report(label_ids, predictions)

    return true_micro_precision, true_macro_precision, true_micro_recall, true_macro_recall, \
           true_micro_f1, true_macro_f1, true_acc, confusion_matrix_report, report


if __name__ == '__main__':
    # tf.enable_eager_execution()

    predictions = [0, 2, 0, 2, 0, 0, 2, 1, 1, 1, 1, 0]
    gold_labels = [0, 1, 2, 2, 1, 0, 0, 0, 1, 2, 0, 1]

    true_micro_precision, true_macro_precision, true_micro_recall, true_macro_recall, \
    true_micro_f1, true_macro_f1, true_acc, confusion_matrix_report, report = calculate_performance(
        predictions, gold_labels)

    print(
        "|true_micro_precision|true_macro_precision|true_micro_recall|true_macro_recall|true_micro_f1|true_macro_f1|true_acc|")
    print("|{}|{}|{}|{}|{}|{}|{}|".format(true_micro_precision,
                                          true_macro_precision,
                                          true_micro_recall, true_macro_recall,
                                          true_micro_f1, true_macro_f1,
                                          true_acc))

    print(confusion_matrix_report)
    print(report)
    print(accuracy_score(gold_labels, predictions))

    predictions_tensor = tf.get_variable(name="predictions", initializer=tf.constant(predictions))
    gold_labels_tensor = tf.get_variable(name='gold_labels', initializer=tf.constant(gold_labels))
    print(predictions_tensor)
    print(gold_labels_tensor)

    tf_precision, precision_op = tf.metrics.precision(
        labels=gold_labels_tensor,
        predictions=predictions_tensor)
    tf_recall, recall_op = tf.metrics.recall(labels=gold_labels_tensor,
                                             predictions=predictions_tensor)
    tf_acc, acc_op = tf.metrics.accuracy(labels=gold_labels_tensor,
                                         predictions=predictions_tensor)

    tp, tp_op = tf.metrics.true_positives(labels=gold_labels_tensor,
                                          predictions=predictions_tensor)
    fp, fp_op = tf.metrics.false_positives(labels=gold_labels_tensor,
                                           predictions=predictions_tensor)
    fn, fn_op = tf.metrics.false_negatives(labels=gold_labels_tensor,
                                           predictions=predictions_tensor)
    tn, tn_op = tf.metrics.true_negatives(labels=gold_labels_tensor,
                                          predictions=predictions_tensor)
    print(tf_precision, precision_op)
    print(tf_recall, recall_op)
    print(tf_acc, acc_op)
    print(tp, tp_op)

    cast_predictions = math_ops.cast(predictions_tensor, dtype=dtypes.bool)
    cast_labels = math_ops.cast(gold_labels_tensor, dtype=dtypes.bool)

    with tf.Session() as sess:
        sess.run(tf.global_variables_initializer())
        sess.run(tf.local_variables_initializer())

        print(sess.run(precision_op))
        print(sess.run(tf_precision))

        print(sess.run(recall_op))
        print(sess.run(tf_recall))

        print(sess.run(acc_op))
        print(sess.run(tf_acc))

        print(sess.run(tp_op))
        print("TP", sess.run(tp))

        print(sess.run(fp_op))
        print("FP", sess.run(fp))

        print(sess.run(fn_op))
        print("FN", sess.run(fn))

        print(sess.run(tn_op))
        print("TN", sess.run(tn))

        print("Predis", sess.run(cast_predictions))
        print("Labels", sess.run(cast_labels))
```

# Reference

- [From zhihu](https://zhuanlan.zhihu.com/p/43359894)
- [From blog](https://www.zybuluo.com/Team/note/1254988)

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>


--- 




