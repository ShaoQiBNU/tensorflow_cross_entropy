tensorflow中的交叉熵损失函数
==========================

# tensorflow中的交叉熵函数有以下三种：

```
tf.nn.softmax_cross_entropy_with_logits

tf.nn.softmax_cross_entropy_with_logits_v2

tf.nn.sparse_softmax_cross_entropy_with_logits

```

## tf.nn.softmax_cross_entropy_with_logits和tf.nn.softmax_cross_entropy_with_logits_v2的labels的输入都是标签经过one-hot编码的向量，即[1,0,0]、[0,1,0]和[0,0,1]。tf.nn.sparse_softmax_cross_entropy_with_logits的labels的输入是实际的标签，即0、1和2。

## 这3个函数输入中的logits都不是softmax或sigmoid的输出，而是softmax或sigmoid函数的输入，因为它在函数内部进行sigmoid或softmax操作。因此在应用交叉熵函数时，要注意logits的赋值。

## tf.nn.softmax_cross_entropy_with_logits和tf.nn.softmax_cross_entropy_with_logits_v2唯一的区别在于tf.nn.softmax_cross_entropy_with_logits在进行反向传播的时候，只对logits进行反向传播，labels保持不变。而tf.nn.softmax_cross_entropy_with_logits_v2在进行反向传播的时候，同时对logits和labels都进行反向传播，常应用在生成对抗生成网络（GAN）等需要改变label的实例中。

## 另外，tf.nn.softmax_cross_entropy_with_logits马上会被弃用，所以可以采用tf.nn.softmax_cross_entropy_with_logits_v2，由于普通的深度学习网络中，labels不需要进行反向传播，所以先将labels的tensor经过tf.stop_gradient，然后传入tf.nn.softmax_cross_entropy_with_logits_v2，此时tf.nn.softmax_cross_entropy_with_logits_v2的作用和tf.nn.softmax_cross_entropy_with_logits一样。具体代码见：https://github.com/ShaoQiBNU/GoogleNet/blob/master/Inception_v2_MNIST.py.
