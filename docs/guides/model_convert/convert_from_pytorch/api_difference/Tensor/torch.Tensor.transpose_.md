## [ 输入参数用法不一致 ]torch.Tensor.transpose_

### [torch.Tensor.transpose_](https://pytorch.org/docs/stable/generated/torch.Tensor.transpose_.html)

```python
torch.Tensor.transpose_(dim0, dim1)
```

### [paddle.Tensor.transpose_]()

```python
paddle.Tensor.transpose_(perm, name=None)
```

PyTorch 的 `dim0, dim1` 与 Paddle 的 `perm` 用法不同，具体如下：
### 参数映射

| PyTorch       | PaddlePaddle | 备注                                                   |
| ------------- | ------------ | ------------------------------------------------------ |
| dim0, dim1 | perm | torch 的 dim0 与 dim1 表示要交换的两个维度, 为整数。 paddle 的 perm 表示重排的维度序列，为 list/tuple 。需要转写。|

### 转写示例
#### dim0, dim1: 表示要交换的两个维度
```python
# pytorch
x = torch.randn(2, 3, 5)
x.transpose_(0, 1)

# paddle
x = paddle.randn([2, 3, 5])
x.transpose_(perm=[1, 0, 2])
```