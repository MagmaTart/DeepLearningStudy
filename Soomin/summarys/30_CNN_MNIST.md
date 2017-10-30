# CNN : MNIST �з� ����
����, ������ MNIST �з��� CNN���� �����ϸ鼭 CNN�� ���� �����غ���. �ڵ�� [�輺�� �������� ���� �ڵ�](https://github.com/hunkim/DeepLearningZeroToAll/blob/master/lab-11-2-mnist_deep_cnn.py)�� �̿��Ͽ���.

����, �츮�� ������ MNIST �з� CNN ���� ������ ���� ����̴�.

![](../images/CNN51.png)

������� ����� Ǯ�� ������ �����ϴ� ���� ���̾ 3ȸ ��ģ �Ŀ�, �� �����͸� �ٽ� �� �켭 SOFTMAX �з��� ���� Fully-Connected Layer�� ���� ���̴�. �� SOFTMAX ������ ����� �̿��Ͽ� �з��� �����ϰ� �ȴ�.

���� MNIST �����͸� �ҷ� ����.

```
import tensorflow as tf
from tensorflow.examples.tutorials.mnist import input_data
tf.set_random_seed(9297)

mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)
```

������ MNIST �з��� �����ߴ� �Ͱ� ����, ��ġ �н��� ������ ���̴�. �н��� ���Ǵ� Hyper Parameter���� ���� �ش�.

```
learning_rate = 0.001
training_epochs = 15
batch_size = 100
keep_prob = 0.7
```

�н���, �н��� Epoch ��, �ѹ� ���� batch size, Dropout���� ������ �������� ���� �־���.

���� X �����͸� �����ͼ�, CNN ��Ʈ��ũ �ȿ� �� �� �ִ� 4���� ������ ���·� ������ش�. �� �������� `tf.reshape()` �޼ҵ带 Ȱ���Ѵ�. Y �����ʹ� ������� �����´�. one-hot vector�̱� ������ �������.

```
X = tf.placeholder(tf.float32)
X_img = tf.reshape(X, [-1, 28, 28, 1])    # Make 4-dimensional image
Y = tf.placeholder(tf.float32)
```

28 * 28�� 1���� �̹����� MNIST �����͸� (1 x 28 x 28) ũ���� 4���� �����ͷ� �ٲپ��ְ� �ִ�. �н��� ���� ��� �����ʹ� �� ���·� ��Ʈ��ũ�� �Էµȴ�.

���� ���� �Ű���� ������ �����̴�. ���� ���̾ �ϳ� �����ϸ�, ������ ���̾�� �ټ��� ũ�⸦ �����ϰ� �ڵ尡 �����ϹǷ� �� ���̾��� ����� �ڼ��� ���캸��.

ù ��° ���� ���̾��, ���ʹ� (3 x 3) ũ�⸦ ����� ���̰�, stride�� (1 x 1 x 1)�� ������ ���̴�. �׸��� ��� �����͸� 4�������� ������ֱ� ���ؼ� ���� ũ���� ���͸� 32�� ����� ���̴�. ���͸� �׷��� ������ش�.

```
W1 = tf.Variable(tf.random_normal([3, 3, 1, 32], stddev=0.01))          # 32 filters of [1, 3, 3]
```

�� ���͸� �̿��Ͽ� �Է� X �����Ϳ� ������� ������ �����Ѵ�. ������� ������ `tf.nn.conv2d()` �Լ��� �̿��Ͽ� �����Ѵ�.

```
Layer1 = tf.nn.conv2d(X_img, W1, strides=[1, 1, 1, 1], padding='SAME')  # Convolution with [1, 1] strides
```

4���� ���·� ���� �Է� �������� `X_img`�� ���͸� (1 x 1 x 1)�� stride�� ������� ������ �����Ѵ�. �׸��� padding�� �����ϴµ�, ���⼭�� �츮�� ���� �е��� ũ�⸦ �������� �ʰ�, Tensorflow���� �ñ�� �ִ�. `padding='SAME'`��, ������� ������ ����� �� ������ ����� �����ϵ��� �е��ش޶�� ��û�Ѵ�. MNIST�� �Է� �����Ͱ� (28 x 28) ũ���̹Ƿ�, `padding='SAME'`�� �����ϸ� ������� ���� �� �е��� ���� ��� Ư¡ ���� ũ�⵵ (28 x 28)�� �ȴ�.

�츮�� �̰� �̿��� �ټ��÷ο찡 �� ũ���� �е��� �����ϰ� �ִ��� ����� �� �ִ�. �е��� ũ�⸦ ![](https://latex.codecogs.com/gif.latex?x)�� ����, [���� ��](https://github.com/MagmaTart/DeepLearningStudy/blob/master/Soomin/summarys/29_CNN4.md)���� �˾ƺ��Ҵ� ��� Ư¡ �� ũ�� ��� �Ŀ� ����־ �е��� ũ�⸦ ���� �� �ִ�.

![](https://latex.codecogs.com/gif.latex?%5Cfrac%7B28%20&plus;%202x%20-%203%7D%7B1%7D%20&plus;%201%20%3D%2028) �̹Ƿ�,  ![](https://latex.codecogs.com/gif.latex?x%20%3D%201)�̴�. ���� �ټ��÷ο찡 �������ִ� �е��� ũ��� 1�̶�� ���� �� �� �ִ�.

�� �� Ȱ��ȭ �Լ��� ReLU�� �������ش�.

```
Layer1 = tf.nn.relu(Layer1)
```

�׸��� �̹����� ũ�⸦ ���̱� ����, Max Pooling�� �����Ѵ�. �ټ��÷ο쿡�� Max Pooling�� `tf.nn.max_pool()` �Լ��� �̿��� ������ �� �ִ�.

```
# Max pooling with [2, 2] size and [2, 2] strides
Layer1 = tf.nn.max_pool(Layer1, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding='SAME')
```

(2 x 2) �������� Ŀ�� ũ���, (2 x 2) stride�� �����̸鼭 Max Pooling�� �����ϰ� �ִ�. ���� Ǯ�� �� ��� Ư¡ ���� ũ��� (14 x 14)�� �ȴ�.

�̷��� �� ���̾�� ��� Ư¡ ���� ����� ����صξ�� �Ѵ�. �׷��� Fully Connected Layer�� ����� ���� �� �ֱ� �����̴�.

Ǯ���� ���� ����� ���� ���� ���̾ �Է��ϰ�, �� ����� ���� ���� ���̾ �Է��ϰ�... �ϸ鼭 �̹����� ũ��� �ٿ� ������ ������ �÷� ������. �׷��� 2��° ���� ���̾�� 3��° ���� ���̾ �׾� ����.

```
W2 = tf.Variable(tf.random_normal([3, 3, 32, 64], stddev=0.01))         # 64 filters of [32, 3, 3]
Layer2 = tf.nn.conv2d(Layer1, W2, strides=[1, 1, 1, 1], padding='SAME')
Layer2 = tf.nn.relu(Layer2)
Layer2 = tf.nn.max_pool(Layer2, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding='SAME')

W3 = tf.Variable(tf.random_normal([3, 3, 64, 128], stddev=0.01))        # 128 filters of [64, 3, 3]
Layer3 = tf.nn.conv2d(Layer2, W3, strides=[1, 1, 1, 1], padding='SAME')
Layer3 = tf.nn.relu(Layer3)
Layer3 = tf.nn.max_pool(Layer3, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding='SAME')
```

ù ��° ���� ���̾ 32���� ���͸� ����Ͽ� (32 x 3 x 3)�� ����� �� �������Ƿ�, �ι�° ���� ���̾���� (32, 3, 3) ũ���� ���͸� �������־�� �Ѵ�. �׸��� �̹����� ����� 64������ ����� �ִ�. ù ��° ���� ���̾��� ��� Ư¡ ���� ũ��� (64, 7, 7)�� �ȴ�.

�� ��° ���� ���̾ �����ϴ�. (64, 3, 3) ũ���� ���͸� �����ؼ� (128, 4, 4) ����� ��� Ư¡ ���� �����ϰ� �ִ�.

�׸��� �� ��° ���� ���̾��� ��� Ư¡ ���� 1�������� ������ Fully-Connected Layer�� ���� �ȴ�. 1�������� ��� �۾��� `tf.reshape()` �Լ��� �̿��Ѵ�.

```
Layer3_flat = tf.reshape(Layer3, [-1, 4 * 4 * 128])    # flat the output feature map to [1,  4 * 4 * 128] size
```

(128, 4, 4) ����� ��� Ư¡ ���� 1�������� ���־���. �׸��� �̰��� �״�� Fully-Connected Layer�� �־��ְ� �ȴ�. ������ ���� ���̴�.

```
W4 = tf.get_variable("W4", shape=[4*4*128, 625], initializer=tf.contrib.layers.xavier_initializer())   # Fully Connected Layer
b4 = tf.Variable(tf.random_normal([625]))
Layer4 = tf.matmul(Layer3_flat, W4) + b4
Layer4 = tf.nn.relu(Layer4)
Layer4 = tf.nn.dropout(Layer4, keep_prob=keep_prob)
```

Xavier Initializer�� ����ؼ� Weight�� �������ְ�, ����� ũ�Ⱑ �پ�� ��ŭ bias�� �����ϰ� �ִ�. ������ �Ϲ� Affine ����ó�� ������ ��, ReLU Ȱ��ȭ �Լ��� Dropout�� �������ְ� �ִ�. Fully-Connected Layer���� Overfitting�� ���� �� �Ͼ�Ƿ�, ���⿡�� ���� Dropout�� �ϳ� ȿ������ ������ ���ȴ�. `keep_prob`�� ���� 0.7���� �տ��� �������� �� �ִ�.

�׸��� 2��° Fully-Connected Layer���� ���� 10���� ��� ���͸� ������.

```
W5 = tf.get_variable("W5", shape=[625, 10], initializer=tf.contrib.layers.xavier_initializer())    # Fully Connected Layer
b5 = tf.Variable(tf.random_normal([10]))

logits = tf.matmul(Layer4, W5) + b5    # Logits function
```

10�� ���̸� ���� ��� ���͸� �����, ���� �Լ��� ������־���. ���� �� ������ �̿��Ͽ� cost �Լ��� �����ϰ� AdamOptimizer�� �̿��Ͽ� Ʈ���̴׸� ������ �ȴ�.

```
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(logits=logits, labels=Y))    # Softmax cost
trainer = tf.train.AdamOptimizer(0.001).minimize(cost)    # Use Adam optimizer
```

�� �ۿ����� ���� ���� CNN�� �������� ������ �ξ����Ƿ�, ���� �н��ϴ� �κ��� ������� �ʰڴ�. �� ��� �ڵ带 [�� ��]()�� �����ϰڴ�.

��Ȯ���� 99.38%�� �������� ���� 1% ���Ϸ� ������ ���� �� �� �־���. CNN�� �̹����� Ư���� �����ϴ� ������ ������ �ֱ� ������ ������ ��Ȯ���� ���� �� ����.

![](../image/CNN52.png)