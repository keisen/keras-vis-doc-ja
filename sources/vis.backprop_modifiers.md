
**Source:** [vis/backprop_modifiers.py#L0](https://github.com/raghakot/keras-vis/tree/master/vis/backprop_modifiers.py#L0)

-------------------

### [guided](https://github.com/raghakot/keras-vis/tree/master/vis/backprop_modifiers.py#L7)

```python
guided(model)
```

正の活性化の正の勾配だけ伝播するよう誤差逆伝播を変更します。

*引数:*

 - **model**:  勾配計算の上書きが必要な`keras.models.Model` インスタンス

参照: Guided back propagatonの詳細は論文で見ることができます： [String For Simplicity: The All Convolutional Net](https://arxiv.org/pdf/1412.6806.pdf)

-------------------

### [deconv, rectified, relu](https://github.com/raghakot/keras-vis/tree/master/vis/backprop_modifiers.py#L20)

```python
deconv, rectified, relu(model)
```

正の勾配のみを伝搬するよう誤差逆伝播を変更します。

*引数:*

 - **model**:  勾配計算の上書きが必要な`keras.models.Model` インスタンス

参照: 詳細は論文で見ることができます： [Visualizing and Understanding Convolutional Networks](https://arxiv.org/pdf/1311.2901.pdf)

-------------------

### [get](https://github.com/raghakot/keras-vis/tree/master/vis/backprop_modifiers.py#L37)

```python
get(identifier)
```
