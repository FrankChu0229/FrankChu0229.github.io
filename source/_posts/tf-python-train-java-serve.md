title: TF Python Train Java Serve
date: 2018-08-02 10:30:59
tags: [python, keras, dl, tensorflow]
categories: Machine Learning
description: Summary for python offline train and java online serve using keras and tensorflow.
---

## Python Model 

Convert model to frozen graph with tf, frozen graph cannot be trained again and usually used in production environment.


### 1. TF: Add Name For Each Placeholder
```
in_question = tf.placeholder(tf.int32, [None, None], name='in_question')  # shape: (batch x seq)
in_answer = tf.placeholder(tf.int32, [None, None], name='in_answer')
in_question_len = tf.placeholder(tf.int32, [None], name='in_question_len')
in_answer_len = tf.placeholder(tf.int32, [None], name='in_answer_len')
dropout_rate = tf.placeholder(tf.float32, None, name='dropout_rate')
y_prob = tf.nn.softmax(logits, name='y_pred')
```

### 2. Train and Save the Model with SavedModelBuilder

```
builder = tf.saved_model.builder.SavedModelBuilder(saved_model_dir)
# x 为输入tensor, keep_prob为dropout的prob tensor
inputs = {'in_question': tf.saved_model.utils.build_tensor_info(self.in_question),
          'in_answer': tf.saved_model.utils.build_tensor_info(self.in_answer),
          'in_question_len': tf.saved_model.utils.build_tensor_info(self.in_question_len),
          'in_answer_len': tf.saved_model.utils.build_tensor_info(self.in_answer_len),
          'in_y': tf.saved_model.utils.build_tensor_info(self.in_y),
          'learning_rate': tf.saved_model.utils.build_tensor_info(self.learning_rate),
          'dropout_rate': tf.saved_model.utils.build_tensor_info(self.dropout_rate)}

outputs = {'y_p': tf.saved_model.utils.build_tensor_info(self.y_p)}

signature = tf.saved_model.signature_def_utils.build_signature_def(inputs, outputs, 'test_sig_name')

builder.add_meta_graph_and_variables(sess, ['baidu-model'], {'signature': signature})
builder.save()
```

### Choose the best model and convert it to frozen graph.

```
def convert_to_frozen_graph(path, tags, output_node_names, frozen_path='./',
                            frozen_graph_name='model.pb'):
    """
    To convert a saved model to a frozen graph. A saved model can be trained,
    however, a frozen graph cannot be trained again. The model used by java
    applications is a frozen graph.
    :param path: saved model path
    :param tags: saved model tags, set of string tags to identify the
    required MetaGraphDef.
    :param output_node_names: List of name strings for the result nodes
    of the graph.
    :param frozen_path: frozen graph save path
    :param frozen_graph_name: frozen graph model name
    :return: a frozen graph
    """
    with tf.Session(graph=tf.Graph()) as sess:
        meta_graph_def = tf.saved_model.loader.load(sess, tags, path)
        constant_graph = graph_util.convert_variables_to_constants(sess,
                                                                   sess.graph.as_graph_def(),
                                                                   output_node_names)

        with tf.gfile.GFile(frozen_path + frozen_graph_name, 'wb') as fw:
            fw.write(constant_graph.SerializeToString())
        print("%d ops in the final graph." % len(constant_graph.node))


def load_frozen_graph(path='./model.pb'):
    with tf.gfile.GFile(path, "rb") as f:
        graph_def = tf.GraphDef()
        graph_def.ParseFromString(f.read())

    # We load the graph_def in the default graph
    with tf.Graph().as_default() as graph:
        tf.import_graph_def(
            graph_def,
            input_map=None,
            return_elements=None,
            name="prefix",
            op_dict=None,
            producer_op_list=None
        )
    return graph

```

### Test the frozen graph

```
if __name__ == '__main__':
    convert_to_frozen_graph('../model/x.model', ['x-model'], ['y_pred'])

```

```
import tensorflow as tf
from .utils import load_frozen_graph

if __name__ == '__main__':
    graph = load_frozen_graph()
    for op in graph.get_operations():
        print(op.name, op.values())

    in_question = graph.get_tensor_by_name('prefix/in_question:0')
    in_question_len = graph.get_tensor_by_name('prefix/in_question_len:0')
    in_answer = graph.get_tensor_by_name('prefix/in_answer:0')
    in_answer_len = graph.get_tensor_by_name('prefix/in_answer_len:0')
    dropout_rate = graph.get_tensor_by_name('prefix/dropout_rate:0')
    y_pred = graph.get_tensor_by_name('prefix/y_pred:0')

    with tf.Session(graph=graph) as sess:
        res = sess.run(y_pred,
                       feed_dict={
                           in_question: [
                               [1935, 1198, 764, 657, 627, 456, 3114, 2964,
                                137, 2702, 1933, 2003, 634, 137, 643, 3021,
                                2003, 634, 137, 3177, 967, 2003, 2964, 137,
                                2729, 240, 2003, 2139, 873, 0, 0, 0, 0, 0, 0]],
                           in_answer: [
                               [2300, 2236, 1733, 1206, 2003, 2702, 1933, 456,
                                3114, 2003, 851, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
                                0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]],
                           in_question_len: [29],
                           in_answer_len: [11],
                           dropout_rate: 0
                       })
    print(res)

```

## Java Serve

```
inQuestion = "in_question";
inQuestionLen = "in_question_len";
inAnswer = "in_answer";
inAnswerLen = "in_answer_len";
dropoutRate = "dropout_rate";
predictSim = "y_pred";

List<Tensor<?>> res = this.session.runner()
    .feed(inQuestion, left.get(0)) // left, right are tensors created by tf
    .feed(inQuestionLen, left.get(1))
    .feed(inAnswer, right.get(0))
    .feed(inAnswerLen, right.get(1))
    .feed(dropoutRate, Tensor.create(0f))
    .fetch(predictSim)
    .run();
res.get(0).copyTo(result);
```

## Reference

- https://docs.google.com/presentation/d/e/2PACX-1vQ6DzxNTBrJo7K5P8t5_rBRGnyJoPUPBVOJR4ooHCwi4TlBFnIriFmI719rDNpcQzojqsV58aUqmBBx/pub?start=false&loop=false&delayms=3000&slide=id.g306175dd89_0_0
- https://blog.metaflow.fr/tensorflow-how-to-freeze-a-model-and-serve-it-with-a-python-api-d4f3596b3adc
- https://www.tensorflow.org/

