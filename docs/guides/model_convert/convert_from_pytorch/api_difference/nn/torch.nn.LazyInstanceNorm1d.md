## [ paddle 参数更多 ]torch.nn.LazyInstanceNorm1d
### [torch.nn.LazyInstanceNorm1d](https://pytorch.org/docs/stable/generated/torch.nn.LazyInstanceNorm1d.html)

```python
torch.nn.LazyInstanceNorm1d(eps=1e-05, momentum=0.1, affine=True, track_running_stats=True, device=None, dtype=None)
```

### [paddle.nn.InstanceNorm1D](https://www.paddlepaddle.org.cn/documentation/docs/zh/develop/api/paddle/nn/InstanceNorm1D_cn.html#instancenorm1d)
```python
paddle.nn.InstanceNorm1D(num_features, epsilon=1e-05, momentum=0.9, weight_attr=None, bias_attr=None, data_format="NCL", name=None)
```

其中，Paddle 不支持 `num_features` 参数的延迟初始化，部分参数名不同，具体如下：
### 参数映射

| PyTorch       | PaddlePaddle | 备注                                                   |
| ------------- | ------------ | ------------------------------------------------------ |
| -             | num_features   | 表示输入 Tensor 通道数，PyTorch 无此参数，Paddle 需要根据实际输入 Tensor 的通道数进行设置。  |
| eps           | epsilon       | 为了数值稳定加在分母上的值，仅参数名不一致。             |
| momentum      | momentum      | 此值用于计算 moving_mean 和 moving_var, 值的大小 Paddle = 1 - PyTorch，需要转写。               |
| affine        | -             | 是否使用可学习的仿射参数，Paddle 无此参数，需要转写。可通过 weight_attr 和 bias_attr 控制。             |
| track_running_stats | -       | 是否跟踪运行时的 mean 和 var， Paddle 无此参数，暂无转写方式。  |
| dtype         |  -            | 输出数据类型， Paddle 无此参数, 需要转写。Paddle 应使用 astype 对计算结果做类型转换。  |
| -           |  weight_attr            | 可学习参数——权重的属性，默认为 None，表示使用默认可学习参数。 PyTorch 无此参数。 |
| -           |  bias_attr            | 可学习参数——偏差的属性，默认为 None，表示使用默认可学习参数。 PyTorch 无此参数。 |
| -           |  data_format            | 指定输入数据格式。 PyTorch 无此参数。 |

### 转写示例

#### num_features: 输入通道数
在 PyTorch 中，使用 `LazyInstanceNorm1d` 时可以不指定 `num_features`，它会在第一次前向传播时根据输入 Tensor 的形状自动确定；而在 Paddle 中，创建 `InstanceNorm1D` 时必须明确指定 `num_features` 参数，其值应与输入 Tensor 的通道数保持一致。
```python
# PyTorch 写法
bn = torch.nn.LazyInstanceNorm1d()
input = torch.randn(3, 5, 32)  # 5 是输入通道数
output = bn(input)  # 此时 num_features 会根据输入 Tensor 的形状自动设置为 5

# Paddle 写法
bn = paddle.nn.InstanceNorm1D(num_features=5)  # 需要明确指定 num_features
input = paddle.randn([3, 5, 32])  # 5 是输入通道数
output = bn(input)
```

#### affine：是否使用可学习的仿射参数
```python
# PyTorch 写法
IN = torch.nn.LazyInstanceNorm1d(eps=1e-05, momentum=0.1, affine=False)

# Paddle 写法
IN = paddle.nn.InstanceNorm1D(num_features, epsilon=1e-05, momentum=0.9, weight_attr=False, bias_attr=False) # 需要根据实际输入 Tensor 的通道数进行设置
```

#### dtype：输出数据类型
```python
# PyTorch 写法
IN = torch.nn.LazyInstanceNorm1d(eps=1e-05, momentum=0.1, affine=False， dtype=torch.float32)
y = IN(x)

# Paddle 写法
IN = paddle.nn.InstanceNorm1D(num_features, epsilon=1e-05, momentum=0.9, weight_attr=False, bias_attr=False) # 需要根据实际输入 Tensor 的通道数进行设置
y = IN(x).astype(paddle.float32)
```
