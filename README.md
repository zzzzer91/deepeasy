# DeepEasy

纯 Numpy 实现各种基本的深度学习算法。

## 已实现

- Xavier Initializer
- mini batch
- Forward propagation
- Back propagation
- SGD
- Momentum
- RMSprop
- Adam
- Nadam
- inverted dropout
- Cross Entropy Cost
- Mean Squared Cost

## TODO

- 代码优化
- Batch Normalization
- L2 Regularization

## 安装

```python
python3 setup.py install
```


## 使用方法

### 基础

定义神经网络结构：

```python
nn_architecture: List[Dict] = [
    {   # 第一层
        'input_dim': int,  # 该层每个神经元被连接数
        'output_dim': int, # 该层神经元数
        'activation': str, # 该层激活函数，可选
        'dropout_keep_prob': float  # 该层 dropout 神经元保留概率，可选
    },
    {   # 第二层
        ...
    }
    ...
]
```

实例化对象，指定随机数种子 seed，保证每次随机初始化 weight 的值都相同，便于测试：

```python
from deepeasy.nnet import NeuralNetwork

nn_architecture = [
    {"input_dim": 2, "output_dim": 50, "activation": "relu"},
    {"input_dim": 50, "output_dim": 1, "activation": "sigmoid"},
]
nn = NeuralNetwork(nn_architecture, seed=100)
```

载入 Minst 数据集（需要提前下好，解压，放入同一个文件夹，文件名不能改动）：

```python
from deepeasy.datasets import load_minst

file_path = '/home/zzzzer/Documents/data/数据集/数字手写体/mnist/'
x_train, y_train, x_test, y_test = minst.load_minst(file_path)
# x_train.shape=(60000, 28, 28), y_train.shape=(60000,)
# x_test.shape=(10000, 28, 28), y_test.shape=(10000,)
```

查看某一张图片，及其标签：

```python
from PIL import Image

img_idx = 10
# 查看图片
Image.fromarray(x_test[img_idx].reshape(28, 28))
# 查看对应标签
y_test[img_idx]
```

开始训练：
```python
nn.train(x_train, y_train,
    epochs=100,
    batch_size=600,
    lr=0.01,
    gd_name='sgd'
)
```

画出 Cost、Accuracy 走势：
```python
nn.plot_history()
```

测试模型：
```python
nn.test_model(x_test, y_test)
```

