
**Source:** [vis/regularizers.py#L0](https://github.com/raghakot/keras-vis/tree/master/vis/regularizers.py#L0)

-------------------

### [normalize](https://github.com/raghakot/keras-vis/tree/master/vis/regularizers.py#L11)

```python
normalize(input_tensor, output_tensor)
```

`input_tensor`の次元に対して`output_tensor`を正規化します。
これは様々な入力画像の次元にわたって多かれ少なかれ均一な正則化重みを生成します。

*引数:*

 - **input_tensor**:  テンソル。形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`。
 - **output_tensor**:  正規化するテンソル

*戻り値:*

正規化されたテンソル

-------------------

## [TotalVariation](https://github.com/raghakot/keras-vis/tree/master/vis/regularizers.py#L27)

-------------------

### [TotalVariation.`__init__`](https://github.com/raghakot/keras-vis/tree/master/vis/regularizers.py#L29)

```python
__init__(self, img_input, beta=2.0)
```

TV正則化は自然画像に似た、より大胆で一貫した画像構造を促進します。
詳細は[Visualizing deep convolutional neural networks using natural pre-images](https://arxiv.org/pdf/1512.02017v3.pdf)の`section 3.2.2`を参照してください。

*引数:*

 - **img_input**:  画像テンソル。形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`。
 - **beta**:  小さい値ほどシャープであるが、より尖った画像が得られます。
   Values \(\in [1.5, 3.0]\) は合理的で妥当な値として推奨されます。（デフォルト値：2.0）

-------------------

### [TotalVariation.build_loss](https://github.com/raghakot/keras-vis/tree/master/vis/regularizers.py#L46)

```python
build_loss(self)
```

バッチ内の全ての画像についてTVを得るために、下記の関数のN次元バージョンを実装します。
$$TV^{\beta}(x) = \sum_{whc} \left ( \left ( x(h, w+1, c) - x(h, w, c) \right )^{2} +
\left ( x(h+1, w, c) - x(h, w, c) \right )^{2} \right )^{\frac{\beta}{2}}$$

-------------------

## [LPNorm](https://github.com/raghakot/keras-vis/tree/master/vis/regularizers.py#L75)

-------------------

### [LPNorm.`__init__`](https://github.com/raghakot/keras-vis/tree/master/vis/regularizers.py#L77)

```python
__init__(self, img_input, p=6.0)
```

Lpノルム関数を構築します。この正則化はピクセルの輝度の制限を促進します。
つまりピクセルがあまりに大きな値にならないようにします。

*引数:*

 - **img_input**:  モデルへの４次元の画像入力テンソル。形状は、`channels_first`の場合は`(samples, channels, rows, cols)`、
  `channels_last`の場合は`(samples, rows, cols, channels)`。
 - **p**:  p番目のノルムを指定
 
-------------------

### [LPNorm.build_loss](https://github.com/raghakot/keras-vis/tree/master/vis/regularizers.py#L94)

```python
build_loss(self)
```
